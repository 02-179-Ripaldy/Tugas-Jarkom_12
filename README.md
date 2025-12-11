# DOKUMENTASI SISTEM GYM BOOKING
## UAS PAW Kelompok 1

---

# 📌 1. ENDPOINT API

## 1.1 Authentication Endpoints

### **File:** `backend/app/views/auth_views.py`

### 📍 POST `/api/auth/register` - Register User Baru

**Deskripsi:** Mendaftarkan user baru (Admin, Trainer, atau Member)

**Potongan Kode:**
```python
@view_config(route_name='auth_register', renderer='json', request_method='POST')
def register(request):
    """Register new user"""
    try:
        data = request.json_body
        name = data.get('name')
        email = data.get('email')
        password = data.get('password')
        role = data.get('role', 'MEMBER')  # default: MEMBER
        membership_plan = data.get('membership_plan', 'Basic')
        
        # Validation
        if not all([name, email, password]):
            return Response(
                json.dumps({'status': 'error', 'message': 'Name, email, and password are required'}),
                status=400,
                content_type='application/json'
            )
        
        # Get database session
        db: Session = request.dbsession
        
        # Check if email already exists
        existing_user = db.query(User).filter(User.email == email).first()
        if existing_user:
            return Response(
                json.dumps({'status': 'error', 'message': 'Email already registered'}),
                status=400,
                content_type='application/json'
            )
        
        # Hash password
        hashed_password = hash_password(password)
        
        # Create user
        user_role = UserRole[role.upper()] if role.upper() in ['ADMIN', 'TRAINER', 'MEMBER'] else UserRole.MEMBER
        new_user = User(
            name=name,
            email=email,
            password=hashed_password,
            role=user_role
        )
        db.add(new_user)
        db.flush()  # Get user ID
        
        # If role is MEMBER, create member profile
        if user_role == UserRole.MEMBER:
            member_profile = Member(
                user_id=new_user.id,
                membership_plan=membership_plan,
                expiry_date=date.today() + timedelta(days=365)
            )
            db.add(member_profile)
        
        db.commit()
        
        # Create JWT token
        token = create_jwt_token(new_user.id, new_user.email, new_user.role.value)
        
        return {
            'status': 'success',
            'message': 'User registered successfully',
            'data': {
                'id': new_user.id,
                'name': new_user.name,
                'email': new_user.email,
                'role': new_user.role.value,
                'token': token
            }
        }
    except Exception as e:
        return Response(
            json.dumps({'status': 'error', 'message': str(e)}),
            status=500,
            content_type='application/json'
        )
```

**Testing di Postman:**
1. Method: `POST`
2. URL: `http://127.0.0.1:6543/api/auth/register`
3. Headers: `Content-Type: application/json`
4. Body (raw JSON):
```json
{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "role": "MEMBER",
    "membership_plan": "Premium"
}
```
5. Expected Response:
```json
{
    "status": "success",
    "message": "User registered successfully",
    "data": {
        "id": 5,
        "name": "John Doe",
        "email": "john@example.com",
        "role": "member",
        "token": "eyJ0eXAiOiJKV1QiLCJhbGc..."
    }
}
```

---

### 📍 POST `/api/auth/login` - Login User

**Deskripsi:** Login dan mendapatkan JWT token

**Potongan Kode:**
```python
@view_config(route_name='auth_login', renderer='json', request_method='POST')
def login(request):
    """Login user and return JWT token"""
    try:
        data = request.json_body
        email = data.get('email')
        password = data.get('password')
        
        if not all([email, password]):
            return Response(
                json.dumps({'status': 'error', 'message': 'Email and password are required'}),
                status=400,
                content_type='application/json'
            )
        
        db: Session = request.dbsession
        
        # Find user by email
        user = db.query(User).filter(User.email == email).first()
        if not user:
            return Response(
                json.dumps({'status': 'error', 'message': 'Invalid email or password'}),
                status=401,
                content_type='application/json'
            )
        
        # Verify password
        hashed_password = hash_password(password)
        if user.password != hashed_password:
            return Response(
                json.dumps({'status': 'error', 'message': 'Invalid email or password'}),
                status=401,
                content_type='application/json'
            )
        
        # Create JWT token
        token = create_jwt_token(user.id, user.email, user.role.value)
        
        # Get member_id if user is MEMBER
        member_id = None
        if user.role == UserRole.MEMBER and user.member:
            member_id = user.member.id
        
        return {
            'status': 'success',
            'message': 'Login successful',
            'data': {
                'user': {
                    'id': user.id,
                    'name': user.name,
                    'email': user.email,
                    'role': user.role.value,
                    'member_id': member_id
                },
                'token': token
            }
        }
    except Exception as e:
        return Response(
            json.dumps({'status': 'error', 'message': str(e)}),
            status=500,
            content_type='application/json'
        )
```

**Testing di Postman:**
1. Method: `POST`
2. URL: `http://127.0.0.1:6543/api/auth/login`
3. Headers: `Content-Type: application/json`
4. Body (raw JSON):
```json
{
    "email": "member@gym.com",
    "password": "member123"
}
```
5. Expected Response:
```json
{
    "status": "success",
    "message": "Login successful",
    "data": {
        "user": {
            "id": 3,
            "name": "Member User",
            "email": "member@gym.com",
            "role": "member",
            "member_id": 1
        },
        "token": "eyJ0eXAiOiJKV1QiLCJhbGc..."
    }
}
```

---

## 1.2 Class Management Endpoints

### **File:** `backend/app/views/class_views.py`

### 📍 GET `/api/classes` - List Semua Classes

**Deskripsi:** Mendapatkan daftar semua gym classes dengan informasi booked count

**Potongan Kode:**
```python
from sqlalchemy import func

@view_config(route_name='api_classes', renderer='json', request_method='GET')
def get_classes(request):
    """Get all classes"""
    try:
        db: Session = request.dbsession
        
        # Query classes dengan jumlah booking menggunakan LEFT JOIN dan agregasi
        classes = db.query(
            Class,
            func.count(Booking.id).label('booked_count')
        ).outerjoin(
            Booking, Class.id == Booking.class_id
        ).group_by(Class.id).all()
        
        # Format data
        classes_data = []
        for gym_class, booked_count in classes:
            class_dict = gym_class.to_dict()
            class_dict['booked_count'] = booked_count
            class_dict['available_slots'] = gym_class.capacity - booked_count
            classes_data.append(class_dict)
        
        return {
            'status': 'success',
            'data': classes_data,
            'count': len(classes_data)
        }
    except Exception as e:
        return Response(
            json.dumps({'status': 'error', 'message': str(e)}),
            status=500,
            content_type='application/json'
        )
```

**Testing di Postman:**
1. Method: `GET`
2. URL: `http://127.0.0.1:6543/api/classes`
3. Headers: (tidak perlu)
4. Expected Response:
```json
{
    "status": "success",
    "data": [
        {
            "id": 1,
            "trainer_id": 2,
            "name": "Morning Yoga",
            "description": "Start your day with yoga",
            "schedule": "2024-12-20T07:00:00",
            "capacity": 20,
            "booked_count": 2,
            "available_slots": 18,
            "trainer": {
                "id": 2,
                "name": "Trainer User",
                "email": "trainer@gym.com",
                "role": "trainer"
            }
        }
    ],
    "count": 1
}
```

---

### 📍 POST `/api/classes` - Create Class Baru

**Deskripsi:** Membuat class baru (Trainer/Admin only)

**Potongan Kode:**
```python
@view_config(route_name='api_classes', renderer='json', request_method='POST')
def create_class(request):
    """Create new class (Trainer/Admin only)"""
    try:
        db: Session = request.dbsession
        data = request.json_body
        
        # Validation
        required_fields = ['trainer_id', 'name', 'schedule', 'capacity']
        if not all(field in data for field in required_fields):
            return Response(
                json.dumps({'status': 'error', 'message': 'Missing required fields'}),
                status=400,
                content_type='application/json'
            )
        
        # Check if trainer exists
        trainer = db.query(User).filter(
            User.id == data['trainer_id'],
            User.role == UserRole.TRAINER
        ).first()
        
        if not trainer:
            return Response(
                json.dumps({'status': 'error', 'message': 'Trainer not found'}),
                status=404,
                content_type='application/json'
            )
        
        # Parse schedule datetime
        schedule_str = data['schedule']
        schedule = datetime.fromisoformat(schedule_str.replace('Z', '+00:00'))
        
        # Create new class
        new_class = Class(
            trainer_id=data['trainer_id'],
            name=data['name'],
            description=data.get('description', ''),
            schedule=schedule,
            capacity=data['capacity']
        )
        
        db.add(new_class)
        db.commit()
        db.refresh(new_class)
        
        return {
            'status': 'success',
            'message': 'Class created successfully',
            'data': new_class.to_dict()
        }
    except Exception as e:
        return Response(
            json.dumps({'status': 'error', 'message': str(e)}),
            status=500,
            content_type='application/json'
        )
```

**Testing di Postman:**
1. Method: `POST`
2. URL: `http://127.0.0.1:6543/api/classes`
3. Headers: `Content-Type: application/json`
4. Body (raw JSON):
```json
{
    "trainer_id": 2,
    "name": "Evening Pilates",
    "description": "Relaxing pilates session",
    "schedule": "2024-12-20T18:00:00",
    "capacity": 15
}
```
5. Expected Response:
```json
{
    "status": "success",
    "message": "Class created successfully",
    "data": {
        "id": 6,
        "trainer_id": 2,
        "name": "Evening Pilates",
        "description": "Relaxing pilates session",
        "schedule": "2024-12-20T18:00:00",
        "capacity": 15,
        "booked_count": 0,
        "available_slots": 15
    }
}
```

---

## 1.3 Booking Endpoints

### **File:** `backend/app/views/booking_views.py`

### 📍 POST `/api/bookings` - Create Booking

**Deskripsi:** Member melakukan booking ke suatu class

**Potongan Kode:**
```python
@view_config(route_name='api_bookings', renderer='json', request_method='POST')
def create_booking(request):
    """Create new booking (Member only)"""
    try:
        db: Session = request.dbsession
        data = request.json_body
        
        # Validation
        if 'class_id' not in data or 'member_id' not in data:
            return Response(
                json.dumps({'status': 'error', 'message': 'class_id and member_id are required'}),
                status=400,
                content_type='application/json'
            )
        
        class_id = data['class_id']
        member_id = data['member_id']
        
        # Check if class exists
        gym_class = db.query(Class).filter(Class.id == class_id).first()
        if not gym_class:
            return Response(
                json.dumps({'status': 'error', 'message': 'Class not found'}),
                status=404,
                content_type='application/json'
            )
        
        # Check if member exists
        member = db.query(Member).filter(Member.id == member_id).first()
        if not member:
            return Response(
                json.dumps({'status': 'error', 'message': 'Member not found'}),
                status=404,
                content_type='application/json'
            )
        
        # Check if class is full
        booked_count = db.query(func.count(Booking.id)).filter(
            Booking.class_id == class_id
        ).scalar() or 0
        
        if booked_count >= gym_class.capacity:
            return Response(
                json.dumps({'status': 'error', 'message': 'Class is full'}),
                status=400,
                content_type='application/json'
            )
        
        # Check if member already booked this class
        existing_booking = db.query(Booking).filter(
            Booking.member_id == member_id,
            Booking.class_id == class_id
        ).first()
        
        if existing_booking:
            return Response(
                json.dumps({'status': 'error', 'message': 'You have already booked this class'}),
                status=400,
                content_type='application/json'
            )
        
        # Create booking
        new_booking = Booking(
            member_id=member_id,
            class_id=class_id,
            booking_date=datetime.utcnow()
        )
        
        db.add(new_booking)
        db.commit()
        db.refresh(new_booking)
        
        return {
            'status': 'success',
            'message': 'Class booked successfully',
            'data': {
                'id': new_booking.id,
                'member_id': new_booking.member_id,
                'class_id': new_booking.class_id,
                'booking_date': new_booking.booking_date.isoformat()
            }
        }
    except Exception as e:
        return Response(
            json.dumps({'status': 'error', 'message': str(e)}),
            status=500,
            content_type='application/json'
        )
```

**Testing di Postman:**
1. Method: `POST`
2. URL: `http://127.0.0.1:6543/api/bookings`
3. Headers: `Content-Type: application/json`
4. Body (raw JSON):
```json
{
    "class_id": 1,
    "member_id": 1
}
```
5. Expected Response:
```json
{
    "status": "success",
    "message": "Class booked successfully",
    "data": {
        "id": 3,
        "member_id": 1,
        "class_id": 1,
        "booking_date": "2024-12-11T10:30:00"
    }
}
```

---

## 1.4 Attendance Endpoints

### **File:** `backend/app/views/attendance_views.py`

### 📍 POST `/api/attendance` - Mark Attendance

**Deskripsi:** Mark kehadiran member di class (Trainer/Admin only)

**Potongan Kode:**
```python
@view_config(route_name='api_attendance', renderer='json', request_method='POST')
def mark_attendance(request):
    """Mark attendance for a booking (Trainer/Admin only)"""
    try:
        db: Session = request.dbsession
        data = request.json_body
        
        # Validation
        if 'booking_id' not in data or 'attended' not in data:
            return Response(
                json.dumps({'status': 'error', 'message': 'booking_id and attended are required'}),
                status=400,
                content_type='application/json'
            )
        
        booking_id = data['booking_id']
        attended = data['attended']
        
        # Check if booking exists
        booking = db.query(Booking).filter(Booking.id == booking_id).first()
        if not booking:
            return Response(
                json.dumps({'status': 'error', 'message': 'Booking not found'}),
                status=404,
                content_type='application/json'
            )
        
        # Check if attendance already exists
        existing_attendance = db.query(Attendance).filter(
            Attendance.booking_id == booking_id
        ).first()
        
        if existing_attendance:
            # Update existing attendance
            existing_attendance.attended = attended
            db.commit()
            db.refresh(existing_attendance)
            
            return {
                'status': 'success',
                'message': 'Attendance updated successfully',
                'data': existing_attendance.to_dict()
            }
        else:
            # Create new attendance record
            new_attendance = Attendance(
                booking_id=booking_id,
                attended=attended
            )
            
            db.add(new_attendance)
            db.commit()
            db.refresh(new_attendance)
            
            return {
                'status': 'success',
                'message': 'Attendance marked successfully',
                'data': new_attendance.to_dict()
            }
    except Exception as e:
        return Response(
            json.dumps({'status': 'error', 'message': str(e)}),
            status=500,
            content_type='application/json'
        )
```

**Testing di Postman:**
1. Method: `POST`
2. URL: `http://127.0.0.1:6543/api/attendance`
3. Headers: `Content-Type: application/json`
4. Body (raw JSON):
```json
{
    "booking_id": 1,
    "attended": true
}
```
5. Expected Response:
```json
{
    "status": "success",
    "message": "Attendance marked successfully",
    "data": {
        "id": 1,
        "booking_id": 1,
        "attended": true,
        "created_at": "2024-12-11T10:35:00"
    }
}
```

---

# 📌 2. BASIS DATA (DATABASE)

## 2.1 Struktur Database

**Database:** PostgreSQL 18  
**Database Name:** `gym_booking_db`  
**Connection:** `postgresql://postgres:ripaldy@localhost/gym_booking_db`

## 2.2 Model: Users

### **File:** `backend/app/models/user.py`

**Deskripsi:** Menyimpan data user (Admin, Trainer, Member)

**Potongan Kode:**
```python
from sqlalchemy import Column, Integer, String, DateTime, Enum
from sqlalchemy.orm import relationship
from datetime import datetime
import enum

from . import Base


class UserRole(enum.Enum):
    ADMIN = "admin"
    TRAINER = "trainer"
    MEMBER = "member"


class User(Base):
    __tablename__ = 'users'
    
    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)
    email = Column(String(100), unique=True, nullable=False, index=True)
    password = Column(String(255), nullable=False)
    role = Column(Enum(UserRole), nullable=False, default=UserRole.MEMBER)
    created_at = Column(DateTime, default=datetime.utcnow)
    updated_at = Column(DateTime, default=datetime.utcnow, onupdate=datetime.utcnow)
    
    # Relationships
    member = relationship("Member", back_populates="user", uselist=False, cascade="all, delete-orphan")
    trainer_classes = relationship("Class", back_populates="trainer", foreign_keys="Class.trainer_id")

    def __repr__(self):
        return f"<User(id={self.id}, name='{self.name}', email='{self.email}', role='{self.role.value}')>"
    
    def to_dict(self):
        return {
            'id': self.id,
            'name': self.name,
            'email': self.email,
            'role': self.role.value,
            'created_at': self.created_at.isoformat() if self.created_at else None
        }
```

**SQL Equivalent:**
```sql
CREATE TYPE user_role AS ENUM ('admin', 'trainer', 'member');

CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    role user_role NOT NULL DEFAULT 'member',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);
```

**Fitur:**
- **Primary Key:** `id` (auto-increment)
- **Unique Constraint:** `email` (tidak boleh duplikat)
- **Index:** `email` (untuk pencarian cepat)
- **Enum:** `role` (ADMIN, TRAINER, MEMBER)
- **Relationships:**
  - `member`: One-to-One dengan tabel `members`
  - `trainer_classes`: One-to-Many dengan tabel `classes`

---

## 2.3 Model: Members

### **File:** `backend/app/models/member.py`

**Deskripsi:** Menyimpan profil member dengan membership plan

**Potongan Kode:**
```python
from sqlalchemy import Column, Integer, String, Date, ForeignKey, DateTime
from sqlalchemy.orm import relationship
from datetime import datetime

from . import Base


class Member(Base):
    __tablename__ = 'members'
    
    id = Column(Integer, primary_key=True)
    user_id = Column(Integer, ForeignKey('users.id', ondelete='CASCADE'), nullable=False, unique=True)
    membership_plan = Column(String(50), nullable=False, default='Basic')
    expiry_date = Column(Date, nullable=False)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    # Relationships
    user = relationship("User", back_populates="member")
    bookings = relationship("Booking", back_populates="member", cascade="all, delete-orphan")
    
    def __repr__(self):
        return f"<Member(id={self.id}, user_id={self.user_id}, plan='{self.membership_plan}')>"
    
    def to_dict(self):
        return {
            'id': self.id,
            'user_id': self.user_id,
            'user': self.user.to_dict() if self.user else None,
            'membership_plan': self.membership_plan,
            'expiry_date': self.expiry_date.isoformat() if self.expiry_date else None,
            'created_at': self.created_at.isoformat() if self.created_at else None
        }
```

**SQL Equivalent:**
```sql
CREATE TABLE members (
    id SERIAL PRIMARY KEY,
    user_id INTEGER UNIQUE NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    membership_plan VARCHAR(50) NOT NULL DEFAULT 'Basic',
    expiry_date DATE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Fitur:**
- **Foreign Key:** `user_id` → `users.id` (ON DELETE CASCADE)
- **Unique Constraint:** `user_id` (satu user hanya punya satu member profile)
- **Membership Plans:** Basic, Premium, VIP
- **Relationships:**
  - `user`: Many-to-One dengan tabel `users`
  - `bookings`: One-to-Many dengan tabel `bookings`

---

## 2.4 Model: Classes

### **File:** `backend/app/models/gym_class.py`

**Deskripsi:** Menyimpan data gym classes (Yoga, HIIT, dll)

**Potongan Kode:**
```python
from sqlalchemy import Column, Integer, String, Text, DateTime, ForeignKey
from sqlalchemy.orm import relationship
from datetime import datetime

from . import Base


class Class(Base):
    __tablename__ = 'classes'
    
    id = Column(Integer, primary_key=True)
    trainer_id = Column(Integer, ForeignKey('users.id', ondelete='CASCADE'), nullable=False)
    name = Column(String(100), nullable=False)
    description = Column(Text)
    schedule = Column(DateTime, nullable=False)
    capacity = Column(Integer, nullable=False, default=20)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    # Relationships
    trainer = relationship("User", back_populates="trainer_classes", foreign_keys=[trainer_id])
    bookings = relationship("Booking", back_populates="gym_class", cascade="all, delete-orphan")
    
    def __repr__(self):
        return f"<Class(id={self.id}, name='{self.name}', schedule='{self.schedule}')>"
    
    def to_dict(self, include_bookings=False):
        data = {
            'id': self.id,
            'trainer_id': self.trainer_id,
            'name': self.name,
            'description': self.description,
            'schedule': self.schedule.isoformat() if self.schedule else None,
            'capacity': self.capacity,
            'booked_count': len(self.bookings) if self.bookings else 0,
            'available_slots': self.capacity - (len(self.bookings) if self.bookings else 0),
            'trainer': self.trainer.to_dict() if self.trainer else None,
            'created_at': self.created_at.isoformat() if self.created_at else None
        }
        if include_bookings:
            data['bookings'] = [b.to_dict(include_member=True) for b in self.bookings]
        return data
    
    def is_full(self):
        """Check if class is at full capacity"""
        return len(self.bookings) >= self.capacity
```

**SQL Equivalent:**
```sql
CREATE TABLE classes (
    id SERIAL PRIMARY KEY,
    trainer_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    schedule TIMESTAMP NOT NULL,
    capacity INTEGER NOT NULL DEFAULT 20,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Fitur:**
- **Foreign Key:** `trainer_id` → `users.id` (ON DELETE CASCADE)
- **Capacity Management:** Maksimal peserta per class
- **Method:** `is_full()` untuk cek apakah class penuh
- **Relationships:**
  - `trainer`: Many-to-One dengan tabel `users`
  - `bookings`: One-to-Many dengan tabel `bookings`

---

## 2.5 Model: Bookings

### **File:** `backend/app/models/booking.py`

**Deskripsi:** Menyimpan data booking member ke class

**Potongan Kode:**
```python
from sqlalchemy import Column, Integer, DateTime, ForeignKey, UniqueConstraint
from sqlalchemy.orm import relationship
from datetime import datetime

from . import Base


class Booking(Base):
    __tablename__ = 'bookings'
    __table_args__ = (
        UniqueConstraint('member_id', 'class_id', name='unique_member_class_booking'),
    )
    
    id = Column(Integer, primary_key=True)
    member_id = Column(Integer, ForeignKey('members.id', ondelete='CASCADE'), nullable=False)
    class_id = Column(Integer, ForeignKey('classes.id', ondelete='CASCADE'), nullable=False)
    booking_date = Column(DateTime, default=datetime.utcnow)
    
    # Relationships
    member = relationship("Member", back_populates="bookings")
    gym_class = relationship("Class", back_populates="bookings")
    attendance = relationship("Attendance", back_populates="booking", uselist=False, cascade="all, delete-orphan")
    
    def __repr__(self):
        return f"<Booking(id={self.id}, member_id={self.member_id}, class_id={self.class_id})>"
    
    def to_dict(self, include_member=False, include_class=False):
        data = {
            'id': self.id,
            'member_id': self.member_id,
            'class_id': self.class_id,
            'booking_date': self.booking_date.isoformat() if self.booking_date else None
        }
        if include_member and self.member:
            data['member'] = self.member.to_dict()
        if include_class and self.gym_class:
            data['class'] = self.gym_class.to_dict()
        if self.attendance:
            data['attendance'] = self.attendance.to_dict()
        return data
```

**SQL Equivalent:**
```sql
CREATE TABLE bookings (
    id SERIAL PRIMARY KEY,
    member_id INTEGER NOT NULL REFERENCES members(id) ON DELETE CASCADE,
    class_id INTEGER NOT NULL REFERENCES classes(id) ON DELETE CASCADE,
    booking_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT unique_member_class_booking UNIQUE (member_id, class_id)
);
```

**Fitur:**
- **Unique Constraint:** `(member_id, class_id)` - member tidak bisa booking class yang sama 2x
- **Foreign Keys:**
  - `member_id` → `members.id` (ON DELETE CASCADE)
  - `class_id` → `classes.id` (ON DELETE CASCADE)
- **Relationships:**
  - `member`: Many-to-One dengan tabel `members`
  - `gym_class`: Many-to-One dengan tabel `classes`
  - `attendance`: One-to-One dengan tabel `attendance`

---

## 2.6 Model: Attendance

### **File:** `backend/app/models/attendance.py`

**Deskripsi:** Tracking kehadiran member di class

**Potongan Kode:**
```python
from sqlalchemy import Column, Integer, Boolean, DateTime, ForeignKey
from sqlalchemy.orm import relationship
from datetime import datetime

from . import Base


class Attendance(Base):
    __tablename__ = 'attendance'
    
    id = Column(Integer, primary_key=True)
    booking_id = Column(Integer, ForeignKey('bookings.id', ondelete='CASCADE'), nullable=False, unique=True)
    attended = Column(Boolean, default=False)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    # Relationships
    booking = relationship("Booking", back_populates="attendance")
    
    def __repr__(self):
        return f"<Attendance(id={self.id}, booking_id={self.booking_id}, attended={self.attended})>"
    
    def to_dict(self):
        return {
            'id': self.id,
            'booking_id': self.booking_id,
            'attended': self.attended,
            'created_at': self.created_at.isoformat() if self.created_at else None
        }
```

**SQL Equivalent:**
```sql
CREATE TABLE attendance (
    id SERIAL PRIMARY KEY,
    booking_id INTEGER UNIQUE NOT NULL REFERENCES bookings(id) ON DELETE CASCADE,
    attended BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Fitur:**
- **Foreign Key:** `booking_id` → `bookings.id` (ON DELETE CASCADE)
- **Unique Constraint:** `booking_id` (satu booking hanya punya satu attendance record)
- **Boolean Field:** `attended` (TRUE/FALSE)
- **Relationships:**
  - `booking`: Many-to-One dengan tabel `bookings`

---

## 2.7 Entity Relationship Diagram (ERD)

```
┌─────────────────┐
│     USERS       │
├─────────────────┤
│ PK id           │
│    name         │
│    email (UQ)   │
│    password     │
│    role (ENUM)  │
│    created_at   │
└────────┬────────┘
         │ 1
         │
         │ 1
┌────────▼────────┐          ┌─────────────────┐
│    MEMBERS      │          │    CLASSES      │
├─────────────────┤          ├─────────────────┤
│ PK id           │          │ PK id           │
│ FK user_id (UQ) │          │ FK trainer_id   │
│    membership   │          │    name         │
│    expiry_date  │          │    description  │
│    created_at   │          │    schedule     │
└────────┬────────┘          │    capacity     │
         │ 1                 │    created_at   │
         │                   └────────┬────────┘
         │                            │ 1
         │ *                          │
         │    ┌─────────────────┐    │ *
         └────►    BOOKINGS     ◄────┘
              ├─────────────────┤
              │ PK id           │
              │ FK member_id    │
              │ FK class_id     │
              │    booking_date │
              │ UQ (member,cls) │
              └────────┬────────┘
                       │ 1
                       │
                       │ 1
              ┌────────▼────────┐
              │   ATTENDANCE    │
              ├─────────────────┤
              │ PK id           │
              │ FK booking_id   │
              │    attended     │
              │    created_at   │
              └─────────────────┘
```

**Relasi:**
- `users` ↔ `members`: One-to-One (via `user_id`)
- `users` ↔ `classes`: One-to-Many (via `trainer_id`)
- `members` ↔ `bookings`: One-to-Many (via `member_id`)
- `classes` ↔ `bookings`: One-to-Many (via `class_id`)
- `bookings` ↔ `attendance`: One-to-One (via `booking_id`)

---

# 📌 3. RANCANG APLIKASI SISI SERVER

## 3.1 Arsitektur Aplikasi

**Framework:** Pyramid (Python WSGI Framework)  
**ORM:** SQLAlchemy  
**Database:** PostgreSQL 18  
**Authentication:** JWT (JSON Web Tokens)

## 3.2 Konfigurasi Utama

### **File:** `backend/app/__init__.py`

**Deskripsi:** Main configuration untuk Pyramid application

**Potongan Kode:**

### 🔧 CORS Configuration

```python
def cors_tween_factory(handler, registry):
    """
    CORS tween untuk menambahkan headers ke semua responses
    Memungkinkan frontend React berkomunikasi dengan backend API
    """
    def cors_tween(request):
        response = handler(request)
        response.headers.update({
            'Access-Control-Allow-Origin': '*',
            'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
            'Access-Control-Allow-Headers': 'Content-Type, Authorization',
            'Access-Control-Max-Age': '3600'
        })
        return response
    return cors_tween
```

**Penjelasan:**
- **Access-Control-Allow-Origin: \***: Mengizinkan request dari domain mana saja
- **Access-Control-Allow-Methods**: HTTP methods yang diizinkan
- **Access-Control-Allow-Headers**: Headers yang diizinkan dalam request
- **Access-Control-Max-Age**: Cache preflight request selama 1 jam

---

### 🗄️ Database Session Management

```python
def main(global_config, **settings):
    """This function returns a Pyramid WSGI application."""
    config = Configurator(settings=settings)
    
    # Add CORS tween
    config.add_tween('app.cors_tween_factory')
    
    # Membuat engine dari konfigurasi di development.ini
    # Connection string: postgresql://postgres:ripaldy@localhost/gym_booking_db
    engine = engine_from_config(settings, 'sqlalchemy.')
    
    # Session factory untuk membuat database session
    session_factory = sessionmaker(bind=engine)
    
    # Function untuk mendapatkan database session per request
    def get_db(request):
        session = session_factory()
        
        # Cleanup callback: Otomatis close session setelah request selesai
        # Ini mencegah memory leak dan connection pool exhaustion
        def cleanup(request):
            session.close()
        request.add_finished_callback(cleanup)
        return session
    
    # Daftarkan method 'dbsession' ke request object
    # Cara pakai: db = request.dbsession
    config.add_request_method(get_db, 'dbsession', reify=True)
    
    # ... route definitions ...
    
    return config.make_wsgi_app()
```

**Penjelasan:**
- **Request-scoped session**: Setiap HTTP request mendapat database session sendiri
- **Automatic cleanup**: Session otomatis di-close setelah request selesai
- **reify=True**: Session di-cache per request (hanya dibuat sekali)
- **Cara pakai:** `db = request.dbsession` di view functions

---

### 🛣️ Route Definitions (21 Endpoints)

```python
    # CORS OPTIONS route (catch-all untuk CORS preflight request)
    config.add_route('options', '/*path', request_method='OPTIONS')
    
    # === KATEGORI 1: AUTHENTICATION (4 endpoints) ===
    config.add_route('home', '/')                              # GET / - Home
    config.add_route('auth_register', '/api/auth/register')    # POST - Register
    config.add_route('auth_login', '/api/auth/login')          # POST - Login
    config.add_route('auth_logout', '/api/auth/logout')        # POST - Logout
    config.add_route('auth_me', '/api/auth/me')                # GET - Current user
    
    # === KATEGORI 2: CLASSES (5 endpoints) ===
    config.add_route('api_classes', '/api/classes')                        # GET/POST
    config.add_route('api_class', '/api/classes/{id}')                     # GET/PUT/DELETE
    config.add_route('api_class_participants', '/api/classes/{id}/participants')  # GET
    
    # === KATEGORI 3: BOOKINGS (5 endpoints) ===
    config.add_route('api_bookings', '/api/bookings')          # GET/POST
    config.add_route('api_booking', '/api/bookings/{id}')      # GET/DELETE
    config.add_route('api_my_bookings', '/api/bookings/my')    # GET
    
    # === KATEGORI 4: ATTENDANCE (4 endpoints) ===
    config.add_route('api_attendance', '/api/attendance')      # GET/POST
    config.add_route('api_my_attendance', '/api/attendance/my') # GET
    
    # === KATEGORI 5: MEMBERSHIP (3 endpoints) ===
    config.add_route('api_membership_plans', '/api/membership/plans')  # GET
    config.add_route('api_my_membership', '/api/membership/my')        # GET
    config.add_route('api_members', '/api/members')                    # GET/POST
    
    # Scan views
    config.scan('.views')
```

**Penjelasan:**
- **RESTful Design**: URL mengikuti convention REST API
- **Dynamic Segments**: `{id}` untuk parameter dinamis
- **Multiple Methods**: Satu route bisa handle GET/POST/PUT/DELETE

---

## 3.3 Authentication System

### **File:** `backend/app/views/auth_views.py`

### 🔐 Password Hashing

```python
import hashlib

def hash_password(password):
    """Hash password menggunakan SHA256"""
    return hashlib.sha256(password.encode()).hexdigest()
```

**Penjelasan:**
- Menggunakan SHA256 untuk hash password
- Password tidak disimpan dalam bentuk plain text
- Password di-hash sebelum disimpan ke database

---

### 🎫 JWT Token Generation

```python
import jwt
from datetime import datetime, timedelta

JWT_SECRET = "your-secret-key-change-this-in-production"
JWT_ALGORITHM = "HS256"
JWT_EXP_DELTA_SECONDS = 3600  # 1 hour

def create_jwt_token(user_id, email, role):
    """Create JWT token for authenticated user"""
    payload = {
        'user_id': user_id,
        'email': email,
        'role': role,
        'exp': datetime.utcnow() + timedelta(seconds=JWT_EXP_DELTA_SECONDS)
    }
    token = jwt.encode(payload, JWT_SECRET, algorithm=JWT_ALGORITHM)
    return token
```

**Penjelasan:**
- **Payload:** Berisi `user_id`, `email`, `role`, dan `exp` (expiration)
- **Algorithm:** HS256 (HMAC with SHA-256)
- **Expiration:** Token berlaku selama 1 jam
- **Secret Key:** Digunakan untuk sign dan verify token

**Contoh Token:**
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjozLCJlbWFpbCI6Im1lbWJlckBneW0uY29tIiwicm9sZSI6Im1lbWJlciIsImV4cCI6MTcwMjMxMjgwMH0.abc123def456
```

---

## 3.4 Database Query Patterns

### Pattern 1: Simple Query

```python
# Get all users
users = db.query(User).all()

# Get user by ID
user = db.query(User).filter(User.id == user_id).first()

# Get user by email
user = db.query(User).filter(User.email == email).first()
```

---

### Pattern 2: Query dengan JOIN

```python
from sqlalchemy import func

# Get classes dengan jumlah booking (LEFT JOIN + COUNT)
classes = db.query(
    Class,
    func.count(Booking.id).label('booked_count')
).outerjoin(
    Booking, Class.id == Booking.class_id
).group_by(Class.id).all()

# Hasilnya: [(Class object, booked_count), ...]
for gym_class, booked_count in classes:
    print(f"Class: {gym_class.name}, Booked: {booked_count}")
```

---

### Pattern 3: Query dengan Multiple Filters

```python
# Get bookings by member_id dan class_id
existing_booking = db.query(Booking).filter(
    Booking.member_id == member_id,
    Booking.class_id == class_id
).first()

# Get trainer classes
trainer_classes = db.query(Class).filter(
    Class.trainer_id == trainer_id
).all()
```

---

### Pattern 4: Create, Update, Delete

```python
# CREATE
new_user = User(
    name="John Doe",
    email="john@example.com",
    password=hashed_password,
    role=UserRole.MEMBER
)
db.add(new_user)
db.flush()  # Get ID tanpa commit
db.commit()
db.refresh(new_user)  # Refresh object dari database

# UPDATE
user.name = "Jane Doe"
db.commit()

# DELETE
db.delete(user)
db.commit()
```

---

## 3.5 Error Handling Pattern

```python
from pyramid.response import Response
import json

try:
    # Business logic here
    result = perform_operation()
    
    return {
        'status': 'success',
        'message': 'Operation successful',
        'data': result
    }
except ValueError as e:
    # Validation error (400 Bad Request)
    return Response(
        json.dumps({'status': 'error', 'message': str(e)}),
        status=400,
        content_type='application/json'
    )
except NotFoundError as e:
    # Resource not found (404 Not Found)
    return Response(
        json.dumps({'status': 'error', 'message': str(e)}),
        status=404,
        content_type='application/json'
    )
except Exception as e:
    # Internal server error (500)
    return Response(
        json.dumps({'status': 'error', 'message': str(e)}),
        status=500,
        content_type='application/json'
    )
```

**HTTP Status Codes:**
- **200 OK**: Request berhasil
- **400 Bad Request**: Validation error
- **401 Unauthorized**: Authentication gagal
- **404 Not Found**: Resource tidak ditemukan
- **500 Internal Server Error**: Server error

---

## 3.6 Validation Pattern

```python
# Validation di create_booking
def create_booking(request):
    db: Session = request.dbsession
    data = request.json_body
    
    # Layer 1: Check required fields
    if 'class_id' not in data or 'member_id' not in data:
        return Response(
            json.dumps({'status': 'error', 'message': 'class_id and member_id are required'}),
            status=400,
            content_type='application/json'
        )
    
    # Layer 2: Check if class exists
    gym_class = db.query(Class).filter(Class.id == class_id).first()
    if not gym_class:
        return Response(
            json.dumps({'status': 'error', 'message': 'Class not found'}),
            status=404,
            content_type='application/json'
        )
    
    # Layer 3: Check if member exists
    member = db.query(Member).filter(Member.id == member_id).first()
    if not member:
        return Response(
            json.dumps({'status': 'error', 'message': 'Member not found'}),
            status=404,
            content_type='application/json'
        )
    
    # Layer 4: Business logic validation (capacity check)
    booked_count = db.query(func.count(Booking.id)).filter(
        Booking.class_id == class_id
    ).scalar() or 0
    
    if booked_count >= gym_class.capacity:
        return Response(
            json.dumps({'status': 'error', 'message': 'Class is full'}),
            status=400,
            content_type='application/json'
        )
    
    # Layer 5: Check duplicate booking
    existing_booking = db.query(Booking).filter(
        Booking.member_id == member_id,
        Booking.class_id == class_id
    ).first()
    
    if existing_booking:
        return Response(
            json.dumps({'status': 'error', 'message': 'You have already booked this class'}),
            status=400,
            content_type='application/json'
        )
    
    # All validations passed, create booking
    new_booking = Booking(...)
    db.add(new_booking)
    db.commit()
```

**Validation Layers:**
1. **Input Validation**: Check required fields
2. **Foreign Key Validation**: Check if related entities exist
3. **Business Logic Validation**: Check capacity, expiry date, etc.
4. **Uniqueness Validation**: Check duplicate bookings
5. **Authorization Validation**: Check user permissions

---

## 3.7 Cara Menjalankan Server

### **File:** `backend/development.ini`

**Konfigurasi:**
```ini
[app:main]
use = egg:gym-booking-backend

pyramid.reload_templates = true
pyramid.debug_authorization = false
pyramid.debug_notfound = false
pyramid.debug_routematch = false
pyramid.default_locale_name = en

sqlalchemy.url = postgresql://postgres:ripaldy@localhost/gym_booking_db

[server:main]
use = egg:waitress#main
listen = 127.0.0.1:6543
```

**Command untuk menjalankan:**
```powershell
cd C:\Users\User\uas_pengweb\backend
C:/Users/User/uas_pengweb/.venv/Scripts/pserve.exe development.ini --reload
```

**Output:**
```
Starting subprocess with file monitor
Starting server in PID 12345.
Serving on http://127.0.0.1:6543
```

---

## 3.8 Database Connection Configuration

**Connection String:**
```
postgresql://postgres:ripaldy@localhost/gym_booking_db
```

**Format:**
```
postgresql://<username>:<password>@<host>/<database_name>
```

**Components:**
- **Username:** `postgres`
- **Password:** `ripaldy`
- **Host:** `localhost` (127.0.0.1)
- **Database:** `gym_booking_db`
- **Port:** Default PostgreSQL port (5432)

---

# 📌 RANGKUMAN

## Total Endpoints: 21
- **Authentication:** 4 endpoints
- **Classes:** 5 endpoints
- **Bookings:** 5 endpoints
- **Attendance:** 4 endpoints
- **Membership:** 3 endpoints

## Total Database Tables: 5
- **users** (dengan enum UserRole)
- **members** (One-to-One dengan users)
- **classes** (One-to-Many dengan users via trainer_id)
- **bookings** (Many-to-Many junction table)
- **attendance** (One-to-One dengan bookings)

## Fitur Server:
- ✅ CORS enabled untuk komunikasi frontend-backend
- ✅ Request-scoped database session management
- ✅ JWT authentication dengan expiration 1 jam
- ✅ SHA256 password hashing
- ✅ Multi-layer validation
- ✅ RESTful API design
- ✅ Error handling dengan HTTP status codes
- ✅ SQLAlchemy ORM dengan relationships
- ✅ Cascade delete untuk data integrity

---

**Dibuat untuk:** UAS PAW Kelompok 1  
**Tanggal:** 11 Desember 2024  
**Database:** PostgreSQL 18  
**Backend:** Python Pyramid Framework  
**Frontend:** React + Vite

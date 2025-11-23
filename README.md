# Tugas Bab 2 – Application Layer  
Buku: *Computer Networking: A Top-Down Approach*  
Halaman: 196–205  

Nama: Ripaldy Saputra Lumbantoruan  
NIM: 123140179  
Kelas: RC  

---

## A. Chapter 2 Review Questions (R1–R15)

**R1.**  
Contoh aplikasi Internet nonproprietary dan protokolnya:  
1. Web browsing – HTTP/HTTPS  
2. E‑mail – SMTP, POP3, IMAP  
3. File transfer – FTP  
4. Name service – DNS  
5. Remote login – Telnet/SSH  

---

**R2.**  
- **Network architecture**: layanan yang disediakan jaringan (mis. TCP/IP, routing, best‑effort, dsb.).  
- **Application architecture**: cara aplikasi dibangun dan disusun (client–server, P2P, hybrid).  

---

**R3.**  
Dalam satu sesi komunikasi:  
- **Client** = proses yang menginisiasi komunikasi.  
- **Server** = proses yang menunggu dan merespons permintaan.  

---

**R4.**  
Pada P2P, setiap peer bisa bergantian menjadi **pemberi layanan** dan **peminta layanan**. Dalam tiap pertukaran, masih dapat ditetapkan peran sementara: pengirim permintaan = client, pemberi jawaban = server.  

---

**R5.**  
Proses mengidentifikasi proses lain dengan kombinasi:  
- **Alamat IP host tujuan**, dan  
- **Nomor port** proses tujuan.  

---

**R6.**  
- **Peran HTTP**: mengatur format dan urutan pesan request–response antara browser dan web server.  
- Komponen lain: browser, web server, TCP/IP, DNS, dan objek web (HTML, gambar, dsb.; opsional: database & aplikasi server‑side).  

---

**R7.**  
Contoh aplikasi yang butuh **tanpa kehilangan data** dan **sangat sensitif waktu**:  
- Sistem kontrol industri real‑time.  
- Telemedicine/operasi jarak jauh.  

---

**R8.**  
Empat kelas layanan transport dan dukungannya:  

1. **Reliable data transfer** – hanya **TCP**.  
2. **Throughput guarantee** – tidak diberikan oleh TCP maupun UDP.  
3. **Timing/delay guarantee** – tidak diberikan oleh TCP maupun UDP.  
4. **Security** – dicapai dengan protokol tambahan (mis. TLS di atas TCP), bukan oleh TCP/UDP murni.  

---

**R9.**  
- **TLS** bekerja **di atas TCP**, secara praktis dianggap di lapisan aplikasi.  
- Pengembang memakai **library TLS** untuk “membungkus” koneksi TCP (bukan memodifikasi TCP), lalu mengirim data melalui sesi TLS tersebut.  

---

**R10.**  
**Handshaking protocol**: pertukaran pesan awal untuk menyepakati parameter dan membentuk hubungan sebelum data aplikasi dikirim (contoh: three‑way handshake TCP).  

---

**R11.**  
- **Stateless protocol**: server tidak menyimpan state antar permintaan.  
- **IMAP**: **stateful** (menyimpan status folder, flag pesan, dsb. selama sesi).  
- **SMTP**: menyimpan state hanya selama satu sesi pengiriman; antar sesi dianggap tidak menyimpan state jangka panjang.  

---

**R12.**  
- Website melacak pengguna dengan: cookies, session ID, local storage, atau teknik fingerprinting.  
- Cookies **tidak selalu wajib**, tetapi paling umum digunakan.  

---

**R13.**  
- Web cache menyimpan objek populer lebih dekat ke client. Saat cache hit, objek dikirim dari cache sehingga delay turun.  
- Pengurangan delay hanya signifikan untuk **objek yang sering diminta dan cacheable**, bukan semua objek.  

---

**R14.**  
Contoh permintaan Telnet ke server web:  

```http
GET /index.html HTTP/1.1
Host: www.contoh.com
If-Modified-Since: Wed, 21 Oct 2015 07:28:00 GMT
Connection: close

```

Jika objek belum berubah sejak tanggal itu, server merespons dengan status `304 Not Modified`.  

---

**R15.**  
- **HTTP body**: bebas (dapat berupa teks/biner), ditandai dengan `Content-Type`.  
- **HTTP header**: wajib teks ASCII dengan format baris `Nama-Header: nilai` dan diakhiri baris kosong.  
- Client dan server harus memakai versi HTTP yang sama/kompatibel agar format pesan dipahami dengan benar.  
- Data sebarang dikirim lewat SMTP dengan **MIME**, yang mengenkode biner (mis. Base64) menjadi teks dan menandai tipe kontennya.  

---

## B. Socket Programming Assignments (P30–P32)

**P30.**  
- Browser modern otomatis membuka beberapa koneksi TCP paralel ke satu situs; pada beberapa browser jumlahnya dapat diatur.  
- **Keuntungan**: unduhan objek paralel → halaman lebih cepat termuat.  
- **Kerugian**: beban resource di client/server naik, potensi kemacetan jaringan dan ketidakadilan pemakaian bandwidth.  

---

**P31.**  
Perbandingan API **byte‑stream (TCP)** vs **message‑oriented (UDP)**:  

- **Kelebihan byte‑stream**:  
  - Sederhana dan fleksibel untuk data besar; aplikasi cukup menganggap data sebagai aliran byte.  
- **Kekurangan byte‑stream**:  
  - Aplikasi harus mengelola sendiri batas pesan (panjang, delimiter, dsb.).  

API yang mengenali batas pesan memudahkan aplikasi (tiap `recv` dapat berisi satu pesan logis), tetapi kurang fleksibel dan dibatasi ukuran pesan.  

---

**P32.**  
- **Apache Web Server**: web server HTTP open‑source yang sangat populer.  
- **Biaya**: gratis (lisensi Apache).  
- **Fungsi utama**:  
  - Menyajikan konten statis dan dinamis.  
  - Mendukung HTTPS/TLS, virtual hosting, modul ekstensi (PHP, proxy, rewrite, logging, autentikasi, cache, dsb.).  
  - Dapat berperan sebagai reverse proxy dan load balancer.  

---

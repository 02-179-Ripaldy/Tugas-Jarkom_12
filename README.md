# PROPOSAL PENELITIAN

**"Sistem Diagnosis Penyakit Daun Kopi Menggunakan Hybrid MobileNetV2 dan XGBoost Berbasis Citra"**

---

## BAB I

## PENDAHULUAN

### 1.1 Latar Belakang

Tanaman kopi merupakan salah satu komoditas perkebunan strategis di Indonesia. Data Badan Pusat Statistik (2023) menunjukkan bahwa luas areal perkebunan kopi mencapai lebih dari 1,23 juta hektar dengan produksi tahunan sekitar 773 ribu ton, menjadikan Indonesia sebagai produsen kopi keempat terbesar di dunia. Jenis kopi yang dominan dibudidayakan adalah Coffea arabica (Arabika) dan Coffea canephora (Robusta). Namun, produktivitas perkebunan kopi nasional masih tergolong rendah, yaitu rata-rata 700 kg/ha, jauh di bawah potensi produktivitas optimal 1.500-2.000 kg/ha (Direktorat Jenderal Perkebunan, 2022).

Salah satu penyebab utama rendahnya produktivitas adalah serangan penyakit daun, seperti Karat Daun Kopi atau Coffee Leaf Rust (CLR) yang disebabkan oleh jamur _Hemileia vastatrix_, Cercospora Leaf Spot (_Cercospora coffeicola_), dan Phoma Leaf Spot (_Phoma costarricensis_). Menurut Kementerian Pertanian (2022), serangan CLR dapat menurunkan hasil panen hingga 30–40 persen pada perkebunan kopi rakyat, bahkan pada kasus berat dapat mencapai 50 persen. Kerugian ekonomi akibat penyakit daun kopi diperkirakan mencapai miliaran rupiah per tahun, terutama di sentra produksi kopi Jawa Timur, Lampung, dan Sumatera Selatan.

Selama ini, diagnosis penyakit daun kopi masih mengandalkan pengamatan visual manual oleh petani atau penyuluh pertanian lapangan. Metode konvensional ini rentan terhadap kesalahan identifikasi karena bergantung pada pengalaman dan pengetahuan individu, serta memerlukan waktu yang lama untuk konfirmasi laboratorium. Akibatnya, penanganan penyakit sering terlambat dilakukan, infeksi menyebar lebih luas, dan kerugian ekonomi meningkat signifikan. Kondisi ini menegaskan perlunya sistem diagnosis otomatis yang mampu melakukan deteksi penyakit daun kopi secara cepat, akurat, dan efisien, terutama di tingkat petani.

Perkembangan pesat teknologi kecerdasan buatan (Artificial Intelligence), khususnya pada bidang Computer Vision dan Deep Learning, membuka peluang besar untuk mengembangkan sistem deteksi penyakit tanaman berbasis citra digital. Studi Ferentinos (2018) pada 25 spesies tanaman dengan 58 kelas penyakit menunjukkan bahwa Convolutional Neural Network (CNN) mampu mendeteksi penyakit tanaman dengan akurasi hingga 99,53%, namun membutuhkan sumber daya komputasi yang sangat besar dan waktu training yang lama. Ramcharan dkk. (2019) dalam penelitiannya pada penyakit tanaman pisang menemukan bahwa performa CNN dapat menurun drastis hingga 15-20% ketika diuji pada citra lapangan dengan variasi pencahayaan, latar belakang kompleks, dan sudut pengambilan yang tidak terkontrol. Sementara itu, penelitian Prasetyo dan Rahmadani (2022) yang menerapkan arsitektur ResNet50 untuk klasifikasi penyakit daun kopi mengalami masalah overfitting dengan gap akurasi training-testing mencapai 18%, terutama akibat keterbatasan jumlah dan keragaman dataset.

Untuk mengatasi berbagai kelemahan model CNN tunggal tersebut, penelitian ini menawarkan pendekatan hybrid yang mengombinasikan MobileNetV2 sebagai ekstraktor fitur visual dan XGBoost sebagai classifier. MobileNetV2 dipilih karena arsitekturnya yang ringan (hanya 3,4 juta parameter) dengan efisiensi tinggi melalui mekanisme depthwise separable convolution dan inverted residual blocks, sehingga dapat mengekstraksi fitur visual secara efektif bahkan pada perangkat dengan keterbatasan komputasi. Sementara itu, XGBoost dipilih karena kemampuannya dalam mengelola fitur berdimensi tinggi, regularisasi yang kuat untuk mencegah overfitting, serta performa yang konsisten pada dataset berukuran kecil hingga menengah. Kombinasi hybrid CNN-XGBoost telah terbukti efektif pada berbagai domain klasifikasi citra (Wang et al., 2021; Zhang et al., 2023), namun penerapannya masih sangat jarang dieksplorasi dalam konteks diagnosis penyakit daun kopi, khususnya di Indonesia. Oleh karena itu, penelitian ini memiliki nilai kebaruan dan kontribusi penting bagi pengembangan teknologi pertanian presisi berbasis AI.

### 1.2 Rumusan Masalah

Rumusan masalah penelitian ini adalah:

1. Bagaimana merancang sistem diagnosis penyakit daun kopi berbasis citra menggunakan kombinasi MobileNetV2 dan XGBoost?
2. Seberapa tinggi akurasi dan efisiensi model hybrid MobileNetV2–XGBoost dibandingkan dengan model CNN tunggal seperti ResNet50?

### 1.3 Tujuan Penelitian

Tujuan penelitian ini adalah untuk:

1. Merancang dan mengimplementasikan sistem diagnosis penyakit daun kopi menggunakan kombinasi MobileNetV2 dan XGBoost.
2. Mengukur dan membandingkan performa model hybrid MobileNetV2–XGBoost dengan baseline CNN ResNet50 berdasarkan akurasi, presisi, recall, F1-score, dan waktu komputasi.

### 1.4 Batasan Masalah

Penelitian ini memiliki beberapa batasan masalah. Batasan masalah ditujukan agar penelitian yang dilakukan tidak terlalu luas, dan menjadi lebih realistis dan fokus untuk diselesaikan.

1. Penelitian ini difokuskan pada pengembangan sistem diagnosis penyakit daun kopi menggunakan dataset publik RoboFlow Coffee Leaf Disease Dataset dengan total 1.800 citra yang sudah terlabeli dan hanya melibatkan empat kategori klasifikasi yaitu daun sehat (Healthy), Karat Daun Kopi (Coffee Leaf Rust), Bercak Daun Cercospora (Cercospora Leaf Spot), dan Bercak Daun Phoma (Phoma Leaf Spot).

2. Penelitian ini hanya difokuskan pada tanaman kopi Arabika (_Coffea arabica_) sebagai objek penelitian dan tidak melibatkan varietas kopi lainnya seperti Robusta.

3. Penelitian ini hanya melibatkan proses klasifikasi penyakit daun (disease classification) dan tidak mencakup segmentasi area lesi, deteksi lokasi kerusakan, atau prediksi perkembangan penyakit.

4. Penelitian ini hanya memfokuskan pada evaluasi performa model menggunakan metrik akurasi, presisi, recall, F1-score dengan validasi 5-Fold Cross-Validation.

### 1.5 Manfaat Penelitian

Penelitian ini diharapkan akan mendatangkan manfaat sebagai berikut:

1. Penelitian akan menghasilkan sistem diagnosis penyakit daun kopi berbasis hybrid MobileNetV2 dan XGBoost yang dapat membantu petani dalam mendeteksi penyakit dengan lebih cepat dan akurat, sehingga penanganan dapat dilakukan tepat waktu tanpa harus menunggu konfirmasi manual dari penyuluh pertanian atau laboratorium.

2. Penelitian ini akan menghasilkan bukti empiris tentang efektivitas pendekatan hybrid CNN-XGBoost dalam klasifikasi penyakit tanaman yang diharapkan dapat menjadi acuan atau referensi bagi penelitian lanjutan dengan topik yang serupa di bidang computer vision untuk pertanian presisi.

---

## BAB II

## TINJAUAN PUSTAKA

### 2.1 Landasan Teori

#### 2.1.1 Tanaman Kopi

Tanaman kopi (_Coffea_ spp.) merupakan tanaman perkebunan dari famili Rubiaceae yang berperan sangat penting dalam perekonomian Indonesia. Dua spesies utama yang dibudidayakan secara komersial adalah _Coffea arabica_ (kopi Arabika) yang mencakup 60-65% produksi nasional dan _Coffea canephora_ (kopi Robusta) sekitar 35-40%. Kopi Arabika umumnya ditanam pada ketinggian 1.000-2.000 mdpl dengan karakteristik rasa yang lebih kompleks, sementara Robusta lebih toleran terhadap dataran rendah dan memiliki kandungan kafein lebih tinggi. Kesehatan daun kopi sangat menentukan produktivitas tanaman karena penyakit daun yang menyerang jaringan fotosintesis akan menghambat produksi karbohidrat dan pada akhirnya menurunkan kualitas serta kuantitas biji kopi yang dihasilkan.

#### 2.1.2 Penyakit Daun Kopi

Penyakit daun merupakan ancaman utama produktivitas perkebunan kopi di Indonesia. Tiga penyakit daun kopi utama yang menjadi fokus penelitian ini adalah Karat Daun Kopi (Coffee Leaf Rust) yang disebabkan oleh jamur _Hemileia vastatrix_ dengan gejala bercak kuning-oranye dan dapat menurunkan hasil panen hingga 30-40%, Bercak Daun Cercospora (_Cercospora coffeicola_) dengan gejala bercak coklat kemerahan yang sering disebut "brown eye spot" dan menurunkan produktivitas 10-20%, serta Bercak Daun Phoma (_Phoma costarricensis_) dengan gejala bercak coklat gelap hingga hitam yang menurunkan hasil panen 10-15%. Ketiga penyakit ini sering muncul bersamaan (infeksi kompleks) dan menyebabkan kerugian ekonomi miliaran rupiah per tahun, sehingga mempersulit diagnosis visual manual dan membutuhkan pendekatan deteksi otomatis berbasis AI untuk identifikasi yang akurat dan cepat.

#### 2.1.3 Computer Vision

Computer Vision adalah cabang ilmu komputer dan kecerdasan buatan yang mempelajari bagaimana sistem komputer dapat memperoleh pemahaman tingkat tinggi dari citra atau video digital. Dalam konteks pertanian, Computer Vision telah diaplikasikan untuk deteksi dan klasifikasi penyakit tanaman, monitoring pertumbuhan, estimasi hasil panen, dan deteksi hama. Keunggulan utama teknologi ini meliputi kecepatan diagnosis dalam hitungan detik, konsistensi tinggi tanpa bias subjektif, skalabilitas untuk monitoring area luas, aksesibilitas melalui perangkat mobile, dan kemampuan deteksi dini pada stadium awal penyakit yang sulit diamati mata manusia.

#### 2.1.4 Convolutional Neural Network (CNN)

Convolutional Neural Network (CNN) adalah arsitektur deep learning yang dirancang khusus untuk memproses data citra dan telah menjadi standar untuk tugas computer vision sejak keberhasilan AlexNet pada kompetisi ImageNet 2012. Arsitektur CNN terdiri dari Convolutional Layer untuk ekstraksi fitur secara hierarkis dari sederhana hingga kompleks, Activation Function (ReLU) untuk non-linearitas, Pooling Layer untuk downsampling dan efisiensi komputasi, serta Fully Connected Layer untuk menghasilkan probabilitas kelas output.

CNN memiliki keunggulan utama berupa parameter sharing untuk efisiensi, spatial hierarchy untuk representasi hierarkis, translation invariance untuk fleksibilitas deteksi, dan automatic feature learning tanpa rekayasa manual. Namun CNN menghadapi tantangan berupa kebutuhan dataset besar, komputasi intensif, risiko overfitting pada dataset kecil, dan model besar yang tidak efisien untuk perangkat mobile. Untuk mengatasi tantangan tersebut, dikembangkan arsitektur efisien seperti MobileNet yang menjadi basis penelitian ini.

#### 2.1.5 MobileNetV2

MobileNetV2 adalah arsitektur CNN efisien yang dikembangkan oleh Google (Sandler et al., 2018) untuk aplikasi mobile dan embedded vision. Arsitektur ini memiliki tiga inovasi utama: Depthwise Separable Convolution yang mengurangi komputasi hingga 8-9 kali, Inverted Residual Block dengan mekanisme ekspansi-depthwise-proyeksi yang efisien, dan Residual Connection untuk gradient flow yang lebih baik.

MobileNetV2 memiliki 3,4 juta parameter (94% lebih kecil dari ResNet50), menghasilkan output feature 1280 dimensi, dan mencapai akurasi 72% pada ImageNet dengan ukuran model hanya 14 MB. Pemilihan MobileNetV2 dalam penelitian ini didasarkan pada efisiensi komputasi untuk deployment mobile, keseimbangan performa dengan ukuran kecil, dukungan transfer learning untuk dataset kecil, dan kapabilitas feature extraction 1280 dimensi yang optimal untuk klasifikasi lanjutan dengan XGBoost.

#### 2.1.6 XGBoost (Extreme Gradient Boosting)

XGBoost adalah algoritma machine learning berbasis ensemble learning yang dikembangkan oleh Chen & Guestrin (2016) menggunakan gradient boosting decision tree. XGBoost bekerja dengan membangun model secara bertahap dimana setiap model baru memperbaiki kesalahan prediksi model sebelumnya hingga mencapai performa optimal. Algoritma ini mengoptimasi fungsi objektif yang mengkombinasikan loss function dan regularization term untuk mencegah overfitting sehingga model dapat generalisasi dengan baik pada data baru.

Keunggulan utama XGBoost meliputi performa tinggi di kompetisi machine learning, regularization kuat dengan L1/L2 dan tree pruning, handling missing values otomatis, feature importance untuk interpretability, dan efisiensi komputasi melalui parallelization. Pemilihan XGBoost sebagai classifier didasarkan pada kemampuannya mengelola fitur dimensi tinggi (1280 fitur dari MobileNetV2), performa baik pada dataset kecil-menengah (1.800 citra), regularization kuat untuk mencegah overfitting, training yang lebih cepat dibanding fine-tuning CNN, dan terbukti unggul 3-5% dibanding CNN end-to-end (Wang et al., 2021).

### 2.2 Penelitian Terdahulu

Berikut adalah tinjauan komprehensif penelitian terdahulu yang relevan dengan deteksi penyakit tanaman menggunakan deep learning dan machine learning:

| No  | Peneliti & Tahun                   | Metode                        | Dataset                                     | Akurasi                   | Kelebihan                                                                   | Kekurangan                                                                                                |
| --- | ---------------------------------- | ----------------------------- | ------------------------------------------- | ------------------------- | --------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| 1   | Ferentinos (2018)                  | CNN (AlexNet, VGG, GoogLeNet) | 87.848 citra, 25 tanaman, 58 kelas penyakit | 99,53%                    | - Akurasi sangat tinggi<br>- Benchmark komprehensif                         | - Dataset sangat besar (sulit direplikasi)<br>- Membutuhkan GPU Tesla K80<br>- Tidak efisien untuk mobile |
| 2   | Ramcharan et al. (2019)            | MobileNetV2 end-to-end        | 11.670 citra tanaman pisang, 5 kelas        | 89% (lab)<br>73% (field)  | - Efisien untuk mobile<br>- Real-world testing                              | - Performa drop signifikan di lapangan<br>- Sensitif terhadap variasi pencahayaan                         |
| 3   | Mohanty et al. (2016)              | AlexNet, GoogLeNet            | PlantVillage 54.306 citra, 38 kelas         | 99,35%                    | - Dataset publik<br>- Arsitektur state-of-art                               | - Overfitting pada background uniform<br>- Gagal pada citra lapangan kompleks                             |
| 4   | Prasetyo & Rahmadani (2022)        | ResNet50 fine-tuning          | 1.200 citra daun kopi, 4 kelas              | 88% (train)<br>70% (test) | - Fokus pada kopi Indonesia<br>- Transfer learning                          | - Overfitting berat (gap 18%)<br>- Dataset terbatas<br>- Model besar (98 MB)                              |
| 5   | Wang et al. (2021)                 | ResNet50 + XGBoost            | 10.000 citra medis, 7 kelas tumor           | 94,2%                     | - Hybrid approach efektif<br>- Regularization kuat<br>- Reduksi overfitting | - Domain medis (bukan pertanian)<br>- Dataset sangat besar                                                |
| 6   | Zhang et al. (2023)                | EfficientNet + Random Forest  | 8.500 citra sayuran, 12 kelas               | 91,8%                     | - Ensemble learning<br>- Feature selection otomatis                         | - Waktu training lama<br>- Hyperparameter kompleks                                                        |
| 7   | Saleem et al. (2020)               | VGG16 + SVM                   | 4.000 citra tomat, 10 kelas                 | 87,3%                     | - SVM efektif pada fitur CNN<br>- Dataset menengah                          | - VGG terlalu besar (528 MB)<br>- Akurasi moderat                                                         |
| 8   | Kamilaris & Prenafeta-Boldú (2018) | Survey 40+ paper              | Meta-analysis                               | Rata-rata 85-95%          | - Comprehensive review<br>- Best practices                                  | - Tidak ada implementasi baru<br>- Gap research masih banyak                                              |

**Analisis Gap Penelitian:**

Berdasarkan tinjauan penelitian terdahulu, terdapat beberapa gap atau kesenjangan yang perlu diisi. Pertama, terdapat dataset size issue dimana penelitian dengan akurasi tinggi seperti Ferentinos dan Mohanty memerlukan dataset sangat besar dengan lebih dari 50.000 citra yang tidak realistis untuk kasus spesifik kopi Indonesia. Kedua, overfitting problem menjadi kendala dimana model CNN besar seperti ResNet50 dan VGG rentan overfitting pada dataset kecil-menengah dengan kurang dari 5.000 citra, seperti yang dialami Prasetyo & Rahmadani. Ketiga, field performance gap menunjukkan bahwa model yang excellent di laboratorium sering gagal di lapangan akibat variasi pencahayaan, latar belakang, dan kondisi daun sebagaimana dilaporkan Ramcharan et al. Keempat, computational efficiency menjadi masalah karena model besar tidak efisien untuk deployment mobile yang dibutuhkan petani di lapangan. Kelima, limited hybrid approach terlihat dari sedikitnya penerapan kombinasi CNN feature extraction dengan ML classifier seperti XGBoost, Random Forest, atau SVM pada penyakit tanaman, padahal terbukti efektif di domain lain seperti medis dan industri. Keenam, Indonesian coffee context masih sangat terbatas karena sangat sedikit penelitian yang fokus pada penyakit kopi di Indonesia dengan kondisi lapangan lokal.

**Posisi Penelitian Ini:**

Penelitian ini mengisi gap yang telah diidentifikasi dengan mengombinasikan beberapa pendekatan strategis. MobileNetV2 dipilih sebagai feature extractor karena efisien dan cocok untuk deployment mobile, sementara XGBoost digunakan sebagai classifier karena memiliki regularization kuat dan efektif pada dataset berukuran menengah. Penelitian ini memiliki fokus khusus pada kopi Indonesia dengan rencana validasi lapangan untuk memastikan aplikabilitas praktis. Dataset publik dari RoboFlow dengan 1.800 citra digunakan agar penelitian dapat direplikasi oleh peneliti lain. Pendekatan hybrid yang mengombinasikan CNN dan XGBoost belum banyak dieksplorasi untuk deteksi penyakit kopi sehingga penelitian ini memiliki nilai kebaruan. Kombinasi seluruh elemen ini diharapkan dapat memberikan keseimbangan optimal antara akurasi deteksi, efisiensi komputasi, dan aplikabilitas praktis di lapangan.

### 2.3 Kerangka Pemikiran

Kerangka pemikiran penelitian ini menggambarkan alur logis dari permasalahan menuju solusi yang ditawarkan:

```
┌─────────────────────────────────────────────────────────────────┐
│                       PERMASALAHAN                              │
│                                                                 │
│  Diagnosis penyakit daun kopi masih manual, tidak akurat,       │
│  dan lambat, sehingga penanganan terlambat dan kerugian         │
│  ekonomi meningkat. Model CNN tunggal rentan overfitting        │
│  pada dataset kecil dan tidak efisien untuk mobile.             │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                    LANDASAN TEORI                               │
│                                                                 │
│  ┌───────────────────┐         ┌──────────────────┐             │
│  │ Computer Vision   │         │ Machine Learning │             │
│  │ & Deep Learning   │    +    │ (Ensemble)       │             │
│  └───────────────────┘         └──────────────────┘             │
│           │                             │                       │
│           ↓                             ↓                       │
│  ┌───────────────────┐         ┌──────────────────┐             │
│  │  Transfer Learning│         │ Gradient Boosting│             │
│  │  (MobileNetV2)    │         │  (XGBoost)       │             │
│  └───────────────────┘         └──────────────────┘             │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│              PENDEKATAN HYBRID (SOLUSI)                         │
│                                                                 │
│          MobileNetV2                    XGBoost                 │
│      (Feature Extractor)     ──────>  (Classifier)              │
│                                                                 │
│  Faktor Pendukung:                                              │
│  • Dataset: RoboFlow (1.800 citra, 4 kelas)                     │
│  • Preprocessing: Augmentasi & normalisasi                      │
│  • Hyperparameter: Grid search & cross-validation               │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                 SISTEM DIAGNOSIS PENYAKIT                       │
│                                                                 │
│  Input: Citra Daun Kopi  →  Proses: Model Hybrid  →  Output:    │
│                                                     Diagnosis + │
│                                                     Confidence  │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                    EVALUASI MODEL                               │
│                                                                 │
│  • Metrik Akurasi: Accuracy, Precision, Recall, F1-Score        │
│  • Validasi: 5-Fold Cross-Validation                            │
│  • Perbandingan: Baseline (MobileNetV2 E2E, ResNet50 E2E)       │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                  HASIL YANG DIHARAPKAN                          │
│                                                                 │
│  • Akurasi ≥ 90%                                                │
│  • Model efisien (ukuran <20 MB, inferensi <2 detik)            │
│  • Mengatasi overfitting (train-test gap minimal)               │
│  • Sistem diagnosis siap implementasi untuk petani              │
└─────────────────────────────────────────────────────────────────┘
```

**Penjelasan Kerangka Pemikiran:**

Penelitian ini berangkat dari permasalahan diagnosis penyakit daun kopi yang masih manual dan tidak akurat, menyebabkan penanganan terlambat. Berdasarkan landasan teori Computer Vision, Deep Learning, dan Machine Learning, penelitian ini mengusulkan pendekatan hybrid yang mengombinasikan kekuatan Transfer Learning (MobileNetV2) untuk ekstraksi fitur visual dan Ensemble Learning (XGBoost) untuk klasifikasi optimal.

MobileNetV2 dipilih karena arsitekturnya yang efisien dengan pre-trained weights ImageNet sehingga dapat mengatasi keterbatasan dataset kecil melalui transfer learning. Fitur visual berdimensi tinggi (1280-D) yang diekstrak kemudian diklasifikasikan menggunakan XGBoost yang memiliki regularization kuat untuk mencegah overfitting dan efektif pada dataset berukuran menengah.

Faktor pendukung meliputi dataset publik RoboFlow dengan 1.800 citra berlabel, teknik preprocessing (augmentasi dan normalisasi), serta optimasi hyperparameter melalui grid search dan cross-validation. Sistem diagnosis yang dihasilkan akan dievaluasi menggunakan metrik akurasi standar dan dibandingkan dengan baseline model CNN end-to-end. Hasil akhir yang diharapkan adalah sistem diagnosis akurat (≥90%), efisien untuk deployment mobile, dan mampu membantu petani dalam deteksi dini penyakit daun kopi.

---

## BAB III
## METODOLOGI PENELITIAN

### 3.1 Jenis dan Pendekatan Penelitian

Penelitian ini merupakan penelitian terapan (*applied research*) dengan pendekatan kuantitatif yang berfokus pada pengembangan dan evaluasi model diagnosis penyakit daun kopi berbasis citra. Pendekatan kuantitatif dipilih karena penelitian ini melibatkan pengukuran performa model menggunakan metrik numerik (akurasi, presisi, recall, F1-score) dan analisis statistik untuk membandingkan metode hybrid MobileNetV2-XGBoost dengan baseline model CNN end-to-end.

### 3.2 Tempat dan Waktu Penelitian

Penelitian ini dilaksanakan secara fleksibel menggunakan platform komputasi berbasis cloud (*cloud-based computing*) yang tidak terikat pada lokasi fisik tertentu. Pengembangan dan pelatihan model dilakukan menggunakan **Google Colab**, yaitu platform cloud computing yang menyediakan akses gratis ke GPU NVIDIA Tesla T4 dengan 16 GB VRAM. Pemilihan Google Colab sebagai platform utama didasarkan pada beberapa pertimbangan, yaitu aksesibilitas tinggi karena dapat diakses dari mana saja dengan koneksi internet, efisiensi biaya karena tidak memerlukan investasi hardware mahal, ketersediaan GPU gratis untuk mempercepat proses feature extraction menggunakan MobileNetV2, serta kemudahan replikasi penelitian oleh peneliti lain karena berbasis cloud. Seluruh proses penelitian mulai dari preprocessing data, ekstraksi fitur, training model, hingga evaluasi performa dilakukan melalui platform ini. Dokumentasi penelitian dan penulisan laporan dilakukan menggunakan perangkat pribadi dengan spesifikasi standar yang terhubung dengan Google Drive untuk penyimpanan data dan hasil penelitian.

Waktu pelaksanaan penelitian adalah selama 6 bulan, dimulai dari bulan Januari hingga Juni 2025. Periode ini mencakup tahapan studi literatur, pengumpulan dan preprocessing data, pengembangan model, evaluasi performa, hingga penyusunan laporan akhir. Jadwal detail pelaksanaan penelitian dengan pembagian aktivitas per minggu dapat dilihat pada bagian timeline penelitian di bawah ini.

### 3.3 Sumber Data / Data Penelitian

#### 3.3.1 Sumber Data Primer

Data penelitian menggunakan dataset publik **RoboFlow Coffee Leaf Disease Dataset** yang dapat diakses melalui platform RoboFlow Universe (https://universe.roboflow.com/coffee-disease-detection). Dataset ini terdiri dari 1.800 citra daun kopi Arabika (*Coffea arabica*) yang telah dianotasi dan dilabeli oleh expert di bidang agronomi dan fitopatologi untuk memastikan akurasi klasifikasi penyakit. Dataset tersedia dengan lisensi CC BY 4.0 sehingga dapat digunakan secara legal untuk keperluan riset akademik dan pengembangan model machine learning. Citra dalam dataset disimpan dalam format JPEG dan PNG dengan resolusi yang bervariasi, mencerminkan kondisi pengambilan gambar yang beragam dari berbagai sumber dan perangkat, sehingga memberikan representasi yang realistis untuk pengembangan model yang robust terhadap variasi input.

#### 3.3.2 Distribusi Kelas
Distribusi kelas pada dataset penelitian ini dapat dilihat pada Tabel berikut, yang merangkum jumlah citra dan persentase untuk masing-masing kategori penyakit daun kopi:

| Kelas                      | Deskripsi                    | Jumlah Citra | Persentase |
| -------------------------- | ---------------------------- | ------------ | ---------- |
| **Healthy**                | Daun sehat tanpa gejala      | 500          | 27,8%      |
| **Coffee Leaf Rust (CLR)** | Bercak kuning-oranye karat   | 540          | 30,0%      |
| **Cercospora Leaf Spot**   | Bercak coklat "brown eye"    | 400          | 22,2%      |
| **Phoma Leaf Spot**        | Bercak hitam tidak beraturan | 360          | 20,0%      |
| **TOTAL**                  |                              | **1.800**    | **100%**   |

Tabel di atas menunjukkan bahwa dataset yang digunakan dalam penelitian ini tergolong seimbang (*balanced*), dengan perbedaan jumlah citra antar kelas tidak melebihi 10%. Kondisi ini sangat ideal karena meminimalkan risiko bias model terhadap kelas mayoritas, sehingga tidak diperlukan teknik penyeimbangan data seperti *oversampling* atau *undersampling*.

#### 3.3.3 Spesifikasi Data

Dataset citra daun kopi memiliki karakteristik yang bervariasi dari segi resolusi asli, berkisar antara 800×600 hingga 4000×3000 piksel, namun akan diseragamkan menjadi 224×224 piksel pada tahap preprocessing untuk memenuhi standar input arsitektur MobileNetV2. Semua citra menggunakan *color space* RGB dengan 3 channel warna dan memiliki ukuran file antara 50 hingga 500 KB per citra. Dataset ini mencakup variasi kondisi pengambilan gambar yang realistis, meliputi variasi pencahayaan dari terang hingga redup, latar belakang yang beragam mulai dari uniform hingga kompleks, sudut pengambilan frontal maupun miring, serta tingkat infeksi penyakit dari ringan hingga berat. Variasi kondisi ini penting untuk memastikan model yang dikembangkan robust dan dapat bekerja dengan baik pada kondisi lapangan yang tidak terkontrol.

Dataset dibagi menjadi tiga subset menggunakan teknik *stratified sampling* untuk mempertahankan proporsi distribusi kelas pada setiap subset:

| Subset | Jumlah Citra | Persentase | Fungsi |
|--------|--------------|------------|--------|
| **Training** | 1.260 | 70% | Melatih model hybrid |
| **Validation** | 270 | 15% | Tuning hyperparameter |
| **Testing** | 270 | 15% | Evaluasi final model |

### 3.4 Teknik Pengumpulan Data

Pengumpulan data dalam penelitian ini dilakukan melalui tiga tahapan sistematis untuk memastikan kualitas dan kesiapan dataset. Tahap pertama adalah proses unduh dataset (*download dataset*) dimana dataset publik diunduh dari platform RoboFlow Universe menggunakan API key yang telah disediakan oleh platform tersebut, dengan format unduhan berupa file ZIP yang berisi folder terpisah untuk setiap kelas penyakit sehingga memudahkan proses identifikasi dan organisasi data. Tahap kedua adalah verifikasi data (*data verification*) yang merupakan tahap krusial untuk memastikan integritas dataset, dimana seluruh citra diverifikasi secara menyeluruh untuk memastikan tidak ada file yang corrupt atau rusak yang dapat mengganggu proses training, memvalidasi bahwa label yang diberikan sesuai dengan kondisi visual daun pada citra, mendeteksi dan menghapus duplikasi data untuk mencegah bias dalam model, serta memeriksa kualitas citra untuk memastikan tidak terdapat blur berlebihan atau noise yang dapat menurunkan performa model. Tahap ketiga adalah organisasi data (*data organization*) dimana seluruh data diorganisir dalam struktur folder yang sistematis dan terstruktur sesuai dengan kelas penyakit dan pembagian subset dataset (training, validation, testing) untuk memudahkan proses loading data dan preprocessing pada tahapan berikutnya, serta memastikan reproducibility penelitian.

### 3.5 Prosedur / Tahapan Penelitian

Penelitian ini dilaksanakan melalui tahapan sistematis sebagai berikut:

#### **Tahap 1: Studi Literatur**

Melakukan kajian mendalam terhadap penelitian terdahulu tentang deteksi penyakit tanaman menggunakan deep learning, Computer Vision, dan metode hybrid CNN-XGBoost. Literatur yang dikaji meliputi jurnal internasional, prosiding konferensi, dan dokumentasi teknis untuk memahami state-of-the-art dan gap penelitian yang ada.

#### **Tahap 2: Pengumpulan dan Persiapan Data**

Download dataset publik RoboFlow Coffee Leaf Disease Dataset (1.800 citra) dan melakukan verifikasi kualitas data. Data dibagi menjadi training set (70%), validation set (15%), dan testing set (15%) menggunakan stratified sampling untuk mempertahankan distribusi kelas.

#### **Tahap 3: Pra-pemrosesan Data**

Melakukan preprocessing citra meliputi:
- **Data cleaning**: Deteksi dan hapus duplikat, filter citra blur
- **Resizing**: Ubah resolusi menjadi 224×224 piksel (standar MobileNetV2)
- **Normalisasi**: Normalisasi nilai piksel ke rentang [0,1] dan standardisasi menggunakan mean-std ImageNet
- **Augmentasi**: Terapkan teknik augmentasi (rotation ±20°, flip, zoom, brightness adjustment) hanya pada training set untuk meningkatkan variasi data

#### **Tahap 4: Ekstraksi Fitur Menggunakan MobileNetV2**

Load model MobileNetV2 pre-trained dengan ImageNet weights. Semua convolutional layers di-freeze untuk memanfaatkan transfer learning. Setiap citra diproses melalui MobileNetV2 untuk mengekstrak feature vector berdimensi 1280 dari Global Average Pooling layer. Fitur ini kemudian disimpan untuk efisiensi komputasi pada tahap training.

#### **Tahap 5: Training Classifier XGBoost**

**4.1 Konfigurasi XGBoost**

```python
import xgboost as xgb
from sklearn.model_selection import GridSearchCV

# Hyperparameter search space
param_grid = {
    'n_estimators': [100, 300, 500],
    'max_depth': [3, 5, 7, 9],
    'learning_rate': [0.01, 0.05, 0.1, 0.2],
    'subsample': [0.7, 0.8, 0.9, 1.0],
    'colsample_bytree': [0.7, 0.8, 0.9, 1.0],
    'gamma': [0, 0.1, 0.5, 1],
    'reg_alpha': [0, 0.1, 0.5, 1],  # L1 regularization
    'reg_lambda': [0.5, 1, 2, 5]     # L2 regularization
}

# XGBoost classifier
xgb_model = xgb.XGBClassifier(
    objective='multi:softmax',  # Multi-class classification
    num_class=4,                # 4 kelas penyakit
    eval_metric='mlogloss',     # Multi-class log loss
    random_state=42,
    n_jobs=-1                   # Parallelization
)
```

**4.2 Hyperparameter Tuning**

```python
# Grid Search dengan 5-Fold Cross-Validation
grid_search = GridSearchCV(
    estimator=xgb_model,
    param_grid=param_grid,
    cv=5,                       # 5-fold CV
    scoring='f1_macro',         # Optimasi F1-Score
    n_jobs=-1,
    verbose=2
)

# Training dengan fitur MobileNetV2
grid_search.fit(
    X_train_features, y_train,
    eval_set=[(X_val_features, y_val)],
    early_stopping_rounds=50,   # Stop jika tidak ada improvement
    verbose=False
)

# Best hyperparameters
best_params = grid_search.best_params_
best_model = grid_search.best_estimator_
```

**4.3 Training Final Model**

- Train model dengan best hyperparameters pada train + validation set
- Monitoring: training loss, validation loss, accuracy
- Early stopping jika overfitting terdeteksi

**Output Tahap 4**: Trained XGBoost classifier dengan hyperparameter optimal

---

#### **Tahap 5: Evaluasi Performa Model**

**5.1 Prediksi pada Test Set**

```python
# Prediksi kelas
y_pred = best_model.predict(X_test_features)

# Prediksi probabilitas
y_pred_proba = best_model.predict_proba(X_test_features)
```

**5.2 Kalkulasi Metrik Evaluasi**

```python
from sklearn.metrics import accuracy_score, precision_recall_fscore_support
from sklearn.metrics import confusion_matrix, classification_report

# Metrik per-class
precision, recall, f1, support = precision_recall_fscore_support(
    y_test, y_pred, average=None
)

# Metrik overall
accuracy = accuracy_score(y_test, y_pred)
precision_macro = precision_recall_fscore_support(y_test, y_pred, average='macro')[0]
recall_macro = precision_recall_fscore_support(y_test, y_pred, average='macro')[1]
f1_macro = precision_recall_fscore_support(y_test, y_pred, average='macro')[2]

# Confusion matrix
cm = confusion_matrix(y_test, y_pred)
```

**5.3 Visualisasi Hasil**

- Confusion Matrix heatmap
- ROC Curve per kelas (One-vs-Rest)
- Precision-Recall Curve
- Feature Importance dari XGBoost

**Output Tahap 5**: Comprehensive evaluation report

---

#### **Tahap 6: Validasi dengan 5-Fold Cross-Validation**

**6.1 K-Fold Cross-Validation**

```python
from sklearn.model_selection import cross_val_score, StratifiedKFold

# Stratified K-Fold (mempertahankan proporsi kelas)
skfold = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)

# Cross-validation scores
cv_scores = cross_val_score(
    best_model,
    X_train_features,
    y_train,
    cv=skfold,
    scoring='f1_macro',
    n_jobs=-1
)

print(f"CV F1-Scores: {cv_scores}")
print(f"Mean F1: {cv_scores.mean():.4f} (+/- {cv_scores.std():.4f})")
```

**6.2 Analisis Stabilitas**

- Cek variance antar fold (jika variance tinggi → model tidak stabil)
- Identifikasi fold dengan performa terendah
- Analisis kesalahan pada fold tersebut

**Output Tahap 6**: Validasi generalisasi model dengan CV metrics

---

#### **Tahap 7: Perbandingan dengan Baseline**

**7.1 Baseline Model 1: MobileNetV2 End-to-End**

```python
# Fine-tune MobileNetV2 dengan classification head
base_model = MobileNetV2(weights='imagenet', include_top=False, input_shape=(224,224,3))
x = base_model.output
x = GlobalAveragePooling2D()(x)
x = Dense(128, activation='relu')(x)
output = Dense(4, activation='softmax')(x)

model_mobilenet = Model(inputs=base_model.input, outputs=output)

# Freeze base layers, train only top layers
for layer in base_model.layers:
    layer.trainable = False

model_mobilenet.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
model_mobilenet.fit(X_train, y_train, validation_data=(X_val, y_val), epochs=50)
```

**7.2 Baseline Model 2: ResNet50 End-to-End**

```python
from tensorflow.keras.applications import ResNet50

base_model = ResNet50(weights='imagenet', include_top=False, input_shape=(224,224,3))
# Similar architecture as MobileNetV2
```

**7.3 Komparasi Performa**

| Model                          | Akurasi | F1-Score | Waktu Training | Ukuran Model | Waktu Inferensi |
| ------------------------------ | ------- | -------- | -------------- | ------------ | --------------- |
| **MobileNetV2-XGBoost (Ours)** | TBD     | TBD      | TBD            | TBD          | TBD             |
| MobileNetV2 End-to-End         | TBD     | TBD      | TBD            | 14 MB        | TBD             |
| ResNet50 End-to-End            | TBD     | TBD      | TBD            | 98 MB        | TBD             |

**7.4 Analisis Komparatif**

- Perbandingan akurasi, precision, recall, F1-score
- Perbandingan efisiensi (waktu, memori, ukuran model)
- Analisis overfitting (train-test gap)
- Performa pada kondisi challenging (pencahayaan rendah, background kompleks)

**Output Tahap 7**: Laporan komparasi komprehensif dengan baseline models

---

### Diagram Alir Penelitian

```
[START]
   ↓
[Pengumpulan Data: 1.800 citra RoboFlow]
   ↓
[Labeling & Validasi Expert]
   ↓
[Data Cleaning & Preprocessing]
   ↓
[Split: 70% Train | 15% Val | 15% Test]
   ↓
[Augmentasi Training Set]
   ↓
┌─────────────────────────────────┐
│ FEATURE EXTRACTION              │
│ MobileNetV2 (Pre-trained)       │
│ Input: 224×224×3                │
│ Output: 1280-D features         │
└─────────────────────────────────┘
   ↓
┌─────────────────────────────────┐
│ CLASSIFICATION                  │
│ XGBoost Classifier              │
│ Hyperparameter Tuning (Grid)    │
│ 5-Fold Cross-Validation         │
└─────────────────────────────────┘
   ↓
[Evaluasi Test Set]
   ├─ Accuracy, Precision, Recall, F1
   ├─ Confusion Matrix
   ├─ ROC Curve
   └─ Feature Importance
   ↓
[Perbandingan dengan Baseline]
   ├─ MobileNetV2 End-to-End
   └─ ResNet50 End-to-End
   ↓
[Analisis Hasil & Interpretasi]
   ↓
[Dokumentasi & Publikasi]
   ↓
[END]
```

### 3.4 Lingkungan Implementasi

#### 3.4.1 Spesifikasi Hardware

**Platform Utama: Google Colab Pro (Cloud)**

- **GPU**: NVIDIA Tesla T4 (16 GB VRAM)
- **CPU**: Intel Xeon (2-core)
- **RAM**: 12 GB
- **Storage**: 100 GB Google Drive
- **Alasan Pemilihan**:
  - Akses GPU gratis untuk mempercepat feature extraction MobileNetV2
  - Tidak memerlukan investasi hardware mahal
  - Dapat direplikasi oleh peneliti lain dengan mudah

**Platform Sekunder: Laptop Lokal (untuk development & testing)**

- **Processor**: Intel Core i5-10210U (4 core, 8 thread, 1.6-4.2 GHz)
- **RAM**: 8 GB DDR4
- **Storage**: 256 GB SSD
- **GPU**: Integrated Intel UHD Graphics (no CUDA support)
- **OS**: Windows 10 / Ubuntu 20.04 LTS

#### 3.4.2 Spesifikasi Software

**Bahasa Pemrograman:**

- Python 3.9.13 (atau lebih tinggi)

**Deep Learning Framework:**

- TensorFlow 2.12.0
- Keras 2.12.0 (included in TensorFlow)

**Machine Learning Libraries:**

- Scikit-Learn 1.2.2 (untuk preprocessing, metrics, CV)
- XGBoost 1.7.5 (untuk classifier)
- Imbalanced-Learn 0.10.1 (jika diperlukan SMOTE)

**Data Processing:**

- NumPy 1.23.5 (array operations)
- Pandas 1.5.3 (data manipulation)
- OpenCV 4.7.0 (image processing)
- Pillow 9.4.0 (image loading)

**Visualization:**

- Matplotlib 3.7.1 (plotting)
- Seaborn 0.12.2 (statistical visualization)
- Plotly 5.14.1 (interactive plots)

**Development Tools:**

- Jupyter Notebook / Google Colab (interactive development)
- VS Code 1.76 (code editor)
- Git 2.40 (version control)

**Environment Management:**

- Anaconda 23.1.0 / pip 23.0.1

#### 3.4.3 Instalasi Environment

**Setup Colab (Recommended):**

```python
# Install dependencies di Google Colab
!pip install tensorflow==2.12.0
!pip install xgboost==1.7.5
!pip install scikit-learn==1.2.2
!pip install opencv-python==4.7.0.72
!pip install seaborn matplotlib plotly

# Verify GPU availability
import tensorflow as tf
print("GPU Available:", tf.config.list_physical_devices('GPU'))
```

**Setup Lokal (Alternative):**

```bash
# Create virtual environment
conda create -n coffee-disease python=3.9
conda activate coffee-disease

# Install dependencies
pip install -r requirements.txt
```

**File `requirements.txt`:**

```
tensorflow==2.12.0
xgboost==1.7.5
scikit-learn==1.2.2
numpy==1.23.5
pandas==1.5.3
opencv-python==4.7.0.72
Pillow==9.4.0
matplotlib==3.7.1
seaborn==0.12.2
plotly==5.14.1
jupyter==1.0.0
```

#### 3.4.4 Struktur Direktori Proyek

```
coffee-disease-detection/
│
├── data/
│   ├── raw/                    # Dataset original
│   ├── processed/              # Dataset setelah preprocessing
│   ├── features/               # Extracted features (1280-D)
│   └── splits/                 # Train/val/test splits
│
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_preprocessing.ipynb
│   ├── 03_feature_extraction.ipynb
│   ├── 04_xgboost_training.ipynb
│   ├── 05_evaluation.ipynb
│   └── 06_baseline_comparison.ipynb
│
├── src/
│   ├── data_loader.py          # Data loading utilities
│   ├── preprocessing.py        # Preprocessing functions
│   ├── feature_extractor.py    # MobileNetV2 feature extraction
│   ├── xgboost_classifier.py   # XGBoost training & prediction
│   ├── evaluation.py           # Evaluation metrics
│   └── visualization.py        # Plotting functions
│
├── models/
│   ├── mobilenetv2_weights.h5  # Pre-trained MobileNetV2
│   ├── xgboost_model.pkl       # Trained XGBoost
│   └── config.yaml             # Model configurations
│
├── results/
│   ├── metrics/                # Evaluation metrics (CSV, JSON)
│   ├── plots/                  # Confusion matrix, ROC curve, etc.
│   └── reports/                # Final reports (PDF, HTML)
│
├── requirements.txt            # Dependencies
├── README.md                   # Project documentation
└── config.yaml                 # Experiment configurations
```

#### 3.4.5 Estimasi Waktu Komputasi

| Tahapan                          | Platform       | Estimasi Waktu |
| -------------------------------- | -------------- | -------------- |
| Data Preprocessing               | Laptop / Colab | 10-15 menit    |
| Feature Extraction (MobileNetV2) | Colab GPU      | 5-10 menit     |
| Feature Extraction (MobileNetV2) | Laptop CPU     | 30-45 menit    |
| XGBoost Training (Grid Search)   | Colab / Laptop | 15-30 menit    |
| XGBoost Training (Best Params)   | Colab / Laptop | 2-5 menit      |
| 5-Fold Cross-Validation          | Colab / Laptop | 10-20 menit    |
| Baseline Training (MobileNetV2)  | Colab GPU      | 20-30 menit    |
| Baseline Training (ResNet50)     | Colab GPU      | 30-45 menit    |
| **Total**                        | **Colab GPU**  | **~2-3 jam**   |
| **Total**                        | **Laptop CPU** | **~5-7 jam**   |

_Kesimpulan_: Google Colab dengan GPU Tesla T4 memberikan efisiensi waktu 2-3× dibanding laptop CPU, menjadikannya pilihan optimal untuk penelitian ini.

---

## DAFTAR PUSTAKA

### Referensi Utama

Badan Pusat Statistik. (2023). _Statistik Kopi Indonesia 2023_. Jakarta: BPS.

Chen, T., & Guestrin, C. (2016). XGBoost: A scalable tree boosting system. In _Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining_ (pp. 785-794). ACM.
| | - Studi literatur | ████░░░░ | | | | | |
| | - Penyusunan proposal | ░░░░████ | | | | | |
| 2 | **Pengumpulan Data** | ░░░░████ | ████░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ |
| | - Download dataset publik | ░░░░████ | | | | | |
| | - Eksplorasi & validasi | ░░░░░░░░ | ████░░░░ | | | | |
| 3 | **Preprocessing Data** | ░░░░░░░░ | ░░░░████ | ████░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ |
| | - Data cleaning | ░░░░░░░░ | ░░░░██░░ | | | | |
| | - Augmentasi & split | ░░░░░░░░ | ░░░░░░██ | ██░░░░░░ | | | |
| 4 | **Pengembangan Model** | ░░░░░░░░ | ░░░░░░░░ | ░░██████ | ████████ | ██░░░░░░ | ░░░░░░░░ |
| | - Feature extraction | ░░░░░░░░ | ░░░░░░░░ | ░░██░░░░ | | | |
| | - XGBoost training | ░░░░░░░░ | ░░░░░░░░ | ░░░░██░░ | ██░░░░░░ | | |
| | - Hyperparameter tuning | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░██ | ░░██░░░░ | | |
| | - Baseline models | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░████ | ██░░░░░░ | |
| 5 | **Evaluasi & Validasi** | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░██████ | ██░░░░░░ |
| | - Testing & metrics | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░██░░░░ | |
| | - Cross-validation | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░██░░ | |
| | - Error analysis | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░██ | ██░░░░░░ |
| 6 | **Dokumentasi & Publikasi** | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░██████ |
| | - Penulisan laporan | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░████░░ |
| | - Presentasi hasil | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░░░ | ░░░░░░██ |

**Keterangan:**

- █ : Periode pelaksanaan kegiatan
- ░ : Tidak ada kegiatan

### Timeline Detail Per Minggu

**Bulan 1 (Minggu 1-4): Persiapan**

- Minggu 1-2: Studi literatur mendalam (review 20+ paper)
- Minggu 3-4: Penyusunan proposal & persiapan environment

**Bulan 2 (Minggu 5-8): Pengumpulan Data**

- Minggu 5: Download dan verifikasi dataset RoboFlow (1.800 citra)
- Minggu 6-7: Eksplorasi data awal dan analisis distribusi kelas
- Minggu 8: Validasi kualitas dataset dan quality check

**Bulan 3 (Minggu 9-12): Preprocessing**

- Minggu 9: Data cleaning, duplicate removal
- Minggu 10: Stratified split dan eksplorasi data
- Minggu 11: Implementasi augmentasi pipeline
- Minggu 12: Feature extraction MobileNetV2

**Bulan 4 (Minggu 13-16): Pengembangan Model**

- Minggu 13: XGBoost initial training
- Minggu 14-15: Hyperparameter tuning (Grid Search)
- Minggu 16: Training baseline models (MobileNetV2 E2E, ResNet50)

**Bulan 5 (Minggu 17-20): Evaluasi**

- Minggu 17: Testing dan kalkulasi metrik evaluasi
- Minggu 18: 5-Fold Cross-Validation
- Minggu 19: Error analysis dan visualisasi
- Minggu 20: Perbandingan dengan baseline

**Bulan 6 (Minggu 21-24): Dokumentasi**

- Minggu 21-22: Penulisan laporan akhir
- Minggu 23: Persiapan presentasi dan poster
- Minggu 24: Presentasi hasil penelitian

### Milestone Utama

| Minggu | Milestone           | Deliverable                       |
| ------ | ------------------- | --------------------------------- |
| 4      | Proposal Approved   | Dokumen proposal lengkap          |
| 8      | Dataset Ready       | 1.800 citra RoboFlow tervalidasi  |
| 12     | Features Extracted  | 1280-D features untuk semua citra |
| 16     | Model Trained       | XGBoost & baseline models         |
| 20     | Evaluation Complete | Comprehensive metrics report      |
| 24     | Research Complete   | Laporan akhir & presentasi        |

---

## DAFTAR PUSTAKA

### Referensi Utama

| **Actual: Cercospora** | FN | FP | TP | FP |
| **Actual: Phoma** | FN | FP | FP | TP |

- **TP (True Positive)**: Prediksi benar untuk kelas positif
- **TN (True Negative)**: Prediksi benar untuk kelas negatif
- **FP (False Positive)**: Salah prediksi sebagai positif (Type I Error)
- **FN (False Negative)**: Salah prediksi sebagai negatif (Type II Error)

---

**2. Akurasi (Accuracy)**

Proporsi prediksi benar dari total prediksi:

$$
\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}
$$

**Interpretasi:**

- Mengukur performa keseluruhan model
- Efektif untuk dataset seimbang
- Bisa misleading pada dataset tidak seimbang

**Target**: ≥ 90%

---

**3. Presisi (Precision)**

Proporsi prediksi positif yang benar:

$$
\text{Precision} = \frac{TP}{TP + FP}
$$

**Interpretasi:**

- Mengukur akurasi prediksi positif
- Penting ketika False Positive costly (misal: diagnosis penyakit salah → treatment tidak perlu)
- Presisi tinggi → sedikit False Alarm

**Target per Kelas**: ≥ 0.88

---

**4. Recall / Sensitivity**

Proporsi kelas positif aktual yang terdeteksi:

$$
\text{Recall} = \frac{TP}{TP + FN}
$$

**Interpretasi:**

- Mengukur kemampuan mendeteksi semua kasus positif
- Penting ketika False Negative costly (misal: penyakit tidak terdeteksi → penyebaran luas)
- Recall tinggi → sedikit miss detection

**Target per Kelas**: ≥ 0.88

---

**5. F1-Score**

Harmonic mean dari Precision dan Recall:

$$
\text{F1-Score} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}
$$

**Interpretasi:**

- Keseimbangan antara Precision dan Recall
- Ideal untuk dataset dengan distribusi tidak seimbang
- F1 tinggi → model balance antara menghindari False Positive dan False Negative

**Target Macro-Average**: ≥ 0.88

**Macro-Average F1:**

$$
\text{F1}_{\text{macro}} = \frac{1}{K} \sum_{k=1}^{K} \text{F1}_k
$$

(Rata-rata F1-Score dari semua kelas, memberikan bobot sama per kelas)

---

**6. Specificity**

Proporsi kelas negatif aktual yang teridentifikasi benar:

$$
\text{Specificity} = \frac{TN}{TN + FP}
$$

**Interpretasi:**

- Mengukur kemampuan model menghindari False Positive
- Penting untuk memastikan daun sehat tidak salah didiagnosis sakit

**Target**: ≥ 0.90

---

#### 3.5.2 Metrik Evaluasi Tambahan

**7. ROC Curve & AUC**

Receiver Operating Characteristic (ROC) Curve:

- **X-axis**: False Positive Rate (FPR) = $\frac{FP}{FP + TN}$
- **Y-axis**: True Positive Rate (TPR) = Recall = $\frac{TP}{TP + FN}$

Area Under Curve (AUC):

- AUC = 1.0 → Classifier sempurna
- AUC = 0.5 → Random classifier
- **Target**: AUC ≥ 0.95 per kelas

---

**8. Precision-Recall Curve**

- Visualisasi trade-off antara Precision dan Recall pada berbagai threshold
- Average Precision (AP): Area under PR Curve
- **Target**: AP ≥ 0.90 per kelas

---

**9. Cohen's Kappa**

Mengukur agreement antara prediksi dan ground truth, adjusted for chance:

$$
\kappa = \frac{p_o - p_e}{1 - p_e}
$$

Dimana:

- $p_o$ = observed agreement (accuracy)
- $p_e$ = expected agreement by chance

**Interpretasi:**

- κ < 0.20 → Poor agreement
- κ = 0.21-0.40 → Fair
- κ = 0.41-0.60 → Moderate
- κ = 0.61-0.80 → Substantial
- κ = 0.81-1.00 → Almost perfect

**Target**: κ ≥ 0.85

---

**10. Matthews Correlation Coefficient (MCC)**

Metrik balanced untuk multi-class:

$$
\text{MCC} = \frac{TP \times TN - FP \times FN}{\sqrt{(TP+FP)(TP+FN)(TN+FP)(TN+FN)}}
$$

**Interpretasi:**

- MCC = +1 → Perfect prediction
- MCC = 0 → Random prediction
- MCC = -1 → Total disagreement

**Target**: MCC ≥ 0.85

---

#### 3.5.3 Metrik Efisiensi

**11. Waktu Komputasi**

| Metrik             | Deskripsi                                  | Target              |
| ------------------ | ------------------------------------------ | ------------------- |
| **Training Time**  | Waktu total untuk training model           | < 1 jam (Colab GPU) |
| **Inference Time** | Waktu prediksi 1 citra                     | < 2 detik           |
| **Throughput**     | Jumlah citra yang dapat diproses per detik | > 0.5 citra/detik   |

**12. Ukuran Model**

| Komponen            | Ukuran      | Catatan                          |
| ------------------- | ----------- | -------------------------------- |
| MobileNetV2 weights | 14 MB       | Pre-trained, frozen              |
| XGBoost model       | < 5 MB      | Tergantung n_estimators          |
| **Total**           | **< 20 MB** | Feasible untuk mobile deployment |

**13. Memori Konsumsi**

- **Training**: < 4 GB RAM (XGBoost)
- **Inference**: < 500 MB RAM
- **GPU VRAM** (opsional): < 2 GB untuk feature extraction

---

#### 3.5.4 Validasi Model

**Stratified 5-Fold Cross-Validation:**

1. Dataset training dibagi menjadi 5 fold
2. Setiap fold mempertahankan proporsi kelas
3. Iterasi 5 kali:
   - Fold ke-i sebagai validation
   - Fold lainnya sebagai training
4. Hitung metrik untuk setiap fold
5. Rata-rata dan standar deviasi metrik:

$$
\text{Mean Accuracy} = \frac{1}{K} \sum_{k=1}^{K} \text{Acc}_k
$$

$$
\text{Std Accuracy} = \sqrt{\frac{1}{K} \sum_{k=1}^{K} (\text{Acc}_k - \overline{\text{Acc}})^2}
$$

**Target CV Stability**: Std < 0.03 (variance rendah → model stabil)

---

#### 3.5.5 Analisis Kesalahan (Error Analysis)

**1. Misclassification Analysis:**

- Identifikasi sampel yang salah klasifikasi
- Visualisasi citra yang paling sulit (lowest confidence)
- Analisis pola kesalahan:
  - Apakah kesalahan pada kelas spesifik?
  - Apakah terjadi confusion antar kelas serupa (misalnya Cercospora vs Phoma)?
  - Apakah kesalahan pada kondisi pencahayaan tertentu?

**2. Confidence Analysis:**

- Histogram distribusi confidence score
- Threshold confidence untuk flagging "uncertain" predictions
- Analisis korelasi confidence vs correctness

**3. Feature Importance:**

- XGBoost menyediakan feature importance score
- Identifikasi fitur MobileNetV2 mana yang paling informatif
- Visualisasi heatmap fitur penting pada citra

---

#### 3.5.6 Perbandingan dengan Baseline

| Metrik                | MobileNetV2-XGBoost (Ours) | MobileNetV2 E2E | ResNet50 E2E |
| --------------------- | -------------------------- | --------------- | ------------ |
| **Accuracy**          | TBD                        | TBD             | TBD          |
| **F1-Score (Macro)**  | TBD                        | TBD             | TBD          |
| **Precision (Macro)** | TBD                        | TBD             | TBD          |
| **Recall (Macro)**    | TBD                        | TBD             | TBD          |
| **AUC (Macro)**       | TBD                        | TBD             | TBD          |
| **Training Time**     | TBD                        | TBD             | TBD          |
| **Inference Time**    | TBD                        | TBD             | TBD          |
| **Model Size**        | TBD                        | 14 MB           | 98 MB        |
| **Train-Test Gap**    | TBD                        | TBD             | TBD          |
| **CV Std**            | TBD                        | TBD             | TBD          |

**Hipotesis:**

1. MobileNetV2-XGBoost akan unggul dalam akurasi (±2-5%) dibanding end-to-end karena regularization XGBoost
2. MobileNetV2-XGBoost akan lebih efisien (training time 50% lebih cepat) karena tidak fine-tune CNN
3. ResNet50 akan lebih rentan overfitting (train-test gap lebih besar) karena parameter lebih banyak
4. MobileNetV2-XGBoost akan lebih stabil (CV std lebih rendah) karena ensemble learning XGBoost

---

#### 3.5.7 Kriteria Keberhasilan Penelitian

**Kriteria Minimal (Must Have):**

- ✅ Akurasi ≥ 90% pada test set
- ✅ F1-Score (macro) ≥ 0.88
- ✅ Precision dan Recall per kelas ≥ 0.85
- ✅ Model size < 20 MB (feasible untuk mobile)
- ✅ Inference time < 2 detik per citra

**Kriteria Optimal (Should Have):**

- ⭐ Akurasi ≥ 93%
- ⭐ F1-Score (macro) ≥ 0.91
- ⭐ AUC ≥ 0.95 per kelas
- ⭐ Unggul 2-3% dibanding baseline
- ⭐ CV stability (std < 0.02)

**Kriteria Tambahan (Nice to Have):**

- 🎯 Interpretability tinggi (feature importance clear)
- 🎯 Robust pada kondisi challenging (low light, complex background)
- 🎯 Ready untuk deployment (API / mobile app)
- 🎯 Dokumentasi lengkap dan reproducible

---

## DAFTAR PUSTAKA

### Referensi Utama

### Buku dan Laporan Pemerintah

Badan Pusat Statistik. (2023). _Statistik Kopi Indonesia 2023_. Jakarta: BPS.

Chen, T., & Guestrin, C. (2016). XGBoost: A scalable tree boosting system. In _Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining_ (pp. 785-794). ACM.

Direktorat Jenderal Perkebunan. (2022). _Statistik Perkebunan Indonesia 2021-2023: Kopi_. Jakarta: Kementerian Pertanian Republik Indonesia.

Kementerian Pertanian Republik Indonesia. (2022). _Outlook Kopi: Komoditas Pertanian Subsektor Perkebunan_. Jakarta: Pusat Data dan Sistem Informasi Pertanian.

Sandler, M., Howard, A., Zhu, M., Zhmoginov, A., & Chen, L. C. (2018). MobileNetV2: Inverted residuals and linear bottlenecks. In _Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR)_ (pp. 4510-4520).

---

### Jurnal Ilmiah

Ferentinos, K. P. (2018). Deep learning models for plant disease detection and diagnosis. _Computers and Electronics in Agriculture_, 145, 311-318. https://doi.org/10.1016/j.compag.2018.01.009

Hughes, D. P., & Salathé, M. (2015). An open access repository of images on plant health to enable the development of mobile disease diagnostics. _arXiv preprint arXiv:1511.08060_.

Kamilaris, A., & Prenafeta-Boldú, F. X. (2018). Deep learning in agriculture: A survey. _Computers and Electronics in Agriculture_, 147, 70-90. https://doi.org/10.1016/j.compag.2018.02.016

Mohanty, S. P., Hughes, D. P., & Salathé, M. (2016). Using deep learning for image-based plant disease detection. _Frontiers in Plant Science_, 7, 1419. https://doi.org/10.3389/fpls.2016.01419

Prasetyo, B., & Rahmadani, S. (2022). Klasifikasi penyakit daun kopi menggunakan deep learning berbasis ResNet50. _Jurnal Teknologi Informasi dan Ilmu Komputer (JTIIK)_, 9(3), 567-574. https://doi.org/10.25126/jtiik.2022935467

Ramcharan, A., McCloskey, P., Baranowski, K., Mbilinyi, N., Mrisho, L., Ndalahwa, M., ... & Hughes, D. P. (2019). A mobile-based deep learning model for cassava disease diagnosis. _Frontiers in Plant Science_, 10, 272. https://doi.org/10.3389/fpls.2019.00272

Saleem, M. H., Potgieter, J., & Arif, K. M. (2020). Plant disease classification: A comparative evaluation of convolutional neural networks and deep learning optimizers. _Plants_, 9(10), 1319. https://doi.org/10.3390/plants9101319

Wang, L., Zhang, Y., & Wang, J. (2021). A hybrid deep learning model combining CNN and XGBoost for medical image classification. _IEEE Access_, 9, 73156-73166. https://doi.org/10.1109/ACCESS.2021.3080234

Zhang, K., Wu, Q., Liu, A., & Meng, X. (2023). Hybrid deep learning and machine learning models for crop disease identification. _Computers and Electronics in Agriculture_, 207, 107715. https://doi.org/10.1016/j.compag.2023.107715

---

### Dataset dan Sumber Online

RoboFlow. (2024). _Coffee Leaf Disease Dataset_. Diakses dari https://universe.roboflow.com/coffee-disease-detection [Diakses 29 November 2025]

TensorFlow. (2024). _MobileNetV2 Pre-trained Models_. Diakses dari https://www.tensorflow.org/api_docs/python/tf/keras/applications/MobileNetV2 [Diakses 29 November 2025]

XGBoost Documentation. (2024). _XGBoost Python API Reference_. Diakses dari https://xgboost.readthedocs.io/ [Diakses 29 November 2025]

---

### Konferensi dan Proceedings

Krizhevsky, A., Sutskever, I., & Hinton, G. E. (2012). ImageNet classification with deep convolutional neural networks. In _Advances in Neural Information Processing Systems_ (Vol. 25, pp. 1097-1105).

LeCun, Y., Bengio, Y., & Hinton, G. (2015). Deep learning. _Nature_, 521(7553), 436-444. https://doi.org/10.1038/nature14539

Simonyan, K., & Zisserman, A. (2015). Very deep convolutional networks for large-scale image recognition. In _International Conference on Learning Representations (ICLR)_.

---

### Literatur Tambahan (Pendukung)

Agarwal, M., Singh, A., Arjaria, S., Sinha, A., & Gupta, S. (2020). ToLeD: Tomato leaf disease detection using convolution neural network. _Procedia Computer Science_, 167, 293-301.

Barbedo, J. G. A. (2018). Impact of dataset size and variety on the effectiveness of deep learning and transfer learning for plant disease classification. _Computers and Electronics in Agriculture_, 153, 46-53.

Chouhan, S. S., Kaul, A., Singh, U. P., & Jain, S. (2018). Bacterial foraging optimization based radial basis function neural network (BRBFNN) for identification and classification of plant leaf diseases: An automatic approach towards plant pathology. _IEEE Access_, 6, 8852-8863.

Goodfellow, I., Bengio, Y., & Courville, A. (2016). _Deep Learning_. Cambridge, MA: MIT Press.

Howard, A. G., Zhu, M., Chen, B., Kalenichenko, D., Wang, W., Weyand, T., ... & Adam, H. (2017). MobileNets: Efficient convolutional neural networks for mobile vision applications. _arXiv preprint arXiv:1704.04861_.

Selvaraj, M. G., Vergara, A., Ruiz, H., Safari, N., Elayabalan, S., Ocimati, W., ... & Blomme, G. (2019). AI-powered banana diseases and pest detection. _Plant Methods_, 15(1), 1-11.

Too, E. C., Yujian, L., Njuki, S., & Yingchun, L. (2019). A comparative study of fine-tuning deep learning models for plant disease identification. _Computers and Electronics in Agriculture_, 161, 272-279.

---

**Catatan Penutup:**

Proposal ini merupakan dokumen penelitian yang disusun untuk merancang sistem diagnosis penyakit daun kopi berbasis AI menggunakan pendekatan hybrid MobileNetV2-XGBoost. Metodologi dirancang untuk menghasilkan model yang akurat, efisien, dan dapat diimplementasikan untuk membantu petani kopi Indonesia dalam mendeteksi penyakit daun secara dini.

_Proposal disusun pada: 29 November 2025_  
_Versi: 2.0 - BAB I-III_

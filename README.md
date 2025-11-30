
# PROPOSAL PENELITIAN
**"Sistem Diagnosis Penyakit Daun Kopi Menggunakan Hybrid MobileNetV2 dan XGBoost Berbasis Citra"**


## BAB I

## PENDAHULUAN

### 1.1 Latar Belakang

Tanaman kopi merupakan salah satu komoditas perkebunan strategis di Indonesia. Data Badan Pusat Statistik (2023) menunjukkan bahwa luas areal perkebunan kopi mencapai lebih dari 1,23 juta hektar dengan produksi tahunan sekitar 773 ribu ton, menjadikan Indonesia sebagai produsen kopi keempat terbesar di dunia. Jenis kopi yang dominan dibudidayakan adalah Coffea arabica (Arabika) dan Coffea canephora (Robusta). Namun, produktivitas perkebunan kopi nasional masih tergolong rendah, yaitu rata-rata 700 kg/ha, jauh di bawah potensi produktivitas optimal 1.500-2.000 kg/ha (Direktorat Jenderal Perkebunan, 2022).
Tanaman kopi merupakan salah satu komoditas perkebunan strategis di Indonesia. Data Badan Pusat Statistik [1] menunjukkan bahwa luas areal perkebunan kopi mencapai lebih dari 1,23 juta hektar dengan produksi tahunan sekitar 773 ribu ton, menjadikan Indonesia sebagai produsen kopi keempat terbesar di dunia. Jenis kopi yang dominan dibudidayakan adalah Coffea arabica (Arabika) dan Coffea canephora (Robusta). Namun, produktivitas perkebunan kopi nasional masih tergolong rendah, yaitu rata-rata 700 kg/ha, jauh di bawah potensi produktivitas optimal 1.500-2.000 kg/ha [3].

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

---


## DAFTAR PUSTAKA

[1] Badan Pusat Statistik, "Statistik Kopi Indonesia 2023," Jakarta: BPS, 2023.
[2] Direktorat Jenderal Perkebunan, "Statistik Perkebunan Indonesia 2021-2023: Kopi," Jakarta: Kementerian Pertanian Republik Indonesia, 2022.
[3] Kementerian Pertanian Republik Indonesia, "Outlook Kopi: Komoditas Pertanian Subsektor Perkebunan," Jakarta: Pusat Data dan Sistem Informasi Pertanian, 2022.
[4] K. P. Ferentinos, "Deep learning models for plant disease detection and diagnosis," Computers and Electronics in Agriculture, vol. 145, pp. 311-318, 2018.
[5] M. Sandler, A. Howard, M. Zhu, A. Zhmoginov, and L. C. Chen, "MobileNetV2: Inverted residuals and linear bottlenecks," in Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR), pp. 4510-4520, 2018.
[6] T. Chen and C. Guestrin, "XGBoost: A scalable tree boosting system," in Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining, pp. 785-794, 2016.
[7] B. Prasetyo and S. Rahmadani, "Klasifikasi penyakit daun kopi menggunakan deep learning berbasis ResNet50," Jurnal Teknologi Informasi dan Ilmu Komputer (JTIIK), vol. 9, no. 3, pp. 567-574, 2022.
[8] A. Ramcharan, P. McCloskey, K. Baranowski, N. Mbilinyi, L. Mrisho, M. Ndalahwa, et al., "A mobile-based deep learning model for cassava disease diagnosis," Frontiers in Plant Science, vol. 10, p. 272, 2019.
[9] L. Wang, Y. Zhang, and J. Wang, "A hybrid deep learning model combining CNN and XGBoost for medical image classification," IEEE Access, vol. 9, pp. 73156-73166, 2021.
[10] K. Zhang, Q. Wu, A. Liu, and X. Meng, "Hybrid deep learning and machine learning models for crop disease identification," Computers and Electronics in Agriculture, vol. 207, p. 107715, 2023.
[11] RoboFlow, "Coffee Leaf Disease Dataset," 2024. [Online]. Available: https://universe.roboflow.com/coffee-disease-detection

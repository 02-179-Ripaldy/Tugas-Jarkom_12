# Tugas Bab 2 – Application Layer  
Buku: *Computer Networking: A Top-Down Approach*  
Halaman: 196–205 (Homework Problems and Questions)

Nama: _(isi sendiri)_  
NIM: _(isi sendiri)_  
Kelas: _(isi sendiri)_  

---

## A. Chapter 2 Review Questions

### SECTION 2.1

**R1. List five nonproprietary Internet applications and the application-layer protocols that they use.**  

Jawab:  
Lima contoh aplikasi Internet nonproprietary dan protokol layer aplikasi yang digunakan adalah sebagai berikut.  

1. **World Wide Web (Web browsing)** – menggunakan protokol **HTTP (HyperText Transfer Protocol)** atau **HTTPS**.  
2. **Surat elektronik (e-mail)** – menggunakan protokol **SMTP (Simple Mail Transfer Protocol)** untuk pengiriman; serta **POP3 (Post Office Protocol v3)** atau **IMAP (Internet Message Access Protocol)** untuk pengambilan.  
3. **Transfer berkas** – menggunakan protokol **FTP (File Transfer Protocol)**.  
4. **Name resolution / pencarian nama domain** – menggunakan protokol **DNS (Domain Name System)**.  
5. **Remote terminal access** – menggunakan protokol **Telnet** atau **SSH (Secure Shell)**.  

---

**R2. What is the difference between network architecture and application architecture?**  

Jawab:  

1. **Network architecture**  
   - Menjelaskan struktur dan layanan yang disediakan oleh jaringan komunikasi secara keseluruhan.  
   - Contoh: arsitektur **TCP/IP**, yang mendefinisikan lapisan‐lapisan seperti link, network (IP), transport (TCP/UDP), dan application.  
   - Berfokus pada **bagaimana host dan router dihubungkan, bagaimana paket dikirim, layanan apa yang diberikan ke aplikasi (misalnya best effort, reliable transport, congestion control, dll.)**.  

2. **Application architecture**  
   - Menjelaskan **cara sebuah aplikasi jaringan dibangun dan terorganisasi**.  
   - Berhubungan dengan penempatan komponen aplikasi pada host dan pola komunikasi antar komponen tersebut.  
   - Contoh: arsitektur **client–server**, **peer-to-peer (P2P)**, atau **hybrid**.  

Perbedaan utamanya: *network architecture* menggambarkan layanan yang diberikan jaringan ke aplikasi, sedangkan *application architecture* menggambarkan **desain logika dan pola interaksi** proses aplikasi yang memanfaatkan layanan jaringan tersebut.

---

**R3. For a communication session between a pair of processes, which process is the client and which is the server?**  

Jawab:  

- Dalam suatu sesi komunikasi, **client** adalah proses yang **menginisiasi komunikasi**, misalnya dengan mengirimkan permintaan (request) pertama.  
- **Server** adalah proses yang **menunggu untuk dihubungi** dan merespons permintaan dari client.  

Dengan demikian, proses yang memulai pertukaran pesan disebut **client**, sementara proses yang pasif menunggu koneksi atau permintaan disebut **server**.

---

**R4. Why are the terms client and server still used in peer-to-peer applications?**  

Jawab:  

- Pada aplikasi **peer-to-peer (P2P)**, setiap host (peer) dapat berperan sebagai **pemberi layanan** maupun **peminta layanan**.  
- Namun, **dalam setiap interaksi tertentu** masih dapat dibedakan peran:  
  - Peer yang **meminta** data atau layanan bertindak sebagai **client**.  
  - Peer yang **memberikan/menyajikan** data atau layanan bertindak sebagai **server**.  

Oleh karena itu, istilah *client* dan *server* tetap digunakan pada P2P untuk **menjelaskan peran sementara** dari masing-masing proses pada saat pertukaran pesan terjadi.

---

**R5. What information is used by a process running on one host to identify a process running on another host?**  

Jawab:  

Sebuah proses pada satu host mengidentifikasi proses di host lain menggunakan kombinasi:  

1. **Alamat IP** dari host tujuan, dan  
2. **Nomor port** dari proses tujuan pada host tersebut.  

Pasangan (**IP address**, **port number**) ini sering disebut sebagai **socket address** dan digunakan oleh protokol transport (TCP/UDP) untuk mengirim data ke proses yang tepat.

---

**R6. What is the role of HTTP in a network application? What other components are needed to complete a Web application?**  

Jawab:  

1. **Peran HTTP**  
   - **HTTP (HyperText Transfer Protocol)** adalah protokol di **lapisan aplikasi** yang mengatur **format** dan **urutan** pesan permintaan (*request*) dan tanggapan (*response*) antara **Web client (browser)** dan **Web server**.  
   - HTTP menentukan bagaimana client meminta objek (HTML, gambar, CSS, JavaScript, dsb.) dan bagaimana server mengirimkannya kembali.  

2. **Komponen lain yang dibutuhkan dalam aplikasi Web**  
   Untuk melengkapi satu aplikasi Web, dibutuhkan beberapa komponen berikut:  
   - **Web browser** (client) yang membentuk permintaan HTTP dan menampilkan konten.  
   - **Web server** (misalnya Apache, Nginx, IIS) yang menerima permintaan HTTP dan mengirim respons.  
   - **Protokol transport dan jaringan**: biasanya **TCP** di atas **IP** sebagai media pengiriman segmen HTTP.  
   - **DNS** untuk menerjemahkan **nama domain** (mis. `www.example.com`) menjadi alamat IP server.  
   - **Objek Web** itu sendiri (file HTML, gambar, video, skrip, stylesheet, dsb.).  
   - (Opsional) **Database** dan **aplikasi sisi server** (PHP, Java Servlet, Node.js, dsb.) untuk konten dinamis.  

---

**R7. Referring to Figure 2.4, we see that none of the applications listed in Figure 2.4 requires both no data loss and timing. Can you conceive of an application that requires no data loss and that is also highly time-sensitive?**  

Jawab:  

- Sebagian besar aplikasi di Gambar 2.4 diklasifikasikan hanya membutuhkan salah satu: **keandalan data** atau **keterbatasan waktu (timing)**, tetapi tidak keduanya sekaligus.  
- Contoh aplikasi yang **membutuhkan kedua syarat**:  

  1. **Sistem kontrol industri real-time**, misalnya pengendalian proses pabrik kimia atau pembangkit listrik, di mana:  
     - Pesan sensor dan perintah kontrol **tidak boleh hilang** (no data loss).  
     - Pesan juga **harus tiba dalam batas waktu tertentu**, karena keterlambatan dapat menyebabkan sistem tidak stabil atau berbahaya.  

  2. **Telemedicine real-time**, misalnya operasi jarak jauh menggunakan robot bedah:  
     - Perintah dokter dan umpan balik sensor harus **lengkap dan akurat**.  
     - Keterlambatan di luar batas tertentu dapat berakibat fatal, sehingga juga **sangat sensitif terhadap waktu**.  

Aplikasi-aplikasi tersebut membutuhkan **reliability** sekaligus **strict timing constraints**, meskipun Internet best-effort standar tidak secara langsung menyediakan dua layanan tersebut secara bersamaan.

---

**R8. List the four broad classes of services that a transport protocol can provide. For each of the service classes, indicate if either UDP or TCP (or both) provides such a service.**  

Jawab:  

Empat kelas besar layanan yang dapat disediakan protokol transport adalah:  

1. **Reliable data transfer (transfer data andal)**  
   - Menjamin bahwa data dikirim tanpa hilang dan tanpa duplikasi, serta dalam urutan yang benar.  
   - **Disediakan oleh:**  
     - **TCP:** Ya (reliable).  
     - **UDP:** Tidak (best-effort, tidak andal).  

2. **Throughput guarantee (jaminan throughput)**  
   - Menjamin tingkat laju data minimum tertentu antara pengirim dan penerima.  
   - Di Internet umum, baik TCP maupun UDP **tidak memberikan** jaminan throughput yang eksplisit; mereka hanya berbagi bandwidth jaringan yang tersedia.  
   - **Disediakan oleh:**  
     - **TCP:** Tidak (hanya melakukan congestion control, bukan guarantee).  
     - **UDP:** Tidak.  

3. **Timing / delay guarantee (jaminan waktu/delay)**  
   - Menyediakan batas atas tertentu terhadap waktu pengiriman (end-to-end delay) untuk data yang dikirim.  
   - **Disediakan oleh:**  
     - **TCP:** Tidak.  
     - **UDP:** Tidak.  
   - Catatan: Jaminan timing biasanya merupakan fungsi dari jaringan khusus (mis. QoS, real-time network), bukan sekadar protokol TCP/UDP itu sendiri.  

4. **Security (keamanan data)**  
   - Layanan seperti kerahasiaan (encryption), integritas, dan autentikasi.  
   - Dalam praktiknya, keamanan **ditambahkan** melalui protokol seperti **TLS** yang berjalan di atas TCP. Secara dasar:  
     - **TCP:** Tidak menyediakan keamanan bawaan; namun dapat digunakan bersama TLS (**TLS over TCP**).  
     - **UDP:** Demikian pula tidak menyediakan keamanan bawaan; dapat dipadukan dengan mekanisme lain (mis. DTLS).  

Ringkas:  
- **TCP:** menyediakan **reliable data transfer**, tetapi tidak menyediakan jaminan throughput dan timing, serta tidak memiliki keamanan built-in (namun dapat dikombinasikan dengan TLS).  
- **UDP:** tidak menyediakan reliability, throughput guarantee, maupun timing guarantee, dan juga tidak memiliki keamanan built-in.

---

**R9. Recall that TCP can be enhanced with TLS to provide process-to-process security services, including encryption. Does TLS operate at the transport layer or the application layer? If the application developer wants TCP to be enhanced with TLS, what does the developer have to do?**  

Jawab:  

1. **Letak TLS dalam model lapisan**  
   - **TLS (Transport Layer Security)** secara konseptual berada **di atas TCP**, sehingga sering dianggap bagian dari **lapisan aplikasi** atau sebagai lapisan “antara” aplikasi dan transport.  
   - TCP tetap bertanggung jawab atas pengiriman data andal; TLS menambahkan **enkripsi**, **integritas**, dan **autentikasi** di atasnya.  

2. **Apa yang harus dilakukan pengembang aplikasi?**  
   - Pengembang tidak memodifikasi TCP secara langsung.  
   - Pengembang cukup menggunakan **library atau API TLS** (misalnya OpenSSL) sehingga aplikasi:  
     - Membuat **socket TCP** seperti biasa, lalu  
     - **“Membungkus” (wrap)** socket tersebut dengan TLS (membangun *TLS session/handshake*), dan  
     - Mengirim serta menerima data melalui lapisan TLS tersebut.  

Dengan demikian, dari sudut pandang aplikasi, ia berkomunikasi melalui **“secure TCP connection”** tanpa perlu mengubah implementasi protokol TCP di sistem operasi.

---

### SECTIONS 2.2–2.5

**R10. What is meant by a handshaking protocol?**  

Jawab:  

- **Handshaking protocol** adalah protokol yang melakukan **pertukaran pesan awal** antara dua entitas (misalnya antara client dan server) sebelum data aplikasi utama dikirim.  
- Tujuan handshaking adalah untuk:  
  1. Menegosiasikan parameter koneksi (versi protokol, opsi, cipher suite, dsb.).  
  2. Menyepakati pembentukan hubungan logis (misalnya koneksi TCP).  
  3. Memastikan kedua pihak **siap** dan **sepakat** terhadap aturan komunikasi.  

Contoh: **three-way handshake TCP (SYN, SYN-ACK, ACK)** yang dilakukan sebelum data aplikasi dikirimkan.

---

**R11. What does a stateless protocol mean? Is IMAP stateless? What about SMTP?**  

Jawab:  

1. **Pengertian stateless protocol**  
   - Protokol dikatakan **stateless** jika **server tidak menyimpan informasi keadaan (state)** tentang client di antara permintaan yang satu dengan yang lain.  
   - Setiap permintaan harus berisi semua informasi yang diperlukan, sehingga server dapat memprosesnya **tanpa mengandalkan riwayat** interaksi sebelumnya.  

2. **IMAP**  
   - IMAP **bukan** protokol yang sepenuhnya stateless.  
   - Dalam IMAP, setelah client melakukan login, server mempertahankan **state sesi**, seperti:  
     - mailbox yang sedang dibuka,  
     - pesan mana yang sudah dibaca atau diberi flag, dan sebagainya.  
   - Jadi, IMAP cenderung **stateful** selama sesi berlangsung.  

3. **SMTP**  
   - SMTP juga **mempertahankan state selama satu sesi pengiriman e‑mail** (misalnya informasi pengirim, penerima, dan status transaksi).  
   - Namun, setelah sesi koneksi SMTP selesai, server **tidak menyimpan state tentang sesi tersebut** untuk permintaan berikutnya dari client yang sama.  
   - Dengan demikian, dari sudut pandang antar-sesi, SMTP dapat dianggap **tidak menyimpan state jangka panjang**, tetapi **stateful di dalam satu sesi**.  

---

**R12. How can websites keep track of users? Do they always need to use cookies?**  

Jawab:  

1. **Cara website melacak pengguna**  
   Website dapat melacak dan mengidentifikasi pengguna dengan beberapa mekanisme, antara lain:  
   - **Cookies HTTP**: server mengirimkan *cookie* ke browser melalui header `Set-Cookie`. Browser menyimpannya dan mengirim kembali pada permintaan berikutnya, sehingga server dapat mengenali pengguna yang sama.  
   - **Session ID** pada URL atau form: server menyematkan *session identifier* ke dalam URL (`?sessionid=...`) atau field tersembunyi dalam form.  
   - **Local Storage / Web Storage**: menyimpan data di sisi browser (HTML5).  
   - **Teknik fingerprinting**: kombinasi informasi browser (User-Agent, resolusi layar, plugin, dsb.).  

2. **Apakah selalu harus menggunakan cookies?**  
   - Tidak selalu.  
   - Meskipun **cookies adalah mekanisme paling umum**, server dapat menggunakan metode lain (misalnya *session ID* dalam URL atau token autentikasi pada header) untuk melacak sesi pengguna.  
   - Namun, cookies memberikan cara yang **standar, praktis, dan didukung luas** oleh browser, sehingga paling sering digunakan.

---

**R13. Describe how Web caching can reduce the delay in receiving a requested object. Will Web caching reduce the delay for all objects requested by a user or for only some of the objects? Why?**  

Jawab:  

1. **Cara Web caching mengurangi delay**  
   - **Web cache (proxy cache)** menyimpan salinan objek Web yang sering diminta (misalnya file HTML, gambar).  
   - Ketika client meminta objek, permintaan dikirim ke cache terlebih dahulu:  
     - Jika objek **sudah tersedia (cache hit)**: cache langsung mengirimkannya ke client, sehingga perjalanan data hanya antara client–cache, yang biasanya lebih dekat (lebih sedikit *hop* dan delay).  
     - Jika objek **belum tersedia (cache miss)**: cache akan mengambilnya dari server asal (origin server), menyimpannya, lalu meneruskan ke client.  
   - Untuk permintaan berikutnya atas objek yang sama, delay jauh lebih kecil karena tidak perlu lagi mengambil dari server asal.  

2. **Apakah mengurangi delay untuk semua objek?**  
   - Web caching **tidak selalu** mengurangi delay untuk **semua** objek yang diminta pengguna.  
   - Cache hanya mengurangi delay secara signifikan untuk:  
     - Objek yang **populer** dan **sering diakses**, sehingga kemungkinan *cache hit* tinggi.  
     - Objek yang **relatif statis**, sehingga dapat disimpan dalam jangka waktu cukup lama.  
   - Untuk objek yang **unik, jarang diakses, atau sangat dinamis**, peluang *cache hit* kecil sehingga delay mungkin tidak berkurang atau bahkan sedikit bertambah karena overhead pengecekan ke cache.  

Kesimpulannya, Web caching terutama mengurangi delay bagi **sebagian objek** (yang populer dan cacheable), bukan untuk semua objek tanpa kecuali.

---

**R14. Telnet into a Web server and send a multiline request message. Include in the request message the `If-Modified-Since:` header line to force a response message with the `304 Not Modified` status code.**  

Jawab (contoh langkah dan format pesan):  

1. **Membuka koneksi Telnet ke server Web (port 80)**  
   ```text
   telnet www.contoh.com 80
   ```  

2. **Mengirim permintaan HTTP secara manual**  
   Ketik pesan HTTP berikut (diakhiri dengan baris kosong dua kali untuk menandai akhir header):  

   ```http
   GET /index.html HTTP/1.1
   Host: www.contoh.com
   If-Modified-Since: Wed, 21 Oct 2015 07:28:00 GMT
   User-Agent: Telnet-Client
   Connection: close

   ```

   Penjelasan:  
   - Baris pertama adalah **request line**: metode `GET`, path `/index.html`, dan versi `HTTP/1.1`.  
   - `Host:` harus diisi dengan nama host yang sesuai.  
   - `If-Modified-Since:` diisi dengan tanggal/waktu tertentu. Jika file di server **belum diubah sejak waktu tersebut**, server akan mengirim **status code `304 Not Modified`** dan **tidak mengirim isi file**.  
   - Tambahkan satu baris kosong di akhir header untuk menandai akhir pesan.  

3. **Respons yang diharapkan (contoh)**  

   ```http
   HTTP/1.1 304 Not Modified
   Date: Fri, 23 Nov 2025 06:00:00 GMT
   Server: Apache/2.4.57 (Unix)
   Connection: close

   ```

   Status line `304 Not Modified` menunjukkan bahwa objek belum berubah sejak waktu yang ditentukan pada header `If-Modified-Since:`.

---

**R15. Are there any constraints on the format of the HTTP body? What about the format of the HTTP header? Why must an HTTP Web server and client use the same HTTP version? How can arbitrary data be transmitted over SMTP?**  

Jawab:  

1. **Keterbatasan format HTTP body**  
   - **Secara prinsip, HTTP tidak membatasi format body**; body dapat berisi teks, gambar biner, video, JSON, dan sebagainya.  
   - Format isi ditentukan melalui header seperti `Content-Type`, misalnya `text/html`, `image/jpeg`, `application/json`, dll.  

2. **Keterbatasan format HTTP header**  
   - Header HTTP memiliki format **teks ASCII** dengan struktur:  
     - Satu baris **status line** atau **request line**.  
     - Diikuti sejumlah baris header dengan format `Nama-Header: nilai`.  
     - Diakhiri dengan **baris kosong**.  
   - Karakter kontrol tertentu (misalnya CRLF) digunakan untuk memisahkan baris.  
   - Dengan kata lain, format header **ketat** dan harus mengikuti standar agar dapat diparse dengan benar oleh client dan server.  

3. **Mengapa server dan client harus menggunakan versi HTTP yang sama atau kompatibel?**  
   - Versi HTTP (mis. HTTP/1.0, HTTP/1.1, HTTP/2) menentukan:  
     - Fitur yang tersedia (mis. koneksi persisten, pipelining, header tambahan).  
     - Cara interpretasi beberapa header dan perilaku koneksi.  
   - Jika client dan server menggunakan versi yang **tidak kompatibel**, salah satu pihak mungkin **tidak memahami** format pesan dari pihak lain sehingga komunikasi gagal.  
   - Oleh karena itu, keduanya harus menggunakan **versi yang sama atau saling kompatibel**, dan biasanya versi dinyatakan di request/response line.  

4. **Bagaimana data arbitrer dapat dikirim melalui SMTP?**  

   - SMTP aslinya dirancang untuk **teks ASCII 7-bit**. Untuk mengirim data biner (gambar, audio, file), dibutuhkan mekanisme tambahan.  
   - Solusinya adalah standar **MIME (Multipurpose Internet Mail Extensions)**, yang:  
     - Mengenkode data biner ke dalam bentuk teks (misalnya **Base64**).  
     - Menyertakan header tambahan yang menunjukkan tipe konten (`Content-Type`), metode encoding (`Content-Transfer-Encoding`), batas antar bagian (`boundary`), dll.  
   - Dengan MIME, **data sebarang (arbitrary data)** dapat dikemas menjadi representasi teks yang sesuai sehingga dapat dikirim melalui SMTP dan kemudian didekode kembali di sisi penerima.

---

## B. Socket Programming Assignments

### P30. Can you configure your browser to open multiple simultaneous connections to a Web site? What are the advantages and disadvantages of having a large number of simultaneous TCP connections?  

Jawab:  

1. **Konfigurasi browser untuk koneksi simultan**  
   - Sebagian besar browser modern (Chrome, Firefox, Edge, dsb.) **secara otomatis membuka beberapa koneksi TCP paralel** ke suatu situs Web untuk mempercepat pemuatan halaman (misalnya 6–10 koneksi per host).  
   - Pada beberapa browser, jumlah maksimum koneksi per host **dapat diubah** melalui pengaturan lanjutan (contoh: `about:config` di Firefox) atau melalui kebijakan konfigurasi.  

2. **Keuntungan memiliki banyak koneksi TCP simultan**  
   - **Pemuatan halaman lebih cepat**, karena:  
     - Banyak objek (gambar, CSS, JavaScript, dsb.) dapat diunduh **secara paralel**, bukan berurutan.  
   - **Memperbaiki waktu respon** jika ada objek yang lambat diunduh, karena objek lain dapat tetap diambil melalui koneksi yang berbeda.  

3. **Kerugian memiliki terlalu banyak koneksi TCP simultan**  
   - **Overhead sumber daya** di sisi client dan server:  
     - Setiap koneksi membutuhkan memori (buffer), CPU untuk pengelolaan segmen, dan *state* tambahan.  
   - **Meningkatkan kemacetan (congestion)** di jaringan:  
     - Terlalu banyak koneksi dapat memperbanyak paket yang bersaing di jalur yang sama.  
   - **Tidak adil (unfairness)** terhadap aplikasi lain:  
     - Satu browser yang membuka banyak koneksi ke server yang sama dapat mengambil porsi bandwidth yang lebih besar dibanding aplikasi lain yang hanya menggunakan sedikit koneksi.  

Karena itu, browser umumnya membatasi **jumlah maksimum koneksi paralel** sehingga masih seimbang antara kinerja dan penggunaan sumber daya jaringan.

---

### P31. We have seen that Internet TCP sockets treat the data being sent as a byte stream but UDP sockets recognize message boundaries. What are one advantage and one disadvantage of byte-oriented API versus having the API explicitly recognize and preserve application-defined message boundaries?  

Jawab:  

1. **Kelebihan API berorientasi byte-stream (seperti TCP)**  

   - **Sederhana dan fleksibel**:  
     - Aplikasi tidak perlu memikirkan bagaimana data dipecah menjadi paket-paket di jaringan; ia hanya menulis dan membaca deretan byte.  
   - **Mudah menangani data berukuran besar**:  
     - Data yang sangat panjang dapat dikirim sebagai satu aliran tanpa memperhatikan batas pesan tetap.  
   - Pengembang dapat mendefinisikan **format pesan sendiri** di atas byte-stream (misalnya menambahkan header panjang pesan).  

2. **Kekurangan API berorientasi byte-stream**  

   - **Aplikasi harus menentukan dan mengelola sendiri batas pesan**:  
     - Karena penerima dapat memperoleh data dalam potongan (chunk) yang tidak sama dengan “pesan logis”, aplikasi harus:  
       - Membaca byte secara berulang,  
       - Menentukan kapan satu pesan dianggap lengkap (misalnya berdasarkan panjang yang tertera di header atau delimiter khusus).  
   - Hal ini menambah **kompleksitas pemrograman aplikasi** dibanding API yang sudah secara eksplisit mengenali batas pesan (seperti pada UDP).  

3. **Kelebihan API yang mengenali batas pesan (message-oriented, seperti UDP)**  

   - Setiap kali aplikasi memanggil operasi baca, ia bisa menerima **satu pesan lengkap** (asalkan buffer cukup besar), sehingga lebih mudah mengelola struktur pesan.  

4. **Kekurangan API message-oriented**  

   - Ukuran pesan harus memperhatikan batas maksimum (maximum message size).  
   - Jika pesan terfragmentasi atau melebihi ukuran tertentu, penanganannya menjadi lebih rumit atau menyebabkan pemotongan.  

Intinya, byte-stream API (TCP) memberikan **fleksibilitas tinggi** tetapi membuat aplikasi **bertanggung jawab untuk mengelola batas pesan sendiri**.

---

### P32. What is the Apache Web server? How much does it cost? What functionality does it currently have?  

Jawab:  

1. **Apa itu Apache Web server?**  
   - **Apache HTTP Server** (sering disebut **Apache**) adalah perangkat lunak **Web server** open-source yang sangat populer dan banyak digunakan di Internet.  
   - Apache berfungsi untuk **menerima permintaan HTTP/HTTPS dari client (browser)** dan **mengirimkan kembali respons** berupa konten Web (HTML, gambar, file, dsb.).  

2. **Biaya**  
   - Apache didistribusikan di bawah **Apache License**, yang merupakan lisensi open-source.  
   - Perangkat lunak ini dapat **digunakan, dimodifikasi, dan didistribusikan secara bebas** tanpa biaya lisensi; dengan kata lain, **gratis**.  

3. **Fungsionalitas yang dimiliki Apache saat ini (secara umum)**  
   - **Melayani konten statis**: file HTML, gambar, CSS, JavaScript, dan berbagai tipe file lainnya.  
   - **Mendukung konten dinamis** melalui modul dan integrasi dengan berbagai bahasa pemrograman, misalnya:  
     - PHP, Perl, Python, Ruby, dan lain-lain (melalui modul `mod_php`, `mod_perl`, `mod_wsgi`, CGI, FastCGI, dsb.).  
   - **Dukungan HTTPS / TLS**:  
     - Melalui modul seperti `mod_ssl`, Apache dapat menyediakan koneksi aman (HTTPS).  
   - **Virtual hosting**:  
     - Satu server fisik bisa melayani **banyak nama domain** atau situs berbeda.  
   - **Modular architecture**:  
     - Fungsionalitas dapat diperluas lewat berbagai modul (authentication, proxy, rewrite, caching, logging, dsb.).  
   - **Logging dan kontrol akses**:  
     - Menyediakan log akses dan error yang lengkap, serta mekanisme autentikasi dan otorisasi pengguna.  
   - **Reverse proxy dan load balancing** (dengan modul tertentu), sehingga bisa berfungsi sebagai front-end bagi aplikasi lain di belakangnya.  

Dengan karakteristik tersebut, Apache menjadi salah satu Web server standar di berbagai sistem operasi (Linux, Unix, Windows) dan banyak digunakan baik untuk keperluan pengembangan maupun produksi.

---

## C. Penutup

Dokumen ini berisi jawaban atas soal‑soal:  
- **Chapter 2 Review Questions:** R1–R15.  
- **Socket Programming Assignments:** P30–P32.  

Jawaban ditulis dalam bahasa Indonesia formal dan disusun dengan penomoran sesuai urutan soal sehingga dapat langsung digunakan sebagai laporan atau tugas.

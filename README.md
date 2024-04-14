# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

**Refleksi Wajib (Penerbit)**

1. Dalam pola Observer yang dijelaskan oleh buku Head First Design Pattern, Subscriber didefinisikan sebagai sebuah antarmuka. Berdasarkan pemahaman Anda tentang pola Observer, apakah kita masih memerlukan antarmuka (atau **trait** dalam Rust) dalam kasus BambangShop ini, atau satu Model **struct** sudah cukup?
    - Pola Observer memungkinkan objek untuk mendapatkan notifikasi tentang perubahan pada objek lain. Biasanya, ini menggunakan **trait** dalam Rust atau **interface** untuk mendefinisikan metode yang diinginkan.
    - Penggunaan **interface** atau **trait** dalam BambangShop tidak sepenuhnya diperlukan karena tidak ada variasi dalam perilaku yang diharapkan dari observer.
    - Sebagai alternatif, penggunaan satu **struct** Model yang mencakup data mengenai observer dan perilaku yang diperlukan untuk memberikan notifikasi sudah cukup. Jadi, penggunaan satu **struct** Model untuk observer sudah memadai.

2. **id** dalam **Program** dan **url** dalam **Subscriber** dimaksudkan untuk unik. Berdasarkan pemahaman Anda, apakah menggunakan **Vec** (daftar) sudah cukup atau menggunakan **DashMap** (peta/kamus) seperti yang saat ini digunakan diperlukan untuk kasus ini?
    - Jika hanya ingin menyimpan ID atau URL dari pelanggan, penggunaan Vec (daftar) sudah cukup. Namun, DashMap (peta/kamus) lebih disarankan karena memberikan keunggulan dalam kecepatan akses dan manipulasi data.
    - Dengan menggunakan DashMap, pengecekan terhadap ID atau URL yang sudah ada menjadi lebih sederhana. Selain itu, DashMap juga menyediakan akses dan pembaruan entri yang sudah ada dengan kecepatan yang lebih tinggi, yang dapat meningkatkan efisiensi dan kinerja aplikasi.

3. Saat pemrograman menggunakan Rust, kita dipaksa oleh batasan kompiler yang ketat untuk membuat program yang aman terhadap thread. Dalam kasus daftar Subscriber (**SUBSCRIBERS**) variabel statis, kita menggunakan pustaka eksternal **DashMap** untuk **HashMap yang aman terhadap thread**. Berdasarkan pemahaman Anda tentang pola desain, apakah kita masih memerlukan **DashMap** atau kita dapat mengimplementasikan pola Singleton sebagai gantinya?
    - Pola Singleton menjamin bahwa sebuah kelas hanya memiliki satu instansi di seluruh aplikasi.
    - Penggunaan DashMap untuk variabel statis SUBSCRIBERS merupakan pilihan terbaik karena memungkinkan akses ke struktur data yang sama dari mana pun dalam aplikasi serta menjaga keamanan konkurensi. Jadi, DashMap memberikan solusi yang efektif untuk mengelola keamanan konkurensi dalam lingkungan multi-threaded yang penting dalam pengembangan program Rust.
    - Meskipun pola Singleton dapat diimplementasikan dalam Rust, penggunaannya tidak terlalu diperlukan dalam kasus ini karena DashMap sudah menyediakan fungsionalitas yang diperlukan. Jadi, penggunaan DashMap adalah opsi yang tepat untuk kebutuhan variabel statis SUBSCRIBERS dalam BambangShop.

#### Reflection Publisher-2

**Refleksi Penerbit-2**

1. Dalam pola gabungan Model-View-Controller (MVC), tidak ada "Service" dan "Repository". Model dalam MVC mencakup penyimpanan data dan logika bisnis. Berdasarkan pemahaman Anda tentang prinsip desain, mengapa kita perlu memisahkan "Service" dan "Repository" dari Model?

   Prinsip SRP (Single Responsibility Principle) menyarankan bahwa setiap kelas harus hanya memiliki satu alasan untuk berubah, yang berarti fokus pada satu tanggung jawab. Oleh karena itu, pemisahan "Service" dan "Repository" dari Model diperlukan agar Model hanya fokus pada definisi struktur data.
   Prinsip desain menyarankan pemisahan komponen untuk memastikan bahwa setiap bagian memiliki tanggung jawabnya sendiri. Dengan memindahkan logika bisnis ke "Service" dan operasi penyimpanan data ke "Repository", Model menjadi representasi murni dari struktur data.
   Pendekatan ini memberikan fleksibilitas pada Model untuk beradaptasi dengan perubahan dalam struktur atau penyimpanan data tanpa terbebani oleh metode yang tidak relevan.

2. Apa yang terjadi jika kita hanya menggunakan Model? Jelaskan imajinasi Anda tentang bagaimana interaksi antara setiap model (Program, Subscriber, Notification) mempengaruhi kompleksitas kode untuk setiap model?

   Jika kita bergantung hanya pada Model tanpa membagi tanggung jawab ke "Service" dan "Repository", maka Model akan terbebani dengan tanggung jawab ganda: representasi data dan logika bisnis. Ini akan menciptakan kelas-kelas yang memiliki terlalu banyak tanggung jawab, bertentangan dengan prinsip SRP.
   Akibatnya, kompleksitas kode akan meningkat, pemeliharaan dan perluasan kode menjadi lebih sulit, interaksi antar-model menjadi lebih rumit dan saling terikat, serta ketergantungan yang kuat antar-kode.
   Sebagai contoh, jika suatu Produk perlu memberitahu Subscriber tentang perubahan, interaksi langsung dengan objek Subscriber untuk mengirimkan Notifikasi akan melanggar prinsip enkapsulasi dan membuat kode sulit dipahami.
   Tanpa pembagian tanggung jawab yang tepat, kompleksitas kode untuk setiap Model akan meningkat, membuat kode menjadi sulit untuk dipahami, diuji, dan dipelihara.

3. Apakah Anda telah menjelajahi lebih lanjut tentang Postman? Ceritakan bagaimana alat ini membantu Anda menguji pekerjaan Anda saat ini. Anda mungkin juga ingin menyebutkan fitur-fitur dalam Postman yang Anda minati atau merasa bermanfaat untuk membantu Proyek Kelompok Anda atau proyek rekayasa perangkat lunak masa depan Anda.

   Postman membantu dalam pengujian program dengan memungkinkan pengiriman respons ke endpoint API, memfasilitasi simulasi tanpa memerlukan HTML.
   Alat ini memungkinkan pengiriman permintaan HTTP ke endpoint API dengan parameter yang dapat disesuaikan, termasuk header, parameter query, dan isi permintaan.
   Postman juga dilengkapi dengan fitur pengujian otomatis yang memungkinkan pembuatan dan pelaksanaan serangkaian pengujian otomatis untuk memverifikasi perilaku endpoint API, termasuk status kode respons dan isi body respons.
   Fitur variabel lingkungan dalam Postman memudahkan pengguna untuk beralih antara lingkungan yang berbeda, seperti pengembangan, staging, dan produksi saat menguji API.

#### Reflection Publisher-3

1. Pola Observer memiliki dua varian: model Push (penerbit mendorong data ke pelanggan) dan model Pull (pelanggan menarik data dari penerbit). Dalam kasus tutorial ini, varian Pola Observer mana yang kita gunakan?

   Dalam modul ini, kita menerapkan varian model Push dari Pola Observer, yang terlihat dari cara kita mengirim notifikasi ke pelanggan melalui permintaan HTTP POST setelah mereka berlangganan. Notifikasi ini diaktifkan oleh aksi seperti pembuatan, promosi, atau penghapusan produk.

2. Apa kelebihan dan kekurangan menggunakan varian lain dari Pola Observer untuk kasus tutorial ini? (contoh: jika jawaban Q1 adalah Push, bayangkan jika kita menggunakan Pull)

   Kelebihan menggunakan model Pull adalah pengurangan dalam konsumsi sumber daya jaringan dan komputasi karena pelanggan hanya mengambil data ketika mereka membutuhkannya. Ini memberi pelanggan kontrol penuh atas waktu pengambilan data, memungkinkan mereka untuk menghindari pengambilan data yang tidak perlu.
   Kekurangan dari model Pull adalah pelanggan harus aktif meminta pembaruan, yang bisa menyebabkan penundaan dalam menerima informasi terkini. Ini bisa menjadi tidak efisien ketika pembaruan diperlukan dengan segera. Selain itu, implementasi model Pull mungkin memerlukan logika tambahan di sisi pelanggan untuk mengelola permintaan dan pembaruan data secara efektif.

3. Jelaskan apa yang akan terjadi pada program jika kita memutuskan untuk tidak menggunakan multi-threading dalam proses notifikasi.

   Tanpa multi-threading, proses notifikasi akan berlangsung secara berurutan, di mana setiap notifikasi diproses satu demi satu. Ini berarti kita harus menunggu penyelesaian notifikasi sebelumnya sebelum memulai yang berikutnya.
   Jika ada banyak notifikasi yang perlu dikirim, proses ini bisa menjadi sangat lambat dan menyebabkan keterlambatan dalam respons.
   Dengan menggunakan multi-threading, notifikasi dapat diproses secara paralel, yang akan mempercepat proses dan meningkatkan responsivitas aplikasi. Ini juga lebih efisien ketika perlu mengirim banyak notifikasi sekaligus.



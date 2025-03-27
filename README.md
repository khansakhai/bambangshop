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
-   [X] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X] Commit: `Create Subscriber model struct.`
    -   [X] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [X] Commit: `Implement add function in Subscriber repository.`
    -   [X] Commit: `Implement list_all function in Subscriber repository.`
    -   [X] Commit: `Implement delete function in Subscriber repository.`
    -   [X] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
    -   [X] Commit: `Implement unsubscribe function in Notification service.`
    -   [X] Commit: `Implement unsubscribe function in Notification controller.`
    -   [X] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [X] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [X] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [X] Commit: `Implement publish function in Program service and Program controller.`
    -   [X] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [X] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

>In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Dalam pola desain Observer, Subscriber biasanya didefinisikan sebagai interface (atau trait di Rust) agar bisa mengabstraksi cara langganan dan menerima pembaruan. Namun, dalam kasus BambangShop ini, saya rasa tidak perlu membuat interface atau trait khusus. Cukup dengan menggunakan struct model Subscriber yang sudah ada, karena sudah bisa langsung menyimpan data yang dibutuhkan tanpa perlu pengabstraksian tambahan.

>id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Kalau hanya menggunakan `Vec`, kita harus mencari item satu per satu, yang tentu tidak efisien, apalagi kalau datanya banyak. Dengan menggunakan `DashMap`, pencarian menjadi lebih cepat karena DashMap memungkinkan kita mencari data berdasarkan kunci unik seperti id atau *url*, dan memastikan data tersebut tidak duplikat. Jadi, menurut saya, penggunaan DashMap lebih tepat untuk kasus ini.

>When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Meskipun kita bisa menggunakan pola Singleton untuk memastikan hanya ada satu instance dari data, menurut saya, menggunakan DashMap di sini lebih praktis karena sudah menyediakan fitur thread safety secara otomatis. Jika pakai Singleton, saya harus menangani sinkronisasi secara manual dengan Mutex atau `RwLock`, yang bisa lebih rumit. Sementara itu, DashMap sudah menangani semua itu dengan lebih mudah dan efisien.

#### Reflection Publisher-2

>In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Menurut saya, memisahkan Service dan Repository dari Model membantu membuat kode lebih terstruktur dan mudah dipelihara. Repository fokus menangani penyimpanan data, sedangkan Service fokus pada aturan atau proses aplikasi. Dengan begitu, setiap bagian punya tanggung jawab yang jelas dan tidak saling tumpang tindih, sehingga lebih mudah untuk menguji dan mengembangkan fitur baru tanpa merusak bagian lainnya.

>What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Kalau kita hanya menggunakan Model, maka semua aturan dan penyimpanan data akan tercampur dalam satu file, dan menurut saya ini akan membuat kode jadi sangat kompleks. Interaksi antara `Program`, `Subscriber`, dan Notification akan saling terikat langsung, membuat perubahan kecil di satu model bisa berdampak besar ke model lain. Ini akan menyulitkan debugging dan pengembangan ke depannya.

>Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Saya sudah coba pakai Postman untuk menguji endpoint subscribe dan `unsubscribe`, dan menurut saya Postman sangat membantu karena saya bisa langsung kirim request HTTP dengan data JSON tanpa perlu buat tampilan atau frontend terlebih dahulu. Fitur yang paling membantu buat saya adalah kemampuan menyimpan *collection request*, melihat response dengan jelas, dan fitur Environment untuk menyimpan variabel seperti URL dasar. Saya merasa ini akan sangat berguna untuk proyek kelompok maupun tugas backend saya selanjutnya.

#### Reflection Publisher-3

>Observer Pattern has two variations: **Push model** (publisher pushes data to subscribers) and **Pull model** (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

Berdasarkan tutorial ini, saya menggunakan **Push model** dari Observer Pattern. Dalam model ini, publisher secara langsung mengirimkan data notifikasi ke semua subscriber ketika terjadi perubahan, seperti saat produk dibuat, dipromosikan, atau dihapus. Model ini sesuai karena aplikasi butuh langsung mengabari semua subscriber tanpa menunggu mereka mengecek sendiri.

>What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

Jika kita menggunakan **Pull model**, maka subscriber harus secara aktif meminta atau mengecek data dari publisher secara berkala. Keuntungannya adalah publisher tidak perlu tahu siapa saja subscribernya, jadi bisa lebih sederhana. Tetapi, menurut saya kekurangannya cukup besar dalam kasus ini karena notifikasi tidak akan sampai secara *real-time*, dan bisa membuat subscriber ketinggalan informasi atau terlalu sering melakukan pengecekan yang tidak efisien.

>Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Jika proses notifikasi tidak menggunakan *multi-threading*, maka setiap notifikasi ke subscriber akan dilakukan satu per satu secara berurutan. Menurut saya, ini akan memperlambat sistem secara keseluruhan, terutama jika jumlah subscriber banyak. Selain itu, proses lain di aplikasi bisa ikut terhambat karena harus menunggu proses notifikasi selesai terlebih dahulu sebelum lanjut menjalankan hal lainnya.
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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or **trait** in Rust) in this BambangShop case, or a single Model **struct** is enough?

    Pada Observer design pattern, penggunaan interface atau trait biasanya bergantung pada kebutuhan variasi perilaku antara observer. Dalam kasus BambangShop hanya ada satu kelas Observer, yaitu Subscriber, tanpa variasi perilaku. Interface atau trait tidak diperlukan karena tidak ada kebutuhan untuk variasi perilaku antara Subscriber. Sebagai gantinya, satu struktur Model yang mencakup data dan perilaku observer sudah cukup efektif.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

    Penggunakan DashMap dalam kasus ini lebih efisien untuk pemetaan antara Product dan Subscriber. Dengan menggunakan DashMap, kita dapat langsung melakukan pemetaan tanpa perlu membuat dua Vec terpisah untuk setiap produk. DashMap memudahkan akses dan pemeliharaan data, sehingga meningkatkan kinerja aplikasi.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

    Menggunakan DashMap adalah pilihan terbaik untuk memastikan keamanan dalam lingkungan multithreading. DashMap menyediakan solusi yang baik untuk keamanan concurrency pada multithreaded environment dalam program Rust. Singleton pattern dapat diimplementasikan, penggunaannya mungkin tidak diperlukan karena DashMap secara efektif menyediakan fungsionalitas yang diperlukan sambil mempertahankan keamanan bersama.

#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

    Dalam MVC, Service dan Repository dipisahkan dari Model untuk memenuhi prinsip tanggung jawab tunggal (Single Responsibility Principle). Service menangani logika aplikasi dan Repository mengelola akses data. Pemisahan ini akan memudahkan pengembangan serta pemeliharaan program.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

    Jika hanya menggunakan Model tanpa Service dan Repository, kompleksitas dalam setiap model akan meningkat. Setiap model akan bertanggung jawab mengelola akses data, manipulasi, dan logika aplikasi. Hal ini dapat membuat kode menjadi rumit dan sulit dipelihara. Dengan memisahkan tanggung jawab dan menggunakan Service dan Repository, setiap model dapat fokus pada tugas intinya.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

    Saya sudah menggunakan Postman sedikit pada mata kuliah di semester sebelumnya. Dengan Postman, kita dapat mengirim request pada suatu endpoint dan melihat apakah respons yang dikembalikan sudah sesuai, hal ini sangat memudahkan pengecekan program.

#### Reflection Publisher-3

1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

    Tutorial ini menggunakan model Push dati Observer Pattern. Untuk setiap proses create, delete, atau update pada objek, akan dikirimkan notifikasi kepada semua subsciber.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

    Jika menggunakan model Pull dari Observer Pattern, keuntungannya adalah penghematan sumber daya dan kontrol yang lebih besar bagi subscriber, tetapi kekurangannya adalah kompleksitas tambahan pada sisi pelanggan dan risiko keterlambatan informasi.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

    Tanpa multi-threading, notifikasi akan dikirim secara berurutan. Hal ini berpotensi menyebabkan penundaan dan penurunan kinerja program, terutama dengan banyak subscriber atau notifikasi yang membutuhkan waktu.
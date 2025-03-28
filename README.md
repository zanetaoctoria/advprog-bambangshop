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
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
>1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Dalam pola desain Observer, semua subscriber harus mengimplementasikan antarmuka yang sama agar berbagai implementasi dapat merespons pembaruan secara konsisten. Oleh karena itu, jika dalam BambangShop terdapat beberapa jenis subscriber, lebih baik menggunakan interface atau trait agar setiap subscriber memiliki perilaku yang seragam. Saat ini, karena hanya ada satu jenis subscriber, cukup menggunakan satu struct model. Namun, untuk menjaga fleksibilitas dalam pengembangan di masa depan, lebih baik menerapkan interface atau trait sejak awal.


>2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Menurut pendapat saya, penggunaan DashMap lebih sesuai untuk situasi ini. DashMap memudahkan pengelolaan data produk dan Subscriber karena memungkinkan pencarian berdasarkan key yang unik (seperti id pada Program atau url pada Subscriber), sehingga pencarian data menjadi lebih efisien. Sedangkan, jika menggunakan Vec, pencarian harus dilakukan secara sekuensial melalui setiap elemen, yang membuatnya kurang efisien dan lebih rumit.

>3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Walaupun kita bisa menerapkan pola Singleton untuk memastikan hanya ada satu instance data, pada kasus BambangShop yang menerapkan multithreading, DashMap merupakan pilihan yang lebih tepat. DashMap sudah dirancang untuk mendukung keamanan thread secara otomatis, sedangkan jika menggunakan pola Singleton, kita harus menangani masalah sinkronisasi secara manual, sehingga DashMap menawarkan solusi yang lebih efisien.


#### Reflection Publisher-2

>1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Dalam arsitektur MVC, kita memisahkan *Service* dan *Repository* dari *Model* untuk menghindari penumpukan tanggung jawab dalam satu komponen. Jika *Model* menangani representasi data, pengelolaan basis data, dan logika bisnis sekaligus, maka hal ini akan melanggar prinsip *Single Responsibility Principle* (SRP) dari SOLID. Dengan membagi tugasnya, *Model* hanya berfungsi sebagai representasi data, *Repository* menangani operasi CRUD, dan *Service* mengelola logika bisnis. Pendekatan ini membuat kode lebih bersih, mudah dipelihara, lebih fleksibel, dan memungkinkan perubahan pada database tanpa mengganggu logika bisnis.

>2.What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Jika kita mengandalkan *Model* untuk semua fungsi, maka beban tanggung jawabnya akan terlalu besar, mencakup representasi data, pengelolaan basis data, dan logika bisnis. Hal ini membuat kode menjadi sulit dibaca, dipelihara, dan diuji karena berbagai fungsi bisa saling bergantung satu sama lain. Selain itu, hal ini juga mengurangi tingkat *reusability* kode dan menyulitkan pengujian unit. Sebagai contoh, dalam proyek ini, jika hanya menggunakan *Model*, maka *Model Program* harus menangani kueri *Subscriber*, membuat data *Notification*, dan mengirim notifikasi ke API eksternal. Dengan menggabungkan semua tanggung jawab dalam satu komponen, kompleksitas kode akan meningkat secara signifikan.

>3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Saya telah mengeksplorasi Postman dan menemukan beberapa fitur yang sangat membantu dalam pengujian API. Beberapa fitur yang berguna antara lain:
- **Request Testing**: Memungkinkan pengujian API (GET, POST, PUT, DELETE) tanpa harus menulis kode di sisi frontend atau backend terlebih dahulu.
- **Collection**: Menyimpan berbagai request API dalam satu set untuk kemudahan akses.
- **Environment Variables**: Memudahkan perubahan URL atau token API tanpa perlu mengedit setiap request secara manual.
- **Automated Tests**: Menjalankan skrip otomatis untuk memverifikasi apakah API mengembalikan data yang benar atau menampilkan error.

Dengan menggunakan Postman, saya dan tim dapat menghemat waktu dalam pengembangan dan pengujian API, sehingga mempercepat proses pengerjaan proyek.

#### Reflection Publisher-3

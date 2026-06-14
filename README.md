# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
# Reflection Subscriber-1

## 1. In this tutorial, we used RwLock<> to synchronize the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

RwLock is necessary because the notification list is shared across multiple requests and may be accessed concurrently. It allows multiple readers to access the data simultaneously while still ensuring exclusive access when a write operation occurs. This improves performance because reading notifications is expected to happen more frequently than writing notifications. A Mutex would also provide thread safety, but it only allows one thread to access the data at a time, even when multiple threads only need read access. Therefore, RwLock is more efficient for this use case.

## 2. In this tutorial, we used lazy_static external library to define Vec and DashMap as a static variable. Compared to Java where we can mutate the content of a static variable via a static function, why did Rust not allow us to do so?

Rust enforces strict ownership and borrowing rules to guarantee memory safety and prevent data races at compile time. Allowing unrestricted mutation of static variables would make it difficult to guarantee thread safety. Therefore, Rust requires synchronization primitives such as RwLock, Mutex, or thread-safe containers to control access to shared mutable state. This design prevents many concurrency bugs that are common in languages that allow unrestricted mutation of global variables.


#### Reflection Subscriber-2
# Reflection Subscriber-2

## 1. Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

Yes, I explored several parts of the project outside the tutorial instructions, especially `src/lib.rs` and the configuration setup. From `src/lib.rs`, I learned how the application uses `lazy_static` to initialize shared objects such as `APP_CONFIG` and `REQWEST_CLIENT`. I also learned how environment variables are loaded using `dotenvy` and how `Figment` is used to generate application configuration values. In addition, I gained a better understanding of how custom error responses are implemented through the `compose_error_response` function and how the application structures common types such as `Result` and `ErrorResponse`. Exploring these files helped me understand how the different layers of the application work together beyond the tutorial tasks.

## 2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy to add to the system?

The Observer pattern makes it easy to add more subscribers because the publisher only needs to maintain a list of subscribers and notify them when an event occurs. New Receiver instances can simply subscribe to a product type without requiring any changes to the publisher implementation. This promotes loose coupling because the publisher does not need to know the internal details of each subscriber.

However, adding multiple instances of the Main application is more complicated. Each publisher instance maintains its own product data and subscriber list. Without a shared database or synchronization mechanism, subscribers registered to one publisher instance would not automatically be known by another publisher instance. Therefore, scaling subscribers is straightforward with the Observer pattern, but scaling publishers would require additional architecture such as shared storage, service discovery, or distributed messaging.

## 3. Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

I reviewed the provided Postman collection and used it as a reference for understanding the API endpoints and request flows in the application. The collection is useful because it provides organized examples of requests for subscribing, unsubscribing, publishing notifications, and viewing received messages. It helps reduce manual effort when testing APIs and makes it easier to verify whether the application behaves as expected. For larger projects, I believe additional automated tests and more detailed API documentation would further improve maintainability and collaboration among team members.

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

<!-- You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman -->

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
1. The subscriber interface is important when different types of subscribers or models need to get notifications from the publisher in their own ways. It helps keep things flexible and ready for future changes. However, if there's only one type of subscriber, we might skip the interface, though it's still a good practice to use it.

2. Using Vec is fine for storing products or subscribers returned by listAll(). But if we need fast operations, especially using id, DashMap might be better for quick searching. DashMap is great for searching, but Vec is enough for storing data in the repository.

3. Combining the Singleton Pattern with DashMap lets us handle multiple threads accessing and updating the map while making sure there's only one instance of SUBSCRIBERS. DashMap is crucial for handling multiple threads, and Singleton Pattern ensures there's only one SUBSCRIBER instance, no matter how many threads are running.

#### Reflection Publisher-2

1. Separating Model, Service, and Repository follows the Single Responsibility Principle. Models handle data representation and related logic, Services deal with non-data-related logic, and Repositories manage data storage and CRUD operations. This approach makes the application modular and easier to understand, improving abstraction and task focus.

2. Even if combined in one file, Model, Repository, and Service should be viewed as separate entities for clarity. Models define data structures and operations, Repositories manage data storage, and Services handle application logic. This separation may seem complex initially but enhances code readability and maintainability.

3. Postman aids in backend testing by verifying request handling. It ensures expected responses, helping identify errors like unexpected server responses. Particularly useful when frontend and backend development are separate, Postman ensures proper communication between components.
#### Reflection Publisher-3
1. The type of observer used is push. We can tell because the NotificationService actively sends notifications to all subscribers whenever there's an update in a particular product. And each subscriber who hasn't actively requested data gets those notifications.

2. So, the upside of using the pull observer pattern is that each subscriber actively and regularly requests data. That way, they always have fresh data and don't need to worry like, "This data was last updated 16 days ago, is it still accurate? Or did I miss something?" But here's the catch: all this active requesting can sometimes be a downside. It might be inefficient and put unnecessary strain on the app, especially if there are tons of subscribers asking for updates frequently. That can really slow down the app.

3. Now, if they don't use multithreading, handling notifications could take forever. Imagine a notification takes 1 second, and there are 500 subscribers connected to one product. If it's done in a single-threaded way, there's a queue that takes 500 seconds to process, meaning the last user in line waits a whopping 500 seconds for their notification! But with multithreading, each notification process runs at the same time, so every user only needs 1 second to get their notification (well, assuming there's one process per thread, which isn't always the case in real-world scenarios).
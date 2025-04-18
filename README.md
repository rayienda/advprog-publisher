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

1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

In Head First Design Patterns, the Observer pattern represents subscribers through an interface. If every subscriber in the system behaves identically, a trait isn't essential. Defining a dedicated interface only makes sense when you have multiple kinds of observers reacting differently. Since BambangShop's subscribers currently share the same structure and logic, using just a plain struct quite satisfies for now.


2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

While it’s technically possible to track unique entries like id or url using a Vec, it’s not the most effective approach. Every time you want to check for existence or remove an element, you'd need to loop through the entire list. On the other hand, DashMap is built for this — it ensures uniqueness through keys, offers faster lookups, and simplifies data handling overall. Given these advantages, keeping DashMap is the smarter, more efficient option here.


3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Rust’s strict type system enforces safe concurrency, which is crucial when working with shared mutable data like the global SUBSCRIBERS list. A Singleton can give you a shared access point, but it doesn't automatically guard against race conditions. Since multiple threads might access or modify subscriber data, using a thread-safe collection like DashMap remains essential. Even with a Singleton, you’d still need a concurrency-safe mechanism under the hood — so DashMap stays relevant.

#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

In MVC, the Model usually handles both saving data and running logic. But if we split it into smaller parts like Service and Repository, our code gets way easier to manage. The Model just keeps the data, the Service handles how things work, and the Repository talks to the database. This way, everything has its own job, which makes our code cleaner, simpler to test, and easier to improve as the app gets bigger.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

If we make the models do all the work—data, logic, and database stuff—it gets messy real fast. Each model would have too many responsibilities, and they’d rely too much on each other. That means if we change one thing, we might accidentally break something else. It would be hard to keep track of how everything connects. Splitting things into services and repositories makes the structure easier to follow and fix.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Yeah, I’ve used Postman to try out my API endpoints. It makes it easy to see if everything’s working right. It’s helpful for when I’m working on my own or for a group project, especially when working with APIs.

#### Reflection Publisher-3

1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

In this case, the system uses the Push method. The publisher sends all the update info straight to the subscribers without waiting for them to ask. The data is created by the publisher and passed into the subscriber’s update function right away.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

- Pros:
With the Pull approach, subscribers can choose when to check for new info. This is helpful when some parts of the app aren’t always active. They can wait until they’re running again before checking for updates. It also means not everything is updated all at once, which can make the system more efficient and avoid overloading the server.

- Cons:
We’d need a good way to decide when the subscriber checks for updates. If they check too often, it could waste resources and slow things down. But if they check too rarely, they might miss important changes or be out of date for a while. So timing becomes tricky.


3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Without multi-threading, each subscriber would be updated one after another. If one takes too long, it would hold up the rest. That could make the whole process slow, especially with lots of subscribers. Multi-threading lets the app notify everyone at the same time, so things run faster and more smoothly as the system grows.
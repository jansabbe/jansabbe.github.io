---
layout: post
title: Clean architecture
description: The principles of clean architecture have been around for years. Sadly I don’t see it applied too often. This article is my attempt at evangelizing these principles.
image: /public/images/clean_architecture/snippet.jpg
title_image: /public/images/clean_architecture/clean_architecture_title.jpg
---

The principles of clean architecture have been around for years. The same goes for hexagonal and onion architectures. Sadly I don’t see it applied too often. Too bad as these principles are simple yet powerful. They lead to better design, clear separation of concerns and improved testability. This article is my attempt at evangelizing these principles.

![Layers](/public/images/clean_architecture/layers.jpg)

In the picture you find an overview of the different layers in this architecture. The important thing to note here is that dependencies flow down. The infrastructure layer depends on the application layer, which depends on the domain layer. The domain layer doesn’t depend on any other layer.

If you ever read the Domain Driven Design book, these might sound familiar.  However the order and content is slightly different. Let’s go over these layers in more detail.

### Presentation layer
The presentation layer is responsible for all things UI. This layer contains all the necessary HTML, JavaScript and CSS files.

### Infrastructure layer
The infrastructure layer is responsible for the configuration of all the technical frameworks you are using in your project. This will contain the REST controllers, the integration with SQL databases and the messaging handlers. The infrastructure layer is the only layer that should have dependencies on frameworks like Spring, Kafka client, AWS SDKs, etc.

It can provide services to the application and domain layer. The domain layer or the application layer can define an interface, which gets implemented in the infrastructure layer. This is called the dependency inversion principle.

![Dependency inversion](/public/images/clean_architecture/dependency_inversion.jpg)

### Application layer
The application layer consists of two things.
* The commands and responses the *presentation* layer (or other applications) can use. These are _transfer objects_ as they can be serialized and be sent over the network. They form the public api of your application.
* Command handlers. What needs to happen if your application receives a certain command?

Put differently, the application layer models the different *use cases* in your system. It should answer the question: '*What can I do with this application/service/domain?*' This is not some kind of technical layer, in fact it shouldn't have any dependency on any kind of technical framework. It describes the functionality and the flow of your system.

Avoid modelling all your use cases as methods on a single service. It will get unwieldly and it’s less clear to a future maintainer what the system is doing. Instead create a separate class for each command you handle, which represents the use case. Like a good supervisor these command handlers will mostly delegate the actual work to the domain layer.

### Domain layer
The domain layer models all your business rules. This is the place where your aggregates, value objects, domain events and entities will live. Like the application layer, this should only contain plain old java objects, without any kind of dependency on other technical frameworks.

Let’s go over an example:

![Example](/public/images/clean_architecture/layers_example.jpg)

The presentation layer will send a request which ends up on a REST controller in the infrastructure layer. This controller will deserialize the request to a command, and pass it to the correct command handler in the application layer.

The `PlaceChickenInCoopHandler` command handler will look up the `ChickenCoop` aggregate via the `ChickenCoopRepository`. The `ChickenCoopRepository` is just an interface that gets implemented in the infrastructure layer by the `SQLChickenCoopRepository`.

Once the command handler looked up the `ChickenCoop` aggregate, it can call the `placeChickenInCoop` method. The aggregate is responsible for enforcing any business invariants (eg allow maximum 1 rooster in a coop). Finally the command handler will call the `save` method on the repository.

# ![Separation of Concerns](/public/images/clean_architecture/separation_of_concerns_title.jpg)

An important aspect of clean/hexagonal and onion architectures is keeping frameworks out of your application or domain layer. You want to avoid mixing technical concerns with business concerns, as these typically evolve at a different pace. Separating these concerns makes it easier to upgrade libraries or change technical decisions without impacting your domain too much. Vice versa, you can start modelling your business domain, even if you don’t know which database or hosting platform you will use.

![Stop frameworkitis](/public/images/clean_architecture/stop_frameworks.jpg)

It can be hard to avoid any kind of dependency in your application or domain layer. A lot of libraries and frameworks expect all sorts of annotations or interfaces on your domain objects. Try to avoid these annotations, as these tie your domain to a certain technical decision. Check if there is an alternative. Check if there is a less invasive library.

If all else fails, try to limit the dependency to just some annotations or interfaces. Just realise it is a form of technical debt. If you update or change the library, you'll need to pay.

The same goes for in-house company frameworks! They are not exempt. Try to limit their scope to the infrastructure layer, while keeping application and domain layer as pure as possible. Don’t go too far with the DRY principle, it leads to high coupling.

# ![Testing](/public/images/clean_architecture/testing_title.jpg)

Because of the separation of concerns, writing unit tests becomes very easy. You can also nicely align the different kinds of tests with the different layers in your application.

![Testing pyramid](/public/images/clean_architecture/testing_pyramid.jpg)

The domain layer will contain all your unit tests. Any interface that is implemented in the infrastructure layer can easily be mocked. In fact, I would *only* mock those interfaces, and use the real implementation of any other class in the domain layer. It’s a very clear guideline for the whole team to follow, which strikes a good balance between *mockitis* and *integration-test-itis*.

The application layer is the ideal place for your acceptance tests. Mock out the infrastructure layer but use the real classes in the domain layer. If you mock those out, you get little benefit of your acceptance tests when you refactor later on. Consider using a BDD framework here to iterate over use cases with your product expert.

Your integration tests can live in the infrastructure layer. These include full end-to-end REST tests, as well as specific integration-tests for your SQL database and messaging platforms. Don’t retest all your business cases again. Focus on testing the integration with external systems and the framework configuration.

Finally the presentation layer will contain all your Jest/Jasmine unit tests for your javascript files. You could also include your selenium tests in this project or keep those in a separate project.

# Conclusion

There are many variants of clean, hexagonal and the onion architecture. Yet they all stress the importance of separation of concerns. They avoid infrastructural dependencies in the domain layer. This not only improves testability. It also allows you to postpone technical decisions till a time where you know more. While powerful, this architectural style is pretty simple. In a next post I hope to show you with a concrete example.
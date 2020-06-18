# LogicBlocks: What Is It?

## Overview

The LogicBlocks platform makes it quick and easy to build and launch new 
products and businesses. LogicBlocks is a production ready microservice-oriented 
platform based on solid architectural principles. LogicBlocks aims to be 
modular, flexible, scalable and evolvable making it a good fit for many domains 
and problem spaces.

## Use Cases

The design of the LogicBlocks platform allows consumers to use as much or as 
little as needed to solve their specific problem. As a result, LogicBlocks is
a good fit in a large variety of cases.

### Full Business Models

LogicBlocks services are designed to work together to solve problems central to
many types of product. Some of those problems are very core to all products, 
such as customer registration and profile management, payments, notifications
and customer support. Others are more specific to particular business models.

Through composing together different services, LogicBlocks aims to provide 
out-of-the-box support for various different business models, such as 
e-commerce, subscriptions, marketplaces, social networks and content or 
experience based businesses. 

### Individual Services

Each LogicBlocks service is independently valuable and solves one specific 
problem. Additionally, each service's repository contains everything needed
to build, test, deploy and operate that service. If a service solves a problem 
you have, you are free to use only that service and ignore the rest. Services 
are individually licensed and can be forked and modified as required.

All services are Docker based so are easily runnable within existing
containerisation platforms. Alternatively, services can be run via the
standalone executable created during the build process. Services follow the
principles of [The Twelve Factor App](https://12factor.net/), making them easy
to deploy and operate.

### Reusable Components

All the components that make up a LogicBlocks service are available as
individual open source libraries and can be used as required in other projects.

### Examples

The services that make up LogicBlocks employ a number of tried and tested
approaches for building flexible and scalable microservice architectures. They
can serve as examples of how to build and deploy large scale microservice 
platforms. 

## Principles

The LogicBlocks platform follows a set of key design principles in all of its
components. 

### Modular & Independent

LogicBlocks services are made up of small, independently tested, packaged and 
released libraries that themselves can be used in different situations or can be
switched out if no longer fit for purpose.

Additionally, all services are designed in such a way that they can be used 
independent of the platform as a whole. Even when services have dependencies,
those dependencies are expressed through well-defined interfaces such that the
specific providers can be swapped out.

### Flexible & Extensible

As far as possible, the libraries that make up LogicBlocks services are 
technology agnostic, allowing specific databases, messaging technology
or deployment platforms to be swapped out as needed. LogicBlocks provides
adapters for the specific technologies it supports but allows others to be
written by consumers when needed.

Additionally, LogicBlocks aims to be extensible to previously unthought-of use
cases.

### Scalable

Where possible, LogicBlocks services are stateless allowing for horizontal
scalability and more simple deployment models. Services use immutable 
persistence models allowing for simpler data replication and co-location of 
required data. Services also utilise caching to minimise load and request 
latencies.

### Production Ready

All LogicBlocks components include automated testing, deployment and operational
tools making them easy to adopt and maintain. LogicBlocks services also expose
operational endpoints for monitoring and allow for observability through 
structured logs and event streams.

LogicBlocks services aim to be secure by default, employing modern standards for
authentication, authorisation and encryption (both in transit and at rest).

## Concepts

### Service Oriented

The LogicBlocks platform is service-oriented and uses a number of architectural
approaches to facilitate this.

#### Domain Driven Design

Overall, the LogicBlocks platform is built using concepts from domain driven 
design. Services are modelled around 
[aggregates](https://martinfowler.com/bliki/DDD_Aggregate.html), expressed in 
the ubiquitous language of the domain. They employ the 
[hexagonal architecture](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software))
to clearly separate responsibilities and use a number of domain modelling 
patterns discussed 
[in](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)
[the](https://www.amazon.co.uk/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577) 
[literature](https://www.amazon.co.uk/Domain-Driven-Design-Distilled-Vaughn-Vernon/dp/0134434420).    

#### REST

All LogicBlocks services utilise true 
[REST](https://en.wikipedia.org/wiki/Representational_state_transfer) including
[HATEOAS](https://en.wikipedia.org/wiki/HATEOAS), such that the full benefits of
the architectural style can be realised. Services use 
[HAL](http://stateless.co/hal_specification.html), a type of JSON, as their 
wire format, which incorporates full hypermedia controls and has support for
[URI templates](https://tools.ietf.org/html/rfc6570).

Using hypermedia allows for more advanced interactions between services, 
building a navigable web of resources that supports location agnosticism, 
polymorphism and over the wire state machines.

#### Resource Modelling

Beyond using HAL, resources in LogicBlocks services employ a number of patterns.

All resources include 
[pseudo-URIs](https://philcalcado.com/2017/03/22/pattern_using_seudo-uris_with_microservices.html),
similar to AWS's ARNs, allowing for flexible equality and introspection of key 
properties of identity. Resources are extensible through metadata, using the 
concepts of 
[labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) 
and 
[annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/) 
borrowed from Kubernetes, allowing additional attributes to be stored against 
resources than those that are part of the core model. 

Additionally, all collection resources allow for complex queries, sorting and 
pagination.

### Event Driven

The LogicBlocks platform is event-driven at its core, employing a number of
patterns from event-based systems.

#### Event Sourcing

All LogicBlocks services treat events as first-class and employ 
[event sourcing](https://martinfowler.com/eaaDev/EventSourcing.html) for 
capturing state changes. In this architectural pattern, services persist all
requests as point-in-time events in an append-only immutable log, storing any 
additional context in the payload of those events. The aggregate is derived by 
[projecting](https://domaincentric.net/blog/event-sourcing-projections) over 
all the events in that aggregate's event stream occurring prior to a given time.
In this way, the state of the aggregate throughout history can be reproduced. 
This approach allows different projections over the same event stream and new 
projections to be introduced at any time, accounting for all historical events.    

#### Event Notification and Event Carried State Transfer

LogicBlocks services expose REST-based event feeds allowing them to be 
integrated with messaging systems (such as kafka) to support 
[event notification](https://martinfowler.com/articles/201701-event-driven.html)
and 
[event carried state transfer](https://martinfowler.com/articles/201701-event-driven.html)
between services. Events exposed by the event feeds are HAL resources exposing
hypermedia controls as in the REST API.

This allows the LogicBlocks platform to be highly reactive in near real-time
providing huge flexibility for future use cases in a decoupled fashion. For 
example, the platform leverages this event notification capability to support 
its various notification mechanisms and makes it relatively simple to add more.

### DSL-Based Configuration

All LogicBlocks components externalise their configuration and are highly 
configurable. By default, configuration is stored in YAML files supporting
hierarchies of configuration that can be used in different contexts, such
as build, testing, deployment and operation.

Services themselves are configured via environment variables allowing other
configuration mechanisms to be plugged in as needed. In cases where the 
configuration of a service requires more customisation than allowed by static 
environment based configuration, LogicBlocks services expose DSLs. These DSLs 
allow significant customisation of services without needing to fork the 
codebase, via custom designed configuration interfaces.

## Technologies

### Built on the Best

LogicBlocks makes heavy use of open source technologies and is an open source
platform by default. Where integration is offered to a proprietary technology,
it is only offered as an optional alternative to an open source equivalent.

All technologies employed are tried and tested and have been used in production 
in various industries, domains and environments.

Where possible, LogicBlocks adheres to standards to allow for future 
interoperability. 

### Clojure at the Core

LogicBlocks services are build using [Clojure](https://clojure.org/), leveraging
much of its latest and greatest ecosystem. Clojure provides a good balance 
between getting up and running fast whilst allowing very advanced problems to be
solved elegantly. With a growing community and many years of investment in its 
core, the Clojure landscape is becoming mature and provides significant power to
developers.

### Strong Foundations

LogicBlocks uses [PostgreSQL](https://www.postgresql.org/) as its default
persistence store, leveraging PostgreSQL's support for JSON column types. For
messaging, LogicBlocks uses [Apache Kafka](https://kafka.apache.org/) by 
default, making use of its long term message retention.  

### Hand-in-hand With InfraBlocks

Whilst entirely optional, LogicBlocks leverages the 
[InfraBlocks](https://github.com/infrablocks) infrastructure platform by default
making it quick and easy to stand up, operate and maintain. All LogicBlocks
services are 
[cloud native](https://en.wikipedia.org/wiki/Cloud_native_computing) and have
been deployed in production a number of times using 
[AWS](https://aws.amazon.com/).

## Organisation Map

### Categories

The LogicBlocks organisation consists of a number of different types of modules:

* _Libraries_: As part of building out the LogicBlocks platform a number of 
  libraries have been created for functionality that wasn't well covered by 
  other libraries in the Clojure ecosystem.
* _Components_: LogicBlocks services utilise 
  [Stuart Sierra's component library](https://github.com/stuartsierra/component)
  for dependency injection and system lifecycle management. LogicBlocks includes 
  a number of configurable components that can be dropped in to other component
  based systems as needed.
* _Resources_: LogicBlocks services use 
  [liberator](http://clojure-liberator.github.io/liberator/) to expose their
  REST APIs. A number of flexible liberator resources have been created for 
  commonly exposed resources across services.
* _Services_: The core units of the LogicBlocks platform are microservices 
  providing a particular capability.
* _Examples_: A few example repositories exist giving end-to-end examples of 
  different aspects of the platform 

### Repositories

#### Libraries

* [`antsy`](https://github.com/logicblocks/antsy): a simple ANSI escape code 
  library.
* [`configurati`](https://github.com/logicblocks/configurati): a library to 
  manage application configuration, supporting various different configuration
  sources, conversions and reusable specifications.
* [`derivative`](https://github.com/logicblocks/derivative): a general purpose
  in place scaffolding and source code rewrite tool. (work in progress).
* [`exegesis`](https://github.com/logicblocks/exegesis): a library for 
  interrogating Java types for annotation information.
* [`hype`](https://github.com/logicblocks/hype): a collection of hypermedia
  utilities for [`bidi`](https://github.com/juxt/bidi) and 
  [`ring`](https://github.com/ring-clojure/ring).
* [`jason`](https://github.com/logicblocks/jason): a configurable JSON encoding 
  and decoding library backed by 
  [`jsonista`](https://github.com/metosin/jsonista) and 
  [`jackson-databind`](https://github.com/FasterXML/jackson-databind) with 
  support for various key conversions.
* [`liberator-mixin`](https://github.com/logicblocks/liberator-mixin): an 
  extension to [`liberator`](http://clojure-liberator.github.io/liberator/)
  allowing for composable mixins, including a number of commonly used mixins.
* [`pathological`](https://github.com/logicblocks/pathological): a thin but 
  comprehensive wrapper of Java NIO2.
* [`spec-validate`](https://github.com/logicblocks/spec-validate): A 
  clojure.spec based validation library, including a number of useful 
  predicates and descriptive error reporting.
* [`vent`](https://github.com/logicblocks/spec-validate): declarative and 
  extensible rule based event processing.
* [`zebra`](https://github.com/logicblocks/zebra): a wrapper for the Stripe
  SDK, with support for the more modern recommended integration options, such 
  as payment intents.
  
#### Components

* [`component.flyway-migrator`](https://github.com/logicblocks/component.flyway-migrator):
  a component to execute flyway migrations on start.
* [`component.jdbc-data-source.postgres`](https://github.com/logicblocks/component.jdbc-data-source.postgres):
  a component providing a pooled PostgreSQL JDBC data source using the
  [`hikari`](https://github.com/brettwooldridge/HikariCP) connection pool.

#### Resources

* [`liberator-hal.discovery-resource`](https://github.com/logicblocks/liberator-hal.discovery-resource):
  a HAL discovery resource for 
  [`liberator`](http://clojure-liberator.github.io/liberator/).
* [`liberator-hal.ping-resource`](https://github.com/logicblocks/liberator-hal.ping-resource):
  a HAL ping resource for
  [`liberator`](http://clojure-liberator.github.io/liberator/).
  
#### Services

TODO: Add this.

#### Examples

* [`example-service`](https://github.com/logicblocks/example-service): an
  example service demonstrating the capabilities of the LogicBlocks platform.
  
## Anatomy of a Service

TODO: Add this.

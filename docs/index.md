# A demonstration for an event-driven solution of autonomous car ride

???+ info "Updated"
    Created 07/06/2023 - Updated 12/05/23

This article illustrates how to apply domain-driven design, and event storming to implement an event-driven solution using different EDA patterns like Event Sourcing, CQRS, and SAGA. The solution is built incrementally adding more complexity over time, so the reader can understand how to apply DDD and all those patterns. There is not a unique solution to implement the requirements, and this repository document different view.

It will be a long journey to implement the different patterns and flavor of the solution. 

As a target implementation, we will use Java, and Python most of the time. The deployment is targeted to use local docker images, and AWS services for cloud deployment. Infrastructure as code is used to deploy dependent services and applications. Some components of the solution will also be AWS Lambda function. The demonstration includes generating CarRides data, that can be used downstream in a Data lake with Analytics.

This repository is linked to [the EDA book](https://jbcodeforce.github.io/eda-studies/) which documents the event-driven design pattern and architecture discussions.

## Application Context

The customer wants to go from one address or geographic location to another one, within a big city, using the Acme Autonomous Car Ride mobile app. The cars are autonomous cars (robot car) with no pilot. 

The application context looks like in the following diagram:

![](./diagrams/app-context.drawio.png){ width=800 }

Travelers use mobile application to book a ride between two locations within the same city, the Car Ride Solution dispatches an autonomous vehicle, uses traffic report to compute ETA and pricing. The application is also monitoring existing rides via car telemetries. The Marketing analysis is an example of external system interested by the generated data. 

## Requirements

* User will use a simple user interface to demonstrate booking a car ride and then accept the payment.
* The first components will be the CarRide management and the car fleet / inventory management.
* The CarRide management will demonstrate Command Query Responsibility Segregation combined with Event Sourcing.
* Demonstrate and document the technology choices and fit for purpose.

See [event storming and DDD section for business requirements analysis](./ddd.md).

### Event Sourcing

[Event sourcing](https://jbcodeforce.github.io/eda-studies/patterns/event-sourcing/) is a design pattern adopted in Event-driven solution. It is not mandatory but it delivers some interesting advantages for such implementation, in particular to explain what happened to the business entity. The CarRide is using the event sourcing, so a dedicated event stores to keep the change to the CarRide Entity.

![](./diagrams/car-ride-es.drawio.png)


### Command Query Responsibility Segregation

CQRS is used in a lot of distributed solution, to be able to scale the read model. DynamoDB by design is supporting CQRS with the read replicas feature. For more details see the [CQRS pattern explanation](https://jbcodeforce.github.io/eda-studies/patterns/cqrs/index.md).

### Saga pattern

The classical implementation of Saga is to use an orchestrator to manage the state of the Saga and being able to rollback the transaction with compensation API. For more details see the [Saga pattern explanation](https://jbcodeforce.github.io/eda-studies/patterns/saga/index.md).

An alternate is to use Choreography. 

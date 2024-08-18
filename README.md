# Project Overview

This is the microservice responsible for handling POSTS from users of a `social network`.

To view the resources documentation for this API, [click here](./docs/RESOURCES.md).

The following technologies were used for development:
* Framework: NestJS (Node.js)
* Language: TypeScript
* Validation: class-validator + class-transformer
* Unit Testing: Jest
* Integration Testing: Jest + SuperTest
* Monitoring: Prometheus & Grafana
* Tracing: Sentry

This microservice also connects with other services:
* Cache service: Redis
* Queue service: RabbitMQ
* Realtime messaging: Socket.io
* Relational Database: PostgreSQL (with TypeORM)
* NoSQL Database: MongoDB (with Mongoose)

## Containerization

### Dockerfile

* Image base: node:20.16-alpine3.19
This image was chosen because it is the most current LTS (long-term support) version, and uses Alpine Linux, a lightweight and high-performance version.

The steps will be:

* Define the WORKDIR
* Copy package*.json to the container
* Install the dependencies,
* Copy the remaining files
* Run the build (when it is a production Dockerfile)
* Expose a port
* Start the server [start:dev, start:prod... depending on the environment]


### Docker Compose

[Click here](./docs/DOCKER_COMPOSE.md) to view the configuration file for multi-container.


### Kubernetes

The application has a queuing and cache system to improve performance. \
Stress tests would be carried out using k6, to identify the range of replicas needed to ensure read/write operations are within 500ms, with a degradation tolerance of no more than 10% over time, even as data scales to millions or billions of records.

Resilience tests can be done with Chaos Mesh.

The application will be continuously monitored with Prometheus & Grafana, and the replica range can be modified.

[Click here](./docs/KUBERNETES.md) to see the steps to deploy the service on Kubernetes and view the manifests.

## Testing Strategy

### Unit Tests

Use Jest to create unit tests.
The goal is to have 100% coverage of the application with unit tests, therefore, delivery must always be made covering all possible scenarios. \
Perform data input and output validations to ensure that the logic for manipulating the data is working as expected.

### Integration Tests

Use Jest with SuperTest to create integration tests. \
They should be created for all critical resources or when there is a complex flow for processing. \
For example, when a resource needs to execute many functions until it returns, or there is a need to test integration with a database or external services.


# Setup Instructions

## Installation

```bash
$ npm install
```

## Running the app with Docker

- `docker-compose build` to build image
- `docker-compose up -d` to create and start containers

## Running the app without Docker

```bash
# development
$ npm run start

# watch mode
$ npm run start:dev

# production mode
$ npm run start:prod
```


# Build, Test, and Deploy Instructions

Build check and tests will be done whenever a push is made to a branch.

To deploy to staging, merge your branch with the staging branch.
This will trigger a pipeline, which will deploy to staging.

To deploy to production, merge your branch with the main branch.
This will trigger a pipeline, which will deploy to production.

### Build in local environment

```bash
# unit tests
$ npm run test

# e2e tests
$ npm run test:e2e

# test coverage
$ npm run test:cov
```

### Test in local environment

```bash
# unit tests
$ npm run test

# e2e tests
$ npm run test:e2e

# test coverage
$ npm run test:cov
```


# CI/CD Pipeline Explanation

Below is a list of yaml files that will be created.

These will be triggered when a push is made to any branch, except `main` and `staging`:

* build-check.yml: ensuring that the project code can be built.
* unit-tests.yml: and will run the application's unit tests, ensuring that each unit of code behaves as expected for different inputs and conditions.
* lint-check.yml: with the aim of identifying problems related to formatting, coding standards, and potential errors before execution.

These are for deploy:

* staging-deploy.yml: will be triggered when a push is made to the `staging` branch.
* production-deploy.yml: will be triggered when a push is made to the `main` branch.

### CI/CD: Task Delivery Flow

For better visualization, [click here](https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=TechTest.drawio&page-id=v5X4ZSBKwLfaPkUQKjhO#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1ptHPn7DArZM16_XKKtGamrwrpVmez-0n%26export%3Ddownload) (acesso externo)

![If the diagram does not load, go to https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=TechTest.drawio&page-id=v5X4ZSBKwLfaPkUQKjhO#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1ptHPn7DArZM16_XKKtGamrwrpVmez-0n%26export%3Ddownload](./docs/diagrams/CI_CD.svg)


# Architecture Overview

### Diagrams

* Infrastructure diagram

For better visualization, [click here](https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&target=blank&highlight=0000ff&edit=_blank&layers=1&nav=1&title=TechTest.drawio#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1ptHPn7DArZM16_XKKtGamrwrpVmez-0n%26export%3Ddownload) (acesso externo)

* Diagram of how this microservice works.

For better visualization, [click here](https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=TechTest.drawio&page-id=s0kt4Jnu0GX9cwu7lj_r#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1ptHPn7DArZM16_XKKtGamrwrpVmez-0n%26export%3Ddownload)

![If the diagram does not load, go to https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=TechTest.drawio&page-id=s0kt4Jnu0GX9cwu7lj_r#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1ptHPn7DArZM16_XKKtGamrwrpVmez-0n%26export%3Ddownload](./docs/diagrams/UsageFlow.svg)

* REST API Behavior Diagram

For better visualization, [click here](https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=TechTest.drawio&page-id=qd33mnqiCQqoM4UgHgPT#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1ptHPn7DArZM16_XKKtGamrwrpVmez-0n%26export%3Ddownload)

![If the diagram does not load, go to https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=TechTest.drawio&page-id=qd33mnqiCQqoM4UgHgPT#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1ptHPn7DArZM16_XKKtGamrwrpVmez-0n%26export%3Ddownload](./docs/diagrams/Api.svg)

### Justification of choices

* Node.js: was chosen because it is performant, secure, and easy to maintain.
* NestJS: was chosen because it is a robust and scalable framework, built on top of Node.js, that makes it easy to create server-side applications with a modular and clean architecture. Its native integration with TypeScript, support for the dependency injection pattern, and a large community make NestJS an excellent choice for building complex APIs and microservices.
* class-validator + class-transformer: were chosen mainly because of their integration with NestJS. They facilitate the validation and transformation of data at runtime, providing an efficient way to ensure that the received data meets the expected requirements before being processed. The native integration with NestJS allows for simple and robust validation configuration in routes.
* Jest + SuperTest: were chosen because of their direct integration with NestJS, which makes it easy to create unit and integration tests. Jest is a natural choice for TypeScript unit testing, while SuperTest allows you to create end-to-end tests that simulate HTTP requests, ensuring that APIs are working properly.
* Prometheus & Grafana: were chosen to monitor application performance, identify bottlenecks, and track service health
* Sentry: was chosen for its ease of implementation and its ability to capture detailed stack traces, facilitating the debugging process and ensuring that critical issues are addressed before they impact end users.
* Redis: was chosen to handle caching because of its ability to store temporary data with fast access, which is ideal for improving the performance of operations that require frequent data queries.
* RabbitMQ: was chosen as the queuing system because it supports multiple forms of routing and message management, ensuring that data is delivered securely and in the correct order, even in distributed systems.
* Socket.io: was chosen to enable real-time, bidirectional communication between clients and servers. It abstracts the complexity of WebSocket and ensures compatibility with all browsers, in addition to providing fallback mechanisms in case of WebSocket connection failures.
* PostgreSQL (with TypeORM): was chosen as the main database to store user posts due to its robustness, support for transactions, and compliance with the SQL standard. It is a highly scalable and reliable relational database, ideal for storing structured data and performing complex queries. TypeORM was selected to facilitate object-relational mapping (ORM), allowing to work with the database in a more intuitive and typed way in NestJS.
* MongoDB (with Mongoose): was chosen to store audits due to its flexibility with unstructured data and horizontal scalability.

https://app.diagrams.net/#G1ptHPn7DArZM16_XKKtGamrwrpVmez-0n#%7B%22pageId%22%3A%22s0kt4Jnu0GX9cwu7lj_r%22%7D


# TODO list

[Click here](./docs/TODO.md) to see estimated development time.


### Stay in touch

- Author - [Douglas Felchilcher](https://www.linkedin.com/in/douglasfelc/)

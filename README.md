# Distributed System

A Java microservices learning project covering customer registration, fraud checks, service discovery and notifications.

<p>
  <a href="https://buymeacoffee.com/lucky_1uck">
    <img src="https://img.shields.io/badge/Support_the_project-FFDD00?style=for-the-badge&amp;logo=buymeacoffee&amp;logoColor=black" alt="Support the project" width="260" height="40">
  </a>
</p>

## Architecture

The customer service stores a registration, then calls the fraud service through
an OpenFeign client. Notification delivery has a REST interface and a RabbitMQ
consumer. Eureka provides service discovery, and the infrastructure configuration
includes Zipkin tracing.

| Module | Role | Configured HTTP port |
| --- | --- | --- |
| [customer](customer) | Customer registration | `8080` |
| [fraud](fraud) | Fraud-check service | `8081` |
| [notification](notification) | Notification persistence and consumption | `8082` |
| [eureka-server](eureka-server) | Service registry | `8761` |
| [clients](clients) | Shared Feign clients and request/response types | — |
| [amqp](amqp) | RabbitMQ configuration and message producer | — |

## Stack

Java 17 · Spring Boot 2.7 · Spring Cloud · OpenFeign · PostgreSQL · RabbitMQ · Maven

## Local setup

1. Install JDK 17, Maven and Docker Compose if using the supplied infrastructure.
2. Review [docker-compose.yml](docker-compose.yml): it defines PostgreSQL, pgAdmin, RabbitMQ and Zipkin, not the Java applications.
3. Remove the empty list item under `zipkin.networks` before using Compose; validate with `docker compose config`.
4. Create the `customer`, `fraud` and `notification` PostgreSQL databases and align each module's datasource settings.
5. Build the Maven reactor:

   ```bash
   mvn clean install
   ```

6. Start `EurekaServerApplication`, `FraudApplication`, `NotificationApplication` and `CustomerApplication` from your IDE, with the required infrastructure available.

The service configurations use Hibernate `create-drop`. Use disposable local
databases: their mapped schemas are created and dropped with the applications.

## API entry points

- `POST /api/v1/customers`: register a customer.
- `GET /api/v1/fraud-check/{customerId}`: request a fraud decision.
- `POST /api/v1/notification`: submit a notification.

The customer-registration code currently calls fraud checking but does not
publish a notification. The separate notification path should not be mistaken
for a completed registration-to-message flow.

## ☕ Support the project

If this project helps you learn or build, you can support its development.

<p>
  <a href="https://buymeacoffee.com/lucky_1uck">
    <img src="https://img.shields.io/badge/Support_the_project-FFDD00?style=for-the-badge&amp;logo=buymeacoffee&amp;logoColor=black" alt="Support the project" width="260" height="40">
  </a>
</p>

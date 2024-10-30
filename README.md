# Telosys Template for Spring Boot JPA REST Controller with Cloud Spanner DB, Kafka Integration, and Entity Change Listener

This Telosys template generates a Spring Boot application with the following features:

- **JPA REST Controller**: Auto-generated REST endpoints for managing entities via JPA.
- **Google Cloud Spanner DB Connection**: Integrates with Cloud Spanner as the primary data source.
- **Kafka Listener and Producer**: Automatically listens for Kafka messages and produces Kafka messages on entity changes.
- **Entity Change Listener**: Listens for JPA entity lifecycle changes and publishes corresponding events to Kafka.

## Prerequisites

Before generating the application, ensure you have the following installed:

- Java 17
- Maven (build system)
- Telosys CLI or Telosys Eclipse plugin (to use the template)
- Service Account Key JSON file for gcp account to be connected to gogle spanner database
- Kafka Confluent Cluster

## Features

1. **Spring Boot data REST Controller**
   - Automatically generated REST endpoints (GET, POST, PUT, DELETE) for JPA entities.
   - Entities are mapped to database tables using JPA.
2. **Cloud Spanner DB Connection**

   - Google Cloud Spanner is set as the primary database.
   - Configuration for Spanner is handled in `application.yml` and `kafka.yml`.
   - JPA is used for persistence, with Hibernate as the ORM and `SpannerDialect` for Spanner compatibility.

3. **Kafka Consumer and Producer**

   - A Kafka consumer that listens to all topics for each entity's change event
   - Kafka producer to publish messages on specific topics when certain events happen, such as entity changes. (Outbox Pattern is implemented with entity change event queue)

4. **Entity Change Listener**

   - A listener that listens to JPA entity lifecycle events (e.g., `@PrePersist`, `@PostUpdate`, `@PreRemove`) and publishes Kafka messages when an entity is created, updated, or deleted.

5. **Kafka Topic JSON schema**

   - JSON schema for kafka messages per topic

6. **API Endpoint configuration yaml file**
   - Rest response schema configuration yaml file per entities

## How to Generate the Project

### Step 1: Install Telosys

Ensure that Telosys is installed either via CLI or the Eclipse plugin.

- **Telosys CLI Installation:**

  - **macOS**

    ```bash
    brew install telosys # for macOS users
    ```

  - **Windows**
    Follow [installation guide for windows](https://doc.telosys.org/telosys-cli/cli-installation-windows)
  - **Linux**
    Follow [installation guide for linux](https://doc.telosys.org/telosys-cli/cli-installation-linux)

- **Telosys Eclipse Plugin:**
  Follow the [Telosys Eclipse plugin documentation](https://www.telosys.org/eclipse.html) to install the plugin.

### Step 2: Define the Model

Create a Telosys model for your entities. The model defines the database tables and relationships between entities. Each entity will correspond to a JPA entity class in the Spring Boot application.

- Define entities in the Telosys model from scratch. [Official Documentation](https://doc.telosys.org/telosys-cli/cli-getting-started/model-creation/new-model-from-scratch)
- Or you can create model from database schema
  - 1. Adjust parameters from `TelosysTools/databases.yaml` (there are two databsae instances defined, you can use any one) with proper jdbc url, username, password
  
  - 2. Check database connection

    ```sh
    telosys > cdb mysql # use your database id from database configuration
    ```

  - 3. Generate telosys model from database

    ```sh
    telosys > nm MyModel mysql # use model id and your database id from database configuration
    ```

- (Optional) Adjust data type for each entites if needed

  Include relationships such as `OneToMany`, `ManyToOne`, etc., if needed.

### Step 3: Generate the Code

1. Adjust parameters on `TelosysTools/telosys-tools.cfg`
2. In your Telosys project, use the template to generate the application code.
3. Run the following command (Telosys CLI):

   ```bash
   tt # this opens telosys cli
   ```

   ```bash
   telosys > h . # sets home path for telosys cli
   ```

   ```bash
   telosys > genb * * # this generates project structure based on models and entities
   ```

4. The template will generate:
   - JPA entities based on the model.
   - REST repositories for each entity.
   - Kafka listeners and producers.
   - Kafka Topic JSON schema
   - REST configuration yaml files

### Step 4: Update the Configuration

#### Environment Configuration (`.env`)

- Copy your service account key JSON file as `gcp-account.json`
- Create and Adjust dotEnv file

  ```sh
  cp .env.example .env
  ```

  Replace variants
  - `PROJECT_NAME`: GCP project name (e.g.: amerchgcp-amerch-dev-dt)
  - `DB_INSTANCE_NAME`: Cloud Spanner Instance Name (e.g.: chainstoretest)
  - `DB_NAME`: Cloud Spanner DB Name (e.g.: chainstoredb)
  - `KAFKA_SERVER`: Confluent Kafka Cluster URL
  - `KAFKA_USERNAME`: Username provided by confluent kafka
  - `KAFKA_PASSWORD`: Key provided by confluent kafka

_That's all !!!_

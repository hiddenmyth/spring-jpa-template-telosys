# Java basic REST CRUD backend with Spring Boot and Spring Data JPA

This bundle generates a full backend application for CRUD operations  

The generated application is as simple as possible :  

- single module project 
- REST controllers based on Spring Boot 
- JSON payload based on basic DTO 
- thin layer of services 
- mapping DTO - JPA entities
- persistence based on JPA entities with Spring Data JPA repositories 

## How to use this template
 - Define entities under Store model
 - Generate files
 ```sh
 genb * *
 ```
 - Copy `.env` file
 ```sh
 cp .env.example .env
 ```
 - Replace variable values in `.env`
 - Generate service account for spanner connection and paste as `/gcp-account.json`
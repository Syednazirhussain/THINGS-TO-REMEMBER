### MONOLITHIC ARHITECTURE
    - App code is developed and deployed as a single unit.
    - Whole app code is based on single language like NodeJS.

### Advantages
    - Easy to manage and deploy
### Dis-Advantage
    - Whole app need to re-deploy even if a single change occure.

### MICROSERVICE ARHITECTURE
    - Destructure the application module as a single unit and combine them through an API gateway with the UI.
    - Each unit have a seprate code based and developed and deployed individually.

### HOW MICROSERVICES COMMUNICATE EACH OTHER
    - Synchronous Communication: Through API calls
    - Asynchronous Communication: It will use message broker. Message brokers works as a queue pipeline.
        - Message Brokers
            1.  Apache Kafka
            1.  RabbitMQ


# EDA - Event Driven Architecture

## Events: 
- An event is something that happened in the past (ex: "ride completed")
- It leaves collateral effects (Ex: car door opened -> turn on the light inside) 
- It can work internally and externally in the software
- Domain Events: a change in the application state / business logic.

## Event Types:
1) Event Notification: A short communication (a little payload that has little information). System A wants to notify system B that something changed inside System A.
2) Event Carried State Transfer: Completed format to traffic information. Data Stream. It does not bring the modification. It brings the whole data. Ex to use: duplicate the data between one database to another into the other microservice.
3) Event Sourcing: capture the events and put them inside a database. (Ex: time series database: a database focused on a timeline). This way you can do some replays in these events

## Other Concepts:
- Event Traditional Method: System A -> System B -> System C -> System D
- Event Collaboration Method: All the Systems are sending events all the moments to all the systems. This way all the systems have the information that they need, and they do not neet to get this information to do something

- CQRS (Command and Query Responsibility Segregation): It's an idea. It is not a standard implementation format.
  * Commands (write and update): This is a user change intent. Ex: "Create a product" and it is done (You do not query it to recover the ID to return to the caller. You throw an exception if get some error). It involves business logic
    ```
    Domain
      Commands
        Use Cases
          Repositories
    ```
  * Query (read): Queries. You forget the Application Domain and the Repository, and then you query only what you need.
    ```
    Query
      UseCases
        DAOs
    ```
  * You can keep the data in different databases (not obliged, but sometimes recommended, depending on the situation). One for Writer and other for reading. The writer can be a SQL (Ex: category: 1, product: 1) and the Reader can the NoSQL (Ex: category: toy, product: car)
  * You can use the "Event Sourcing" instead of doing the "Reader"
  * OBS: Repository in Domain Model is different from Repository of ORM. Repository from Domain Model always return the Domain from DDD. (The repository from ORM always return its orm model).


## Branas

- Unit of Work: Speaking about databases, it is a way to consist of the data in only one transaction. If something fails, rollback is needed. (usually we use @Transactional in Spring)


### CQRS:

- It is a model. There can be several examples. Using the same database or not.

- Example 1: Instead of getting information from a database calling different services, create a database with a read replica and get the information from it.
- Example 2: Even if it is the same service, create a read replica and get it from it.
- Example 3: Instead the UseCase gets a lot of repositories from its own service, creating its own select. (an example of the same database)
- Example 4: To create a read replica (if you are not using AWS that creates automatically the read replica), you can create events from the writer database and send it to the read database

- Solutions
  * A different Project Pattern to access the database
  * Create snapshots in the database with specific information (ex:
  * Projection Table for reading
  * Materialize Views
  * Separate Database of Read and Write
  * Use specific database for the purpose


### Events - Patterns:
- Retry: Attempt one or more tries at short intervals. It can solve simple problems such as packet loss, network oscillation, and deploying at the wrong time.
- Fallback: Attempt to use another service. For example, an e-commerce site could attempt transactions with several credit card acquirers to avoid unavailability (or even blocking).
- SAGA: Responsible for managing a long transaction through a sequence of local transactions.

### Events - Types of Transactions
- Pivot Transaction: The flux will go or will be aborted ("go" or "no go") (ex: cancel the order)
- Compensable Transaction: Is rollbacked in case of whole transaction is aborted
- Retriable Transaction: Guarantee of execution and can recover of a possible fail or unavailability. (ex: reprocess the order)

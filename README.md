# EDA - Event Driven Architecture

## Events: 
- An event is something that happened in the past
- It leaves collateral effects (Ex: car door opened -> turn on the light inside) 
- It can work internally and externally in the software
- Domain Events: a change in the application state / business logic.

## Event Types:
1) Event Notification: A short communication (a little payload that has little information). System A wants to notify system B that something changed inside System A.
2) Event Carried State Transfer: Completed format to traffic information. Data Stream. It does not bring the modification. It brings the whole data. Ex to use: duplicate the data between one database to another into the other microservice.
3) Event Sourcing: capture the events and put them inside a database. (Ex: time series database: a database focused on a timeline). This way you can do some replays in these events

## Other Concepts:
- Event Tradictional Method: System A -> System B -> System C -> System D
- Event Collaboration Method: All the Systems are sending events all the moments to all the systems. This way all the systems have the information that they need, and they do not neet to get this information to do something

- CQRS (Command and Query Responsibility Segregation): It an idea. It is not a standard implementation format.
  * Commands (write and update): This is a user change intent. Ex: "Create a product" and it is done. It involves business logic
    ```
    Domain
      Commands
        UseCases
          Repositories
    ```
  * Query (read): Queries. You forget the Application Domain and query only what you need.
    ```
    Query
      UseCases (Can be the controller)
        DAOs
    ```
  * You can keep the data in different data bases (not obliged, but sometimes recommended, depending on the situation). One for Writer and other for read. The writer can be a SQL (Ex: category: 1, product: 1) and the Reader can the NoSQL (Ex: category: toy, product: car)
  * You can use the "Event Sourcing" instead of doing the "Reader"
  * 

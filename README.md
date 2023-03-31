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
- Event Traditional Method: System A -> System B -> System C -> System D
- Event Collaboration Method: All the Systems are sending events all the moments to all the systems. This way all the systems have the information that they need, and they do not neet to get this information to do something

- CQRS (Command and Query Responsibility Segregation): It's an idea. It is not a standard implementation format.
  * Commands (write and update): This is a user change intent. Ex: "Create a product" and it is done. It involves business logic
    ```
    Domain
      Commands
        Use Cases
          Repositories
    ```
  * Query (read): Queries. You forget the Application Domain and query only what you need.
    ```
    Query
      UseCases (Can be the controller)
        DAOs
    ```
  * You can keep the data in different databases (not obliged, but sometimes recommended, depending on the situation). One for Writer and other for reading. The writer can be a SQL (Ex: category: 1, product: 1) and the Reader can the NoSQL (Ex: category: toy, product: car)
  * You can use the "Event Sourcing" instead of doing the "Reader"

## Interface for events

```
public interface Event { // Event (Loaded data)
    String getName();
    LocalDateTime getDateTime();
    Object getPayload();
}

public interface EventHandler { // Operations that will be executed when the event is called
    void handle(Event event);
}

public interface EventDispatcher { // Register the event and the operations, and dispatch/fire the event to execute the operations. It has to be implemented.
    void dispatch(Event event) throws Exception;
    
    void register(String eventName, EventHandler eventHandler) throws Exception;
    void remove(String eventName, EventHandler eventHandler) throws Exception;
    void has(String eventName, EventHandler eventHandler) throws Exception;
    void clear(String eventName, EventHandler eventHandler) throws Exception;
}
```

- Unit of Work: Speaking about databases, it is a way to consist of the data in only one transaction. If something fails, rollback is needed. (usually we use @Transactional in Spring)

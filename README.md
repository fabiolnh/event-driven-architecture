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


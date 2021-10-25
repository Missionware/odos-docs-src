# Object Server

ODOS Object Server is the component that orchestrates the clients, their operations as well as the storing of data to the appropriate data store. Main modules of the Object Server are:

- Client Manager

- Server Object Manager
  
  - Transaction Manager
  
  - Persistency Manager
  
  - Transaction Publisher
  
  - Object Cache

- RPC Broker 

- Authentication Manager

## Client Manager

The Client Manager module is responsible to handle the sessions of the client applications. It is continuously monitoring that the connections are live by sending "heartbeats" to the clients. 

> Note: Typically also the client applications are continuously monitoring the server connection. 

In case a client's connection gets broken the server cleanups any resources that retained for this client. 

The Client Manager uses the configured communication library and the Authentication Manager to perform connection and authentication. 

The communication library to be used is pluggable. Currently the following methods are supported:

- WebSocket (Missionware's implementation)

- Named Pipes (IPC)

- Remoting (IPC, Socket) - deprecated

- WCF - deprecated

Selecting a different communication library is very easy, just change a setting in the app.config file. 

## Server Object Manager

### Transaction Manager

The Transaction Manager is responsible to receive the transactions from the clients, to perform in-memory transaction, validating all the rules that retain the object model consistent, to use the Persistency Manager to store the updated data to the appropriate data store, to update the in-memory cache and then push the changes to the Transaction Publisher. 

### Persistency Manager

The Persistency Manager manages the various data stores that can be used. Each managed domain may have a different data store. Currently various data stores are supported: 

- Standard SQL 92 RDBMS through the built-in code-first ORM in what we call the OODB mapping schema:
  
  - MS SQL Server
  
  - Azure SQL Database
  
  - PostgreSQL

- Binary Files

Additional SQL Databases are very easy to be supported with the standard code-first OODB schema. 

Additionally support custom schema for the data store is under development in that we call RMDB schema. 

The plugging of the persistency library is done, similar to the communication one, by just setting the appropriate settings in app.config file. 

## Transaction Publisher

The Transaction Publisher is the module responsible to forward the transaction to the appropriate clients. Filtering of which transactions are routed to which clients can be configured programmatically or using the built-in object subscription. 

### Object Cache

The Object Cache of the Server keeps track of the subscriptions of the various Client Object Managers (COMs) to objects and in this way the Transaction Publisher can forward the transactions based on these subscriptions. 



## RPC Broker

The RPC Broker is the component responsible to forward commands from one client to another. P2P remote calls are also under development. The RPC can be easily constructed using the appropriate SDK and standard .NET interfaces implemented by both ends. 



## Authentication Manager

The Authentication Manager (AM) is the component that manages the internal identity domain. AM can communicate with various identity stores such as:

- Active Directory

- Any LDAP

- Windows Local (SAM DB)

- Linux Local (PAM)

- Open-ID Connect
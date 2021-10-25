# Object Managers

The object managers are the components that the ODOS DataItems live and are managed by. There are two main types of object managers.

## Server Object Manager (SOM)

The SOM is the component that performs the Object Transactions, initially in memory and then in the Data Store. So a SOM does the following:

1. Receive changesets from connected clients

2. Perform in-memory transactions by validating the changeset against the current object state and applying the state in server's cache

3. Write the data to the data store

4. Propagate the transaction to all connected clients 

SOM has a built in Object-Relation Mapping (ORM) engine to use standard RDBMS as its data store. Currently the following RDBMS are fully supported:

- MS SQL Server

- Azure SQL Server

- PostgreSQL

Nevertheless as the ORM is generic enough, ODOS can support easily and SQL 92 compatible RDBMS.

ODOS also has a built in Binary File data store adapter that can be used for various prototyping scenarios, tools or simple applications. 

## Client Object Manager (COM)

COM is the component that typically lives inside the client applications and exchanges changesets and transactions with the SOM.
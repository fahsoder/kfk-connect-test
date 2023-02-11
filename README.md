
# Kfk Connect Test

Kfk Connect Test is a project built to test the connectivity within kafka and sql server



## Stack

**Docker:**: Used to create and host containers running kafka connect and sql server


**Kafka**: Recommend you to use a online kafka server to plug in your kafka connect or you can even build up a local kafka server to do the same job

**Kafka Connect**: Used to connect in the SQL Server Database and post messages in the kafka server you're connected

**SQL Server**: Database where you are going to make inserts, alterations and deletes to generate event messages via kafka connect

## How to run

Clone this repo in your machine and execute the following command in your terminal (cmd or powershell):

`cd kfk-connect-test`

.

As soon as you are now into the project folder in your terminal, execute the following command:

`docker build . -t cdc:latest`


Well done :) Image created

Now let's run the containers executing the following command:

`docker-compose up`

The containers take a little bit to finish its loads, so wait for around a minute before starting working over them

If you don't want to see the execution of the containers in the terminal, run the command with the param `-d`:

`docker-compose up -d`


## Database

Now you need to connect your JDBC into your SQL Server that is running over the docker container

`Host: localhost`

`Port: 1433`

`Username: sa`

`Password: P@ssw0rd`

`Database/Schema: master`


Alright, you're connected to your SQL Server container 

Lets create a new database, table and insert some values executing the SQL Script `script.sql` you have in your project's folder


## Configuration

Wow, now you're with your infrastructure created and ready to use kafka connect

Lets make the connection between your kafka connect within your kafka and your database

For that, execute the following request: 

Remember to replace the `{kafka-server}` with your kafka server address + kafka server port `localhost:9092`

`curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" http://localhost:8083/connectors/ -d '{"name": "db-connector", "config": {"connector.class" : "io.debezium.connector.sqlserver.SqlServerConnector", "tasks.max" : "1", "database.server.name" : "sqlserver", "database.hostname" : "sqlserver", "database.port" : "1433", "database.user" : "sa", "database.password" : "P@ssw0rd", "database.dbname" : "db", "database.history.kafka.bootstrap.servers" : {kafka-server}, "database.history.kafka.topic": "schema-changes.db"}}'`

All set up! :)

Now all you have to do is execute some dabase operators (inserts, updates and deletes) and the messages on the kafka topic will appear magicly 
## Feedback

If you have a feedback, please let me know via this email address fahsoder@gmail.com



Thank you! Have a fun!

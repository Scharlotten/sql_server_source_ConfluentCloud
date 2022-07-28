# Considerations
Disclaimer - This was one of the most annoying connectors I had to set up so far
## Topics
If you are connecting to Confluent Cloud, create a topic called `server1` a topic called `server1.dbo.customers` and a last one `schem-changes.inventory`
All three topics are needed so the connector can start recording the messages. 

## Docker-compose 
* The connect image is actively waithing for sqlserver to come online by a healthcheck. If you remove this, connect will start too early and will complain about
not finding the database
* Look at the sql-server_conn.properties file and configure your API key and secrets since there is database.history tracking topic you need to authenticate in the connector config file not only in the worker file
* In case you are interested once sqlserver starts, you can see a bunch of commands that are run - 
It is a single SQL query that is being executed once the DB is up - what's interesting about this is that it does not only 
create a table but it also enables change data capture on the whole DB then on the table just in case (most likely it will say in the logs it can't enable it again as it is already enabled)

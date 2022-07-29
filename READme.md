# Considerations
If you would like to use the docker-compose file as it is within the sql_server folder
- remove the "_template" keyword from the end of `sql-server_conn_template.properties`
making it --> `sql-server_conn.properties` and apply the same to the worker file as well. `worker_template.properties` --> `worker.properties`

This is needed as the docker-compose file points to the files containing the API keys and secrets however that would be bad practice to upload that to git... 

## Topics
If you are connecting to Confluent Cloud, create a topic called 
* `server1` - If you want to change this update the database server name in `sql_server_conn_template.properties` this topic records
all the changes that happen in the DB
* `server1.dbo.customers` - this topic will contain the payload - what is actually inserted into the db
* `schema-changes.inventory` - this records DDL changes \n

All three topics are needed so the connector can start recording the messages. 

## Docker-compose 
* The connect image is actively waithing for sqlserver to come online by a **healthcheck**. If you remove this, connect will start too early and will complain about
not finding the database
* Add a `.env` file to your project and set your API_KEY and API_SECRET - docker should be able to pick these up 
automatically instead of replacing them in the main `docker-compose.yml` file
* Make sure you add your API key and secret to
     * `sql-server_conn.properties` file and configure your API key and secrets since there is database.history tracking
        topic you need to authenticate in the connector config file not only in the worker file
     * `worker.properties` file - this is needed because you are authenticating against Confluent Cloud, all 
        producers and consumers need to be configured correctly

_In case you are interested once sqlserver starts, a few commands are run - 
`001_tbl_creation.sql` is responsible for creating **testDB**, **customers** and **enabling change data capture on the DB**_
 
Happy streaming 
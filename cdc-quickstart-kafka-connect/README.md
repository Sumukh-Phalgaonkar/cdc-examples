# Running PG source with PG Debezium Connector

1. Bring up the docker containers
   ```
   docker compose up -d
   ```
  
2. Log in to postgres shell
   ```
   docker run --network=cdc-quickstart-kafka-connect_default -it --rm --name postgresqlterm --link pg:postgresql --rm postgres:11.2 sh -c 'PGPASSWORD=postgres exec psql -h pg -p "$POSTGRES_PORT_5432_TCP_PORT" -U postgres'
   ```
3. Create tables
    
    ```
    CREATE TABLE t1 (id int primary key, value_1 text default 'abc', value_2 text default '123');
    ```
  
4. Deploy the connector.
   Here is a sample config to deploy connector. Make changes appropriately.
    ```
   curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" localhost:8083/connectors/ -d '{
   "name": "ybconnector1",
   "config": {
     "plugin.name":"pgoutput",
     "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
     "database.hostname":"172.165.27.143",
     "database.port":"5432",
     "database.user": "postgres",
     "database.password":"postgres",
     "database.dbname":"postgres",
     "database.server.name":"ybconnector1",
     "snapshot.mode":"never",
     "provide.transaction.metadata":"true",
     "topic.prefix":"vanilla",
     "table.include.list":"public.t1"
   }
   }'
    ```

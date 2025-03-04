# debezium
Services from https://debezium.io/documentation/reference/stable/tutorial.html on 1 compose file


Get connector list 
curl -H "Accept:application/json" localhost:8083/connectors/
curl -s -X GET -H "Accept:application/json" localhost:8083/connectors/inventory-connector | jq

Connect to db
> docker compose exec -it <container_name> bash
Enter sql
> mysql -hlocalhost -P3306 -uroot -pdebezium
use table inventory
> use inventory;
show tables in database inventory
> show tables;
show all data in table customers
> SELECT * FROM customers;
update one entity
> UPDATE customers SET first_name='Anne Marie' WHERE id=1004;

Postgres DATABASE
Check WAL, needs logical
SHOW wal_level;

Connect to db
psql -h <pg_host> -U <pg_user> -d postgres
Create table
CREATE DATABASE "inventory-foo";
Connect to db inventory
\c "inventory-foo"
Create table consumers
CREATE TABLE "consumers-foo" (
   id SERIAL PRIMARY KEY,
   first_name TEXT NOT NULL,
   last_name TEXT NOT NULL,
   email TEXT UNIQUE NOT NULL
);

Insert data in the table consumers-foo
INSERT INTO "consumers-foo" (id, first_name, last_name, email) VALUES (1001, 'Bob', 'Smith', 'bob@bob.com');
Show entities
SELECT * FROM "consumers-foo";
Update intity 
UPDATE "consumers-foo" 
SET first_name = 'Robert' 
WHERE id = 1001;


get kafka entities. run it from container 
> kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic dbserver1.inventory.customers --from-beginning
Either use this command:
docker compose exec -it kafka sh -c "./bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic dbserver1.inventory.customers --from-beginning" | jq
docker compose exec -it kafka sh -c "./bin/kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic dbpg15.inventory-foo.consumers-foo --from-beginning" | jq

Show kafka topic lists
docker compose exec -it kafka sh -c "./bin/kafka-topics.sh --bootstrap-server kafka:9092 --list" 

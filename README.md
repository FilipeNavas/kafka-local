# Kafka Local Development alternative (especially for Mac M1 users)

Goal
====
### To be an alternative to [Lenses](https://github.com/lensesio/fast-data-dev) and [Confluent Platform](https://docs.confluent.io/platform/current/quickstart/ce-docker-quickstart.html) for local development, **especially for Mac M1 users.**

---
This docker-compose is based on the [demos available from Confluent on Github](https://github.com/confluentinc/demo-scene). It uses many of Confluent images, except the one for Control Center. That is because Control Center doesn't work on Mac M1 yet - even under emulation. For a GUI we use Kafka UI.

Pre-requisites
====
You need to have Docker and Docker Compose installed and running in your computer. If you have installed [Docker Desktop](https://www.docker.com/products/docker-desktop) you are good to go.

How to use
====
Clone this repo, access it locally in your terminal, run `docker-compose up -d` and it should start up all services.

Check your Docker/Docker Desktop to view that all services are running.

Access [Kafka UI](http://localhost:3030) on `http://localhost:3030` to manage your Kafka cluster. (you can change this port by updating the kafka-ui port mapping in the docker-compose file).

On Kafka UI in the browser, refresh the page until you see that the broker is online (it may take a few minutes).

How it works
====
Check the `docker-compose.yml` file to view all services and configurations. On a high level, the services are:

- [Zookeper](https://hub.docker.com/r/confluentinc/cp-zookeeper)
- [Kafka broker](https://hub.docker.com/r/confluentinc/cp-kafka)
- [Schema registry](https://hub.docker.com/r/confluentinc/cp-schema-registry)
- [Kafka connect](https://hub.docker.com/r/confluentinc/cp-kafka-connect-base) - used if you need to run CDC Connectors (such as MySQL Debezium, already included in the docker-compose file)
- [ksqldb](https://hub.docker.com/r/confluentinc/ksqldb-server)
- [Kafka UI](https://github.com/provectus/kafka-ui)

How to create connectors 
====
To create connectors you can try using Kafka UI by creating a new connector and providing its configuration in JSON.

NOTE: in the `kafka-connect` service -> `command` section in the docker-compose file it already installs the Debezium MySQL connector. If you need another type of connector, add it there. Check the [Confluent Connectors docs for more connectors.](https://docs.confluent.io/home/connect/self-managed/kafka_connectors.html)
To check which connectors you have installed navigate to [http://localhost:8083/connector-plugins](http://localhost:8083/connector-plugins) on your browser.

You can also make an HTTP POST request to `http://localhost:8083/connectors/` with the configuration (via Postman or curl for example).

==> IMPORTANT: Note that the **bootstrap servers** need to be set to `broker:29092` and the **schema registry** to `http://schema-registry:8081` on your connector configuration.

Troubleshooting
===
**Kafka Connect is not connecting to the broker**

If you see a message similar to `expected 1 broker node, found 0` on the kafka-connect logs in Docker, try restarting it. Sometimes it times out before the broker is ready.

Updates are appreciated
===
If you have updates feel free to open a PR. The main goal of this repo is to provide a local Kafka environment for Mac M1 users.
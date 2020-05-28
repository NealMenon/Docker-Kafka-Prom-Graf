# Setting up Kafka monitoring using Prometheus and Grafana on Docker

Kafka is a distributed streaming platform that is used publish and subscribe to streams of records. Kafka is used for fault tolerant storage. It replicates topic log partitions to multiple servers. Kafka is designed to allow your apps to process records as they occur. 

## Prerequisites
This tutorial assumes that you have stable build of docker and docker-compose pre-installed. Installation guidelines can be found [here](https://docs.docker.com/compose/install/).

## Initializing your containers

In a suitable location, clone the set up repo. Switch to this directory and call docker-compose to start your containers
```
$ git clone https://github.com/NealMenon/Docker-Kafka-Prom-Graf.git
$ cd Docker-Kafka-Prom-Graf
$ docker-compose up -d
```
The -d flag starts the containers in background mode. At the point, you may call ``docker-compose ps`` to confirm that your 4 containers are up. Additionally, you may shut down your containers using ``docker-compose stop CONTAINER_NAME``

## Kafka set up
To similiate a kafka set up, we will set up a sample Producer and Consumer under a sample topic. Start by enterring the kafka container using the docker command and creating a topic
```
$ sudo docker exec -it kafka /bin/sh
$ cd /opt/kafka
$ ./bin/kafka-topics.sh --create --zookeeper:2181 --replication-factor 1 --partitions 1 --topic sachin
```
Now that we have a sample topic called sachin, we will link a producer to this topic for it to publish to.


## Prometheus set up

Opening [Prometheus Dashboard](http://localhost:9090/graph) show a rudimentary view of all Kafka metrics. Confirm that Kafka data is being scraped by visiting [targets](http://localhost:9090/targets), where the endpoint kafka should be up.

## Grafana set up

### Log in
Login into [Grafana](http://localhost:3000/login) using credentials 'admin' in both user and password fields. You may or may not choose to skip changing the password. 

### Add Data Source
Before setting up a dashboard, Prometheus must be added as a data source. Do so by clicking on the ``Add you first data source card`` and choosing Prometheus. Assign a suitable name for this and provide the source URL as ``http://localhost:9090/`` and Access type of Browser (not Server). Confirm that everything is set upright with the Save & Test button and return to Grafana dashboard. 

### Add basic dashboard
On the left side of the dashboard, hover over the Create tab and choose Import. Enter 721 under Import via Grafana and Load. Once again, assign a suitable name and choose source as the same Prometheus data source we set up above and import. 
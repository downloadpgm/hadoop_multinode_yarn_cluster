# Hadoop running into YARN cluster in Docker

Apache Hadoop YARN is a resource management and job scheduling technology in the open source Hadoop distributed processing framework.
This Docker image contains Hadoop binaries prebuilt and uploaded in Docker Hub.

## Shell Scripts Inside 

> run_hadoop.sh

Sets up the environment for the YARN cluster by executing the following steps :
- sets environment variables for HADOOP and YARN
- starts the SSH service and scans the slave nodes for passwordless SSH
- copies the Hadoop configuration files to slave nodes
- initializes the HDFS filesystem
- starts Hadoop Name node and Data nodes
- starts YARN Resource and Node managers

> create_conf_files.sh

Creates the following Hadoop files $HADOOP/etc/hadoop directory :
- core-site.xml
- hdfs-site.xml
- mapred-site.xml
- yarn-site.xml
- hadoop-env.sh

## Initial Steps on Docker Swarm

To start with, start Swarm mode in Docker in node1
```shell
$ docker swarm init
Swarm initialized: current node (xv7mhbt8ncn6i9iwhy8ysemik) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token <token> <IP node1>:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

Add more workers in two or more hosts (node2, node3, ...) by joining them to manager running the following command in each node.
```shell
$ docker swarm join --token <token> <IP node1>:2377
```

Change the workers as managers in node2, node3, ... running the following in node1.
```shell
$ docker node promote node2
$ docker node promote node3
$ docker node promote ...
```

Start the YARN cluster by creating a Docker stack 
```shell
$ docker stack deploy -c docker-compose.yml yarn
```



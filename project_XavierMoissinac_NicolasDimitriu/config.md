### **1. Kafka with Docker**

There are official Docker images for Kafka and Zookeeper. Here’s how to set them up.

### Installation Steps:

1. **Create a Docker network** (optional but useful for communication between containers):

    ```bash
    docker network create kafka-network
    ```

2. **Launch Zookeeper** (Kafka needs it to function):

    ```bash
    docker run -d --name zookeeper --network kafka-network -e ZOOKEEPER_CLIENT_PORT=2181 -p 2181:2181 wurstmeister/zookeeper
    ```

3. **Launch Kafka**:

    ```bash
    docker run -d --name kafka --network kafka-network -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9093 -e KAFKA_LISTENER_SECURITY_PROTOCOL=PLAINTEXT -e KAFKA_LISTENER_NAME=PLAINTEXT -e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9093 -e KAFKA_LISTENER_PORT=9093 -e KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181 -p 9093:9093 wurstmeister/kafka
    ```

    This will start Kafka on port 9093 and connect it to Zookeeper.

### Basic Usage:

- Create a Kafka topic:

    ```bash
    docker exec -it kafka kafka-topics.sh --create --topic nom_du_topic --bootstrap-server localhost:9093 --partitions 1 --replication-factor 1
    ```

### **2. HDFS with Docker**

There are Docker images for Hadoop (HDFS). You can use a ready-to-use image to quickly get started.

### Installation Steps:

1. **Launch HDFS with Docker**: Use the `sequenceiq/hadoop-docker` image to deploy a Hadoop cluster with HDFS:

    ```bash
    docker run -d --name hadoop -p 50070:50070 -p 9000:9000 sequenceiq/hadoop-docker:2.7.0 /etc/bootstrap.sh -d
    ```

    This will start Hadoop with HDFS. You can access the HDFS interface at `http://localhost:50070`.


We encountered a problem here when running this code. If you have the same as us try this: 

```bash
docker run -d --name hadoop -p 50070:50070 -p 9000:9000 harisekhon/hadoop
```

### Basic Usage:

- Create a directory in HDFS:

    ```bash
    docker exec -it hadoop hadoop fs -mkdir /chemin/vers/dossier
    ```

### **3. Hive with Docker**

For Hive, you can use a pre-configured Docker image, often with a MySQL database as the backend for metadata.

### Installation Steps:

1. **Launch MySQL for Hive**:

    ```bash
    docker run -d --name mysql-hive -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=hive -p 3306:3306 mysql:5.7
    ```

2. **Launch Hive with Docker**: You can use an image like `bde2020/hive-docker` for Hive:

    ```bash
    docker run -d --name hive --network kafka-network -e HIVE_METASTORE_URI=thrift://localhost:9083 -p 10000:10000 bde2020/hive:2.3.0-postgresql
    ```

    We encountered a problem here when running this code. If you have the same as us try this: 

    ```bash
    docker run -d --name mysql-hive -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=hive -p 3306:3306 mysql/mysql-server:5.7
    ```

    This will start Hive with MySQL as the backend for metadata.

### Basic Usage:

- Start the Hive shell:

    ```bash
    docker exec -it hive /opt/hive/bin/hive
    ```

### **4. Spark with Docker**

Spark can easily be launched via Docker. You can use the official image or an image with Hadoop for distributed environments.

### Installation Steps:

1. **Launch Spark in local mode** (if you want to use Spark locally without Hadoop):

    ```bash
    docker run -d --name spark --network kafka-network -p 4040:4040 bitnami/spark:latest
    ```

2. **Launch Spark with Hadoop** (if you want to use Spark with Hadoop/HDFS):

    ```bash
    docker run -d --name spark-hadoop --network kafka-network -p 4040:4040 -p 7077:7077 bde2020/spark-hadoop:2.4.5-hadoop2.7
    ```

    We encountered a problem here when running this code. If you have the same as us try this: 

    ```bash
    docker run -d --name hive --network kafka-network -e HIVE_METASTORE_URI=thrift://localhost:9083 -p 10000:10000 bde2020/hive
    ```

### Basic Usage:

- Launch the Spark shell:

    ```bash
    docker exec -it spark /opt/spark/bin/spark-shell
    ```


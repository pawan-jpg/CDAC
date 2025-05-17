# **Comprehensive Guide: Kafka-PySpark Integration for Stream Processing**

This document explains how to use **Apache Kafka** with **PySpark Structured Streaming** for real-time data processing. Below is a detailed breakdown of each step.

---

## **1. Kafka Topic Management Commands**
These commands are executed in a **terminal/bash shell** to manage Kafka topics.

### **Create a Kafka Topic**
```bash
kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic first_topic --create --partitions 3
```
- Creates a topic `first_topic` with **3 partitions** (default replication factor = 1).

```bash
kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic first_topic --create --partitions 3 --replication-factor 2
```
- Creates `first_topic` with **3 partitions and 2 replicas** (for fault tolerance).

```bash
kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic first_topic --create --partitions 3 --replication-factor 1 --topic voters
```
- **❌ Incorrect Syntax**: This command tries to create two topics (`first_topic` and `voters`) but will fail.  
- **Fix**: Use separate commands for each topic.

### **List and Delete Topics**
```bash
kafka-topics.sh --zookeeper 127.0.0.1:2181 --list
```
- Lists all Kafka topics.

```bash
kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic <topicname> --delete
```
- Deletes a specified topic.

```bash
kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic new_topic --delete
```
- Deletes `new_topic`.

---

## **2. Producing and Consuming Messages**
### **Produce Messages to Kafka**
```bash
kafka-console-producer.sh --broker-list 127.0.0.1:9092 --topic first_topic --producer-property acks=all
```
- Starts a producer that sends messages to `first_topic`.
- `acks=all` ensures messages are **fully replicated** before acknowledgment.

### **Consume Messages from Kafka**
```bash
kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic --from-beginning
```
- Consumes messages from `first_topic` starting from the earliest offset.

---

## **3. PySpark-Kafka Integration**
### **Start PySpark Shell with Kafka Dependency**
```bash
pyspark --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.4.5 --master local[2]
```
- Launches PySpark with **Kafka integration** (`spark-sql-kafka`).
- `local[2]` runs Spark with **2 threads** (for parallel processing).

### **Create a Kafka Topic for PySpark**
```bash
kafka-topics.sh --create --zookeeper 127.0.0.1:2181 --replication-factor 1 --partitions 1 --topic voters
```
- Creates `voters` topic with **1 partition and 1 replica**.

### **Streaming Data from Kafka in PySpark**
#### **(3) Create a Streaming DataFrame**
```python
kafkaVoterDF = spark.readStream.format("kafka") \
    .option("kafka.bootstrap.servers", "127.0.0.1:9092") \
    .option("subscribe", "voters") \
    .option("startingOffsets", "earliest") \
    .load()
```
- Reads from `voters` topic starting from the earliest offset.

#### **(4) Display Raw Kafka Data**
```python
rawVoterQuery = kafkaVoterDF.writeStream \
    .trigger(processingTime='10 seconds') \
    .outputMode("append") \
    .format("console") \
    .start()
```
- **Trigger**: Processes data every **10 seconds**.
- **Output Mode**: `append` (new data only).
- **Sink**: `console` (prints to Spark shell).

#### **(6) Push Data to Kafka**
```bash
./pushKafkaData.sh
```
- Assumes a script (`pushKafkaData.sh`) sends JSON-formatted voter data to Kafka.

#### **(8) Stop the Streaming Query**
```python
rawVoterQuery.stop()
```
- Stops the streaming job.

#### **(10) Extract Kafka Payload as String**
```python
voterStringDF = kafkaVoterDF.selectExpr("CAST(value AS STRING)")
stringVoterQuery = voterStringDF.writeStream \
    .trigger(processingTime='10 seconds') \
    .outputMode("append") \
    .format("console") \
    .option("truncate", "false") \
    .start()
```
- Converts Kafka binary `value` to **String** and displays it.

#### **(12) Parse JSON and Aggregate Data**
```python
from pyspark.sql.types import StringType, LongType, StructType, StructField
from pyspark.sql.functions import from_json

voterSchema = StructType([
    StructField("gender", StringType(), True),
    StructField("age", LongType(), True),
    StructField("party", StringType(), True)
])

voterStatsDF = kafkaVoterDF.select(
    from_json(kafkaVoterDF["value"].cast(StringType()), voterSchema).alias("voterJSON")
).groupBy("voterJSON.gender", "voterJSON.party").count()

voterStatsQuery = voterStatsDF.writeStream \
    .trigger(processingTime='1 minute') \
    .outputMode("complete") \
    .format("console") \
    .start()
```
- **Schema**: Defines JSON structure (`gender`, `age`, `party`).
- **Processing**: Groups by `gender` and `party` to count voters.
- **Output Mode**: `complete` (shows full aggregation).

#### **(13) Stop the Query**
```python
voterStatsQuery.stop()
```
- Terminates the streaming job.

---

## **Key Takeaways**
1. **Kafka Topics** must be created before streaming.
2. **PySpark Structured Streaming** reads Kafka data in real-time.
3. **Schema Enforcement** ensures proper JSON parsing.
4. **Aggregations** (`groupBy`, `count`) work in streaming mode.
5. **Triggers** control batch processing intervals (`10s`, `1m`).
6. **Output Modes**:
   - `append` (new rows only).
   - `complete` (full aggregation results).

---

## **References**
- [Spark-Kafka Integration Guide](http://spark.apache.org/docs/2.4.5/structured-streaming-kafka-integration.html)

This setup enables **real-time voter data analysis** (e.g., gender/party distribution) using **Kafka + PySpark**. 🚀

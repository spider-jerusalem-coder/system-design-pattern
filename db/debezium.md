# Debezium

### 1. What kind of delivery debezium supports ?
Atleast once (when debezium crashes and it hasnt recorded the offset)

### 2. How to maintain order in debezium ?
Send changes to one topic only

### 3. How does debezium connect with Kafka ?
Using Kafka connect framework (kafka connector)

### 4. What databases connector does debezium support ?
* PostgreSQL
* MySQL
* MongoDB
* Cassandra

### 5. What is the schema of the event in the CDC ?
```python
{
  key: ... # primary key
  value: {
    before: {},
    after: {}
  },
  source: {
    # table name and metadata
  },
  ts_ms: 1650007000 # timestamp of change data capture,
  operation: 'U' | 'I' | 'D' # update, insert, delete
}
```

### 6. What formats does debezium publishes events in ?
1. JSON (default)
2. Avro ()

### 7. What are the benefits of using Avro in debezium
* Performance: As schema is provided then faster serialization and deserialization
* Integration with Kafka: Kafka would store it in schema registry and validate message at producer and consumer end
* Support for Schema evolution: Can you use old schema to deserialize new version of schema based data

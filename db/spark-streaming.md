# Spark streaming

### 1. Is it realtime
No. It uses micro-batches

### 2. Do you have to start just once ?
Yes. it will continously stream from the source interms of micro-batches

### 3. What command is used to sort the data and is it total order sorted or sort per partition ?
It is total order sorted and not per partition. its a wide transformation. `sort`

### 4. During stream, where is the data stored on the nodes ? 
In-memory until its explicitly mentioned to be checkpointed in HDFS or S3

### 5. How is a new set of batch accomodated and what command is used ?
`updateStateByKey` or `mapWithState` which takes a new set of batch and finds where the node these key are located and update the value for these keys

### 6. Counter part to spark streaming
Flink or AWS Firehose

### 7. Can you run count min sketch in it
Yes by providing it explicitly

### 8. What is the code of top k items
``` python
# Function to update the running count of each key
def updateFunction(newValues, runningCount):
    if runningCount is None:
        runningCount = 0
    return sum(newValues, runningCount)

# Number of top elements to retrieve
K = 10

# Create a SparkContext and a StreamingContext
sc = SparkContext("local[2]", "TopKElements")
ssc = StreamingContext(sc, 1) # 1-second batch interval
ssc.checkpoint("/path/to/checkpoint-dir")

# Create a DStream that connects to a server socket
lines = ssc.socketTextStream("localhost", 9999)

# Assuming each line in the DStream is a (key, value) pair
pairs = lines.map(lambda line: (line, 1))

# Update the running counts
runningCounts = pairs.updateStateByKey(updateFunction)

# Function to extract and print the top K counts
def process(time, rdd):
    sortedRdd = rdd.sortBy(lambda x: x[1], ascending=False)
    topK = sortedRdd.take(K)
    print(f"Top {K} elements at {time}: {topK}")

# Apply the processing function to each RDD
runningCounts.foreachRDD(process)

# Start the computation
ssc.start()
ssc.awaitTermination()
```

### 9. What is the code for top k without sortby
```python
def process(time, rdd):
    topK = rdd.top(K, key=lambda x: x[1])
    print(f"Top {K} elements at {time}: {topK}")
```
> Prirotiy queue is maintained

### 10. What is the code for rolling window top k
```python
from pyspark import SparkContext
from pyspark.streaming import StreamingContext

# Function to update the counts
def updateFunction(newValues, runningCount):
    if runningCount is None:
        runningCount = 0
    return sum(newValues, runningCount)

# Number of top elements to retrieve
K = 10

# Create SparkContext and StreamingContext
sc = SparkContext("local[2]", "RollingWindowTopK")
ssc = StreamingContext(sc, 1) # 1-second batch interval
ssc.checkpoint("path/to/checkpoint-dir")

# Create a DStream that connects to a server socket
lines = ssc.socketTextStream("localhost", 9999)

# Assuming each line is a (key, value) pair
pairs = lines.map(lambda line: (line, 1))

# Define the window parameters: window length and sliding interval
windowLength = 30 * 60  # 30 minutes
slidingInterval = 10 * 60  # 10 minutes

# Apply window operation
windowedPairs = pairs.reduceByKeyAndWindow(lambda x, y: x + y, windowLength, slidingInterval)

# Function to get top K elements
def getTopK(rdd):
    return rdd.top(K, key=lambda x: x[1])

# Get top K elements for each windowed RDD
topKStream = windowedPairs.transform(getTopK)

# Print the result
topKStream.pprint()

# Start the computation
ssc.start()
ssc.awaitTermination()
```

### 11. How does `reduceByKeyAndWindow` work ?
Each node has a set of data for a window and they slide the window in each node

### 12. What to Fraud detection using spark streaming ?
* Spark Streaming creates one-minute batches from this input data. 
* The Spark engine processes each one minute batch and figures out the fraudulent transactions using already trained fraud detection model.

### 13. What are the application of spark streaming ?
* Netflix uses for movie reccoo
* Uber uses for telemtry analysis
* Fraud detection
* Pinterest how users are engaing with pins

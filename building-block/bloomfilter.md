# Bloomfilter

* Number of elements to be inserted = n = 100M
* Probability of false positive = 1.0E-7 (1 in 1M)

* Number of bits used = m = n(ln(p)) / (ln2)**2
* Number of hash functions = k =  (ln2) * (m / n) 

> Size of the Bloom filter (m): Approximately 3.35 billion bits (or around 419.3 megabytes, as 1 byte = 8 bits).
> 
> Optimal number of hash functions (k): About 23 hash functions.

# Challenges
* Increase in m leads to more storage
* Increase in hash functions leads to higher query latency as key has to pass through all k hash functions

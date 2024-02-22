# Mysql

## 1. How to make it distributed database
Use Vitess

## 2. How does Vitess support linearizabilty ?
Using 2 phase commit. Transaction running on all partitions or nothing. Creating global order of transactions

## 3. How many transactions can mySQL support per second ? How would you calculate it ?
* https://sirupsen.com/napkin/problem-10-mysql-transactions-per-second
* Maximum fsyncs per seconds =  1 / latency of fsync (1ms) = 1 K
* If every write leads to fsync then 1 K / sec
* But we batch so its closes to 10 K / s

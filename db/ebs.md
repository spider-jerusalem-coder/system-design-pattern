# EBS
* EC2 --> EBS volumen --> EBS volumne --> ...
* Request from EC2 to EBS volumne over network

### 1a. If an EC2 dies what is the recovery plan
A new EC2 is spin up and the EBS volumnes with attached disk is then attached to the new EC2

### 1b. What replication is used by EBS
Chain replication

### 1c. How is it better than running on EC2
Its fault tolerant

### 1d. Challenges regarding it
* Only one ec2 instance can attach EBS volumne (network based storage system).
* Sending large data over network.
* EBS are all stored in one DC by AZ so if a dc fails then not fault tolerant.

### 1e. What is the upgraded version of EBS
Aurora

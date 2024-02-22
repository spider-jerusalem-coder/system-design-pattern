# Raft

### 1. Why is it better than Paxos ?
* Understandibility
* Ease of implementation

### 2. What are the main key points of Raft
* Leader election
* Log replication
* Safety
* Membership changes

### 3. Explain Leader election
* Using randomization to east the process (choose any it wont matter)
* When a leader fails or disconnected, a new leader is elected
* Client connect to the leader
* Every follower have a heartbeat timeout and would trigger an election if its crossed

####  
* Follower detecting timeout would mark iself as a candidate
* Follower would RequestVote() to all nodes and also cast one vote for itself
* Three possibilities can happen
  * Follower wins the election
  * No candidate wins the election
  * Some other candidate wins the election


### 4. How is log replication done in raft
* Leader sends log to the other nodes and to maintain a consistent set of logs across the nodes

### 5. How is safety done in raft ?
* if any node applies a log entry in its log then no other node would be applying a different for the same log index

### 6. What are the different RPCs across nodes
* RequestVote() initiated by the candidates
* AppendEntries() initiated by the leader to append log entries to the follower nodes
  * AppendEntries RPCs that carry no log entries as form of heartbeat

### 7. What are the different states in which Raft node can be in ?
* Node can be in three states
* Leader, Follower, Candidate

 ### 8. How is a leader elected in Raft
* Follower recieve messages from Leader or Candidate
* Term is the time period for which a leader remains elected
* Term act as logical clock
* Each node stores the current term number which increases monotonically
* During exachange if a node discovers its current term is behind, it updates to the latest term number
* If a leader or follower encounters its current term number to be stales, it immmediately moves to the follower state
* if a node recieved a request with a stale term number, it rejects the request

  

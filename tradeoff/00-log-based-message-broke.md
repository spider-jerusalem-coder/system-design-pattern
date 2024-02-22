### Benefit
- **Scalable**
  - Due to partitions in each topics
  - On disk storage
- **Durable**
  - Ability to re-run logs based onn offset

### Challenges
- **At-least once delivery**
  - Can lead to duplicate counting
  - Idempotency at application level
-  **Consmer Lag**
   - Monitor consumer lag and provision nodes accordingly
 - Probability of missing update if the speed of writing into a partition is faster than consuming and the **data is overwritten**
   - Data retention policy 
- **Broker also need to be scaled along with the DB**

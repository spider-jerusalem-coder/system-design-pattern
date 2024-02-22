# Load Balancer
* Load balancer automatically distributes incoming traffic across multiple servers.
* Routes request based on url path

## State
* Active Active 
* Active Passive

### Active Active
CDNS use this.
#### Use case
* Used in higher traffic scenario
* Global traffic distribution
* Redundancy and High Availability (low downtime)
  
#### Benefits
* Higher availability
* Higher scalability

#### Challenges
* Cost
* Complexity

## Active Passive
### Use case
* Request to only one load balancer. Used in lower traffic scenario
* Used in disaster recovery scenario
* Used in cost effective redundancy
* Variable traffic pattern
  
### Benefits
* Simple
* Less costly

### Challenges
* Limited scalability
* Availability suffer at times of fail over
* Resource under-utilization
  
## Types
* Level 4 (IP layer, TCP)
* Level 7 (Application layer)

### L4
Update the haproxy config to `tcp` mode

#### Benefits
* Lightweight and performant (do not check message content)
* Health check
* Hiding internall network from public internet
* Queueing connection to prevent server overload
* Rate limiting connections

#### Challenges
* Can not do TLS termination (pod or web server need to do it)

### L7
Update the haproxy config to `http` mode, Yoy get everything from L4 and more

#### Benefits
* Routes based HTTP headers, url, cookie
* Alter HTTP messages
* Rate limiting at more granular level
* TLS termination
* TLS termination



## Implementations
### 1. Hardware
#### Benefits
* Performance

#### Challenges
* Expensive
* Vendor specific
* Requires human intervention on premises
* Difficult to horizontal scale

### 2. Software
### Benefits
* Cheaper
* Easy to scale (Non commodity node)
* TLS termination and request routing (programmable)

### Challenges
* Not as fast as hardware balancers

### 3. Cloud
#### Benefits
* Less Maintannce
* Observability
* Low time to setup
* Can be used as a global load balancer
* Used along with on premise load balancer
* Metered cost
* Auditing

#### Challenges
* Cost

## Global load balancer
* **DNS based** (Amazon Route 53 or Cloudflare  based on client ip adddress)
* **Any Cast**: (Multiple nodes are provided the same ip addresses, closest hop address is chosen)

### DNS Based
* Return a list of ip addresses which have lower network cost

#### Challenges
* Client would use stale content due to TTL
* Client might order the ip address as per its own choice
* DNS packet is small (512B) so large number of ip address can not be send

### Any Cast
* **Based on proximity** (Network routing protocol would store the closet node ip based on hops, path cost and network latency)
* **Based on BGP** (Dynamic routing protocol will store the min cost path to the ip)
  
#### Challenges
* Doesnt support statefull applications on the server side (as request is routed to different machines)

#### Overcome
* **Session is maintained at client side**

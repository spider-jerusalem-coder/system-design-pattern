## Bi Directional Communication

1. Websockets
2. Regular Polling
3. Long Polling

### Websockets
* Build on top of HTTP protocol
 
#### Benefits
* Low latency

#### Challenges
* Resource intensive (number of connections/sockets are bounded)
* Browser support
* De-provisioning a node is complex (have to wait for node to drain all connections)
* Lack of built in features (compression, caching or content distribution)
* Complexity (error handling, message guarantees)

### Long Polling
* Long polling is a technique used in web development to allow a server to push information to a client as soon as new data is available.
* Long polling is often used in situations where it's crucial to get updates in a timely manner but where real-time WebSocket connections are not feasible or necessary.
 
 #### How does it work
- Client sends a request to the server for the data
- Server do not respond back immediately but after a certain time period (1 min) if the data is available
- If the data is not available, server responds back with empty
- Client can again retry the same operation
  
#### Benefits
- **Simplicity**: (No additional protocol needed)
- **Lesser request count**: (Reduce number of request / response)
- **Compatibility / Support**

#### Challenges
- **Resource intensive** for server with larger scale clients
- **Complexity at server**
- **Latency in setting up** new polling request as compared to websocket

#### Real world
* socket.io in JS
* websocket / tornado in Python

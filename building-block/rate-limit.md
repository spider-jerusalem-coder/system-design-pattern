# Rate Limit
* Rate limit for the whole service or each endpoint

## Tradeoffs
### Benefits
* Stability and reliability
* Latency would be under SLA under increasing workload
* Rate of flow of request is maintained

### Challenges
* Additional hop or computation
* Incorrect limit can lead to bad user experience
* Long lived requests would lead to large number of concurrent requests

## Limits
* Reads per second = 100
* Writes per second = 100

## API Schema
* is_request_allowed(token)
  * response: `200`| `429`
  * response: `Headers: rate-limited-till: epoch_timestamp; `
 
## Client based handling
When `429` is observed, client should retry after the given ts mentioned in the header


## Challenges
### Thundering herd problem
If the client is using exponential backoff to handle `429` error code, then it can lead to thundering herd problem where 
multiple process wake up at the same time and would again be rate-limited similar how it was prior to them being rate-limited.
> use jitter when setting the exponential backoff to randomize the wakeup time

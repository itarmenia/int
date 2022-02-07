@CircuitBreaker Circuit breaker—Stops making requests when an invoked service is failing

Retry—Retries a service when it temporarily fails

@Bulkhead Bulkhead—Limits the number of outgoing concurrent service requests to avoid overload
	Semaphore bulkhead—Uses a semaphore isolation approach, limiting the number of concurrent requests to the service. Once the limit is reached, it starts reject- ing requests.
	Thread pool bulkhead—Uses a bounded queue and a fixed thread pool. This approach only rejects a request when the pool and the queue are full.

@RateLimiter Rate limit—Limits the number of calls that a service receives at a time

Fallback—Sets alternative paths for failing requests

The main difference between the bulkhead and the rate limiter pattern is that the bulkhead pattern 
is in charge of limiting the number of concurrent calls (for exam- ple, 
it only allows X concurrent calls at a time). With the rate limiter, we can limit the number 
of total calls in a given timeframe (for example, allow X number of calls every Y seconds).
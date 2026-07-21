# Collection Engine — Performance / Load Test Summary (Sample)

> Representative load test report for portfolio purposes. Figures are illustrative of a real
> test structure and scale, not tied to any specific company's production environment.

## Test Configuration

| Parameter | Value |
|---|---|
| Tool | Apache JMeter |
| Test Type | Sustained Load / Soak Test |
| Duration | 6 hours |
| Merchants Simulated | 62 |
| Total Transactions Generated | 405,067 |
| Target Throughput | 80.2 TPS |

## Results

| Metric | Result |
|---|---|
| Stable Throughput Achieved | ~78.5 TPS – ~80.2 TPS |
| Peak Throughput Validated | ~100 TPS |
| Error Rate | 0.001% |
| P90 Latency | 82 ms |
| P95 Latency | 319 ms |
| P99 Latency | 1500 ms |
| Uptime During Test | 99.9%+ |

## Bottleneck Analysis

The application layer sustained target throughput comfortably. The limiting factor observed was
**Redis queue memory** under sustained load — as the queue backlog grew during traffic spikes,
memory pressure on the Redis instance became the bottleneck before the application's own
processing capacity was exhausted.

**Recommendation:** size the Redis queue's memory allocation for peak-hour traffic patterns, not
average throughput, and add queue-depth alerting ahead of the memory ceiling.

## Test Coverage During Load Run

- Login
- Collection initiation (UPI)
- Transaction status polling
- Dashboard refresh under concurrent merchant sessions
- Settlement calculation under load

## Conclusion

The Collection Engine met its throughput target under a 6-hour sustained load test across 62
simulated merchants and 400K+ transactions, with error rates well within acceptable limits
(0.001%). The primary scaling consideration going forward is infrastructure-level (queue memory)
rather than application logic.

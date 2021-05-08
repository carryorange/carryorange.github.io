# Back-Of-The-Envelope Estimation

## Numbers to keep in mind
### Power of two
* $2^{10} = 1024 \approx 10^3$
* $2^{20} = 1,048,576 \approx 10^6$
* $2^{30} \approx 10^9$, $2^{31} \approx 2 \times10^9$ (32-bit integer bound)
* $2^{63} \approx 9 \times 10^{18}$ (64-bit integer bound)
<br>
<br>

### Latency numbers
|           Operation name             | Time |
| ------------------------------------ | ---- |
| Mutex lock/unlock                    | 100ns|
| Main memory reference                | 100ns|
| Read 1 MB sequentially from memory   | 250us|
| Disk seek                            | 10ms |
| Read 1 MB sequentially from network  | 10ms |
| Read 1 MB sequentially from disk     | 30ms |
<br>
<br>

### Availability numbers
| Avaiability % | Downtime per day | Downtown per year |
| ------------- | ---------------- | ----------------- |
|    99%        |   14.4 minutes   |     3.65 days     |
|   99.9%       |   1.44 minutes   |     8.77 hours    |
|   99.99%      |   8.64 seconds   |    52.60 minutes  |
|  99.999%      |   863 ms         |    5.26 minutes   |
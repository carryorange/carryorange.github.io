# The Trouble with Distributed Systems
__Designing Data-Intensive Applications__ Chapter 8 and miscellaneous notes

## Unreliable Networks
* Key takeaway:
  * Network issue can happen any time
    * Packet may be lost (never reaching target machine)
    * Target machine may not process the packet
    * Response may be lost
  * Unbounded delay network
  * Hard to detect failure
    * Estimated timeout as an indication of failure
    * Use adaptive algorithms such as _Phi Accrual Failure Detection_
  
## Unreliable Clocks
### Two Kinds of Clocks
* Time-of-day clocks
  * Sync with NTP (Network Time Protocol)
    * PTP (Precision Time Protocol) for accuracy
  * Suitable for getting __point in time__
  * Oddities: may jump backwards / ahead during syncing
* Monotonic clocks
  * Suitable for getting __duration__
  * Guarantee to move forward
  * Can't compare monotonic clocks across computers
  * There may be a timer per CPU that are NOT synchronized

### Clock Synchronization and Accuracy
* Issues
  * Quartz clock in a computer is NOT accurate
    * 6 ms drift/30 seconds, or 17 seconds drift/day
  * Syncing with NTP server causes sudden jumps forward or backward
  * NTP synchronization is subject to network delay
  * Leap seconds may not be handled
* Key takeaway
  * __Do not rely on synchronized clocks__

## Process Pauses

## Knowledge, Truth, and Lies
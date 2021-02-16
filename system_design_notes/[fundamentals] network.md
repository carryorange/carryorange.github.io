# Notes on Networking

## IP (Internet Protocol)
### [IPv4](https://en.wikipedia.org/wiki/IPv4)
* A connectionless protocol, operating on a best-effort delivery model

## [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)
* TCP functionality
  * A reliable stream deliver service which guarantees the bytes sent and received will be
    * identical
      * package properly checksumed
    * in the same order
      * package has sequence number for ordering
  * Flow control
    * Limits the rate a sender tranfers data to guarantee reliable delivery
  * [Congestion control](https://en.wikipedia.org/wiki/TCP_congestion_control)

* Why three-way handshake
  * TCP is a bi-directional connection. Each party needs to set up an _initial sequence number (ISN)_
    * Alice --> Bob: `SYN` with Alice's ISN [$ISN_A$]
    * Alice <-- Bob: `ACK` that Bob's ready for [$ISN_A+1$]
    * Alice <-- Bob: `SYN` with Bob's ISN [$ISN_B$]
    * Alice --> Bob: `ACK` that Alice's ready for [$ISN_B+1$]
    
    The second and third steps are combined in one `SYN/ACK` (by setting both bits in the same packet)
  * Three-way handshake allows **both** parties to establish an ISN, thus guarantees reliable **bi-directional** communication.

* TCP termination - four-way handshake
  * Alice --> Bob: `FIN`
  * Alice <-- Bob: `ACK`
  * Alice <-- Bob: `FIN`
  * Alice --> Bob: `ACK`

  Each side of the connection terminates independently (hence four-way handshake)

## [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)

## [Routing Algorithms](https://www.geeksforgeeks.org/fixed-and-flooding-routing-algorithms/)
* Fixed routing
  * through a fixed routing table that stays the same unless network topology changes
* Flooding
  * each incoming packet to a node is sent out on every outgoing (except the one from which the packet is sent)
  * a lot of duplicate packets
* [Open Shortest Path First (OSPF)](https://en.wikipedia.org/wiki/Open_Shortest_Path_First)
  * a kind of interior gateway protocols (IGP)
* [Border Gateway Protocol (BGP)](https://en.wikipedia.org/wiki/Border_Gateway_Protocol)
  * a kind of exterior gateway protocal

## What happens when you type an URL in the browser and press enter

### [Reference](https://medium.com/@maneesha.wijesinghe1/what-happens-when-you-type-an-url-in-the-browser-and-press-enter-bb0aa2449c1a)

### Outline
1. DNS resolution
   * Browswer cache
   * OS cache
     * e.g. On mac, use the command
        ```bash
        dscacheutil -q host -a name google.com
        ```
   * Router cache
   * ISP cache
   * ISP's DNS server initiates a DNS query

2. The browswer initiates a TCP connection with the server
   * TCP/IP three-way handshake
     1. Client machine sends a `SYN` packet
     2. Server responds a `SYN/ACK` packet (if it allows a connection)
     3. Client acknowledge by sending an `ACK` packet.

3. The browswer sends an HTTP request to the server

4. The server handles the request and sends back an HTTP response

5. Browswer parses and renders the HTML content (or initiates other actions based on HTTP response)
   
<br><br>

### Side notes
#### Domain architecture
* Root domain: "."
* Top-level domains: e.g. edu, org, gov, com, au, cn
* Second-level domains: e.g. google.com, us.gov
* Third-level domains: e.g. maps.google.com

#### [Exponential backof](https://en.wikipedia.org/wiki/Exponential_backoff)
* An algorithm used to space out repeated retransmissions of the same block of data to avoid network congestion
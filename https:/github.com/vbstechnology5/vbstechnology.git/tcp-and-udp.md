---
description: Layer 4
---

# TCP & UDP

TCP (Transmission Control Protocol) and UDP (User Datagram Protocol) are two widely used transport layer protocols in computer networking. They provide communication services at the transport layer of the Internet Protocol (IP) suite. Here's an overview of each:

#### TCP (Transmission Control Protocol):

1. **Connection-Oriented:**
   * TCP is connection-oriented, meaning it establishes a reliable connection between two devices before data exchange.
   * It ensures that data is delivered in order and without errors.
   * The connection setup involves a three-way handshake (SYN, SYN-ACK, ACK) between the sender and receiver.
2. **Reliability and Flow Control:**
   * TCP provides error checking and correction mechanisms. If data packets are lost or corrupted during transmission, TCP will retransmit them.
   * Flow control mechanisms prevent the sender from overwhelming the receiver with too much data too quickly, preventing congestion.
3. **Ordered Delivery:**
   * Data sent over a TCP connection is received by the receiver in the same order it was sent by the sender.
4. **Full Duplex Communication:**
   * TCP supports full-duplex communication, allowing data to be sent and received simultaneously.
5. **Examples of Applications:**
   * Web browsing (HTTP)
   * File transfer (FTP)
   * Email (SMTP, POP3)
   * Remote login (SSH)
   * Real-time applications (Voice over IP, video streaming)

#### UDP (User Datagram Protocol):

1. **Connectionless:**
   * UDP is connectionless, meaning it does not establish a dedicated connection before sending data.
   * It's a "fire-and-forget" protocol, providing no guarantee of delivery.
2. **No Reliability or Flow Control:**
   * UDP does not guarantee delivery, and there is no mechanism for error recovery or retransmission.
   * There's also no flow control, so the sender can transmit data at its maximum rate.
3. **Unordered Delivery:**
   * UDP does not ensure the order of data delivery. Packets may arrive out of order.
4. **Low Overhead:**
   * UDP has lower overhead compared to TCP since it lacks the connection setup, error recovery, and flow control mechanisms.
5. **Broadcast and Multicast Support:**
   * UDP supports broadcasting, allowing a single packet to be sent to all devices on a network.
   * It also supports multicasting for sending data to a select group of devices.
6. **Examples of Applications:**
   * DNS (Domain Name System)
   * DHCP (Dynamic Host Configuration Protocol)
   * Streaming media (e.g., video and audio streaming)
   * Online gaming (for real-time communication)
   * SNMP (Simple Network Management Protocol)

#### Choosing Between TCP and UDP:

* **Use TCP when:**
  * Reliability and data integrity are crucial.
  * Data must be delivered in the correct order.
    * **Example:** File Transfer
    * **Reasoning:** When transferring a file, you want to ensure that every bit of data arrives intact. TCP's error-checking and retransmission mechanisms help guarantee data integrity.
  * Connection setup and teardown overhead is acceptable.
* **Use UDP when:**
  * Low-latency communication is essential.
    * **Example**: Gaming
    * &#x20;**Reasoning:** Especially real-time multiplayer games, receiving information in a specific order is indeed crucial for maintaining game state consistency. While UDP is connectionless and doesn't guarantee ordered delivery like TCP, game developers implement their own mechanisms to handle this.
  * Loss of some data is acceptable (e.g., real-time multimedia).
    * **Example:** Video Streaming (e.g., YouTube)
    * **Reasoning:** In real-time multimedia streaming, such as video or audio streaming, losing a few packets occasionally may not be noticeable to users. UDP's speed is prioritized over guaranteed delivery.
  * Minimizing protocol overhead is a priority.
    * Minimizing protocol overhead refers to the practice of reducing the additional data and processing required by a communication protocol beyond the essential information needed for the communication itself. In networking, protocol overhead includes any extra bytes or computational work introduced by the communication protocol.

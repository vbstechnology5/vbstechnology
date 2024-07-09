---
description: Layer 2 Datalink
---

# MAC Adresses

The Data Link Layer is responsible for addressing and framing data for transmission. It ensures that data is error-free and in the correct order, typically through techniques like MAC (Media Access Control) addresses and error detection/correction.

Let's break down the key responsibilities of the Data Link Layer:

1. **Addressing:**
   *   **MAC Addresses (Media Access Control):** Each device connected to a network at the Data Link Layer is assigned a unique identifier known as a MAC address. This address is burned into the network interface card (NIC) and is used to identify the source and destination of data frames on the local network.

       Example:

       * MAC Address: `00:1A:2B:3C:4D:5E`
2. **Framing:**
   * **Frame Construction:** Data is broken down into frames for transmission. A frame includes the actual data along with control information such as MAC addresses, frame type, and error-checking information.
   * Research the above part a bit more because honestly I have no idea what the fuck it means.
3. **Error Detection and Correction:**
   *   **Checksums and CRC (Cyclic Redundancy Check):** The Data Link Layer uses techniques like checksums or CRC to detect errors in the transmitted data. If errors are detected, the frame is often retransmitted.

       Example:

       * A checksum is calculated based on the frame data, and the result is sent with the frame. The recipient recalculates the checksum upon receiving the frame. If the recalculated checksum doesn't match the received checksum, an error is detected.

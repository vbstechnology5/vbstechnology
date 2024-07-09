---
description: These are the 7 OSI layers
---

# OSI Model

## OSI Model

The OSI (Open Systems Interconnection) model is a conceptual framework used to understand and standardize how different networking protocols and technologies interact within a networked environment. It consists of seven layers, each of which represents a specific function or set of functions involved in network communication. The OSI model is a reference model, and it helps in the design, analysis, and troubleshooting of network communication systems. Here are the seven layers of the OSI model, from the lowest (Layer 1) to the highest (Layer 7):

* **Physical Layer (Layer 1)**:
  * This layer deals with the physical medium and transmission of raw bits over a physical link. It includes specifications for cables, connectors, switches, and other hardware components. It is primarily concerned with the electrical and mechanical characteristics of the transmission medium.
    * Protocols: Ethernet, USB, Bluetooth
* **Data Link Layer (Layer 2)**:
  * The Data Link Layer is responsible for addressing and framing data for transmission. It ensures that data is error-free and in the correct order, typically through techniques like MAC (Media Access Control) addresses and error detection/correction.
* **Network Layer (Layer 3)**: (IP addresses)
  * The Network Layer is responsible for routing packets of data from the source to the destination across multiple networks. It handles addressing, packet forwarding, and routing.
* **Transport Layer (Layer 4)**:
  * The Transport Layer manages end-to-end communication and data segmentation. It establishes, maintains, and terminates connections, and it ensures the reliable and ordered delivery of data.&#x20;
    * Protocols: TCP (Transmission Control Protocol) and UDP (User Datagram Protocol).
* **Session Layer (Layer 5)**:
  * The Session Layer manages the establishment, maintenance, and termination of sessions or connections between applications. It is responsible for synchronization, checkpointing, and recovery of data.
* **Presentation Layer (Layer 6)**:
  * The Presentation Layer is responsible for data translation, encryption, and compression. It ensures that data is in a format that can be understood by the application layer. Functions like data encryption and character encoding take place here.&#x20;
    * Protocols: SSL/TLS (Secure Sockets Layer/Transport Layer Security)
* **Application Layer (Layer 7)**:
  * The Application Layer is the top layer and interacts directly with end-user applications and services. It provides a platform-independent interface between the application and the lower layers.&#x20;
    * Protocols: HTTP (Hypertext Transfer Protocol), FTP (File Transfer Protocol), SMTP (Simple Mail Transfer Protocol), DNS (Domain Name System)

The OSI model is a valuable framework for understanding how network communication works because it allows for a clear separation of concerns and responsibilities at each layer. However, it's essential to note that in practice, real-world networking protocols and technologies don't always neatly align with the OSI model. For example, the TCP/IP model, which is used for the internet, is a more practical and widely adopted model with fewer layers, and it combines elements from the OSI model.

# SSL Certificates

SSL certificates (or more accurately, TLS certificates) are digital certificates that provide secure, encrypted communication over a computer network, most commonly the internet. TLS (Transport Layer Security) is the successor to SSL (Secure Sockets Layer), but the term "SSL certificate" is still widely used.

### **Why Certificates?**

In the context of web security, certificates, specifically SSL/TLS certificates, play a crucial role in securing the transmission of data between a user's browser and a web server. They ensure that the communication is encrypted, preventing unauthorized access, eavesdropping, and tampering.

#### Note

SSL (Secure Sockets Layer) and TLS (Transport Layer Security) are related cryptographic protocols that provide secure communication over a computer network. While they share similar goals and concepts, they are not exactly the same thing. TLS is essentially the successor to SSL.

Here's a brief overview:

1. **SSL (Secure Sockets Layer):** SSL was the original protocol developed by Netscape in the 1990s to secure online communication. However, as vulnerabilities were discovered and the protocol evolved, it was succeeded by TLS.
2. **TLS (Transport Layer Security):** TLS is the modern and more secure version of SSL. The development of TLS was initiated to address the security flaws found in SSL. TLS has undergone several versions, with each new version introducing improvements in security and functionality.
3. **SSL Certificates vs. TLS Certificates:** The term "SSL certificate" is commonly used, but it often refers to certificates that are used in the broader context of TLS. In practice, when people say "SSL certificate," they are often referring to a certificate that can be used with both SSL and TLS protocols.

### Overview of SSL/TLS

1. **Encryption and Authentication:** SSL/TLS certificates serve two primary purposes. First, they enable encryption of data transmitted between a user's web browser and a website's server. This encryption helps protect sensitive information such as login credentials, personal details, and financial transactions.
2. **Authentication:** The second purpose is authentication. SSL/TLS certificates verify the identity of the website or server to which the user is connecting. This helps users trust that they are interacting with the legitimate website and not a malicious imposter.
3. **Digital Signatures:** SSL/TLS certificates include a digital signature, which is a cryptographic mechanism that ensures the integrity of the certificate. If the certificate is modified in any way, the signature becomes invalid, alerting users to potential security risks.
4. **Certificate Authorities (CAs):** SSL/TLS certificates are issued by Certificate Authorities, which are trusted entities responsible for verifying the legitimacy of entities (such as websites) requesting certificates. Popular CAs include Let's Encrypt, DigiCert, and Comodo.
5. **Common SSL/TLS Certificate Types:**
   * **Domain Validated (DV) Certificates:** Verify that the entity requesting the certificate has control over the domain.
   * **Organization Validated (OV) Certificates:** Include additional verification of the organization's identity.
   * **Extended Validation (EV) Certificates:** Provide the highest level of validation, displaying the organization's name in the browser's address bar.
6. **HTTPS Protocol:** SSL/TLS certificates are integral to the implementation of HTTPS (Hypertext Transfer Protocol Secure), the secure version of HTTP. Websites with HTTPS in their URLs use SSL/TLS certificates to encrypt and authenticate data exchanged with users.

### How SSL certificates work? (Under the hood)

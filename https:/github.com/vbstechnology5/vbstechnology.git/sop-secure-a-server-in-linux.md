# SOP Secure a Server in Linux

## <mark style="color:red;">LAL NGO:</mark>

### How to secure a server from A to Z:

I started by securing a ubuntu server threw my kali linux machine .I opened an SSH session to remote control the server threw root in terminal.Use putty or wcs or terminal ssh to connect to the server.

### Steps are in the following:

<mark style="color:blue;">**1)**</mark>[<mark style="color:blue;">**Setup and updates:**</mark>](sop-secure-a-server-in-linux.md#id-1-setup-and-updates)

<mark style="color:blue;">**2)**</mark>[<mark style="color:blue;">**Configure and secure SSH(configure fail2ban)**</mark>](sop-secure-a-server-in-linux.md#id-2-configure-and-secure-ssh)

<mark style="color:blue;">**3)**</mark>[<mark style="color:blue;">**Firewall setup:UFW rules etc…**</mark>](sop-secure-a-server-in-linux.md#id-3-firewall-setup-ufw-rules-etc)

<mark style="color:blue;">**4)**</mark>[<mark style="color:blue;">**Monitoring and audit**</mark>](sop-secure-a-server-in-linux.md#id-4-monitoring-and-audit)

<mark style="color:blue;">**5)**</mark>[<mark style="color:blue;">**Secure Shared Memory**</mark>](sop-secure-a-server-in-linux.md#id-5-secure-shared-memory)

<mark style="color:blue;">**6)**</mark>[<mark style="color:blue;">**Harden Network Security**</mark>](sop-secure-a-server-in-linux.md#id-6-harden-network-security)

<mark style="color:blue;">**7)**</mark>[<mark style="color:blue;">**Protect Against SYN Flood Attacks**</mark>](sop-secure-a-server-in-linux.md#id-7-protect-against-syn-flood-attacks)

<mark style="color:blue;">**8)**</mark>[<mark style="color:blue;">**Configure AppArmor**</mark>](sop-secure-a-server-in-linux.md#id-8-configure-apparmor)

<mark style="color:blue;">**9)**</mark>[<mark style="color:blue;">**Secure Apache from DDOS & DOS attacks**</mark>](sop-secure-a-server-in-linux.md#id-9-secure-apache)

<mark style="color:blue;">**10)**</mark>[<mark style="color:blue;">**Secure My SQL from sql injection attacks**</mark>](sop-secure-a-server-in-linux.md#id-10-secure-my-sql)

<mark style="color:blue;">**11)**</mark>[<mark style="color:blue;">**Regular Backups (using crontab & rsync)**</mark>](sop-secure-a-server-in-linux.md#id-11-regular-backups)

<mark style="color:red;">**12)**</mark>**Additional questions**

&#x20;

## <mark style="color:red;">1)Setup and updates:</mark>

### <mark style="color:blue;">Using a command to update the server ,Disable root login,Reload SSH service,use:</mark>

`sudo su`

`apt update && upgrade` : to update the system

`sudo apt upgrade -y` : also the

`sudo apt autoremove -y`  : to remove unnecessary packages

Commands to upload everything to latest version

### <mark style="color:blue;">Create a new user:Avoid using the</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**root**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">account for regular management.</mark>

`sudo adduser newuser`

`sudo usermod -aG sudo newuser`

## &#x20;<mark style="background-color:red;">Q & A:</mark>

### <mark style="background-color:green;">Question:</mark>

Is there some sort of tool (or solution) to scan the server to determine whether there are some problematic directories? (for example, directories that have 777 permissions that we have forgotten about), or for example certain files that are owned by "root". We want to make sure we are following the security best practices when it comes to managing a server.

### &#x20;<mark style="background-color:green;">Answer:</mark>

First : Directories with 777 permissions are those where anyone (the owner, the group, and others) has read, write, and execute permissions.

Yes there is various tools we can use to check if my server follows security practices especially of these types of directories with 777 permissions.Many tools and methods are available for scanning server security but I will talk about lynis because we used it in Step **number 4** [audit & monitoring](sop-secure-a-server-in-linux.md#regular-security-audits).

#### **Lynis:**

* **Description**: Lynis is a comprehensive security auditing tool for Unix-based systems. It performs a detailed scan of your system, checking for security vulnerabilities, configuration issues, and best practices.
* **How it Helps**: Lynis can detect a variety of security problems, including directories with overly permissive permissions (e.g., 777) and files that may pose a security risk due to their ownership or permissions.

<mark style="color:blue;">**Create a script to find directories with 777 Directories using Lynis:**</mark>

```typescript
echo "Checking for directories with 777 permissions..."
find / -type d -perm 777 2>/dev/null > /tmp/777_dirs.txt
echo "Directories with 777 permissions have been saved to /tmp/777_dirs.txt"
```

<mark style="color:blue;">**Integrate this script with Lynis by adding a custom test:**</mark>

`sudo nano /etc/lynis/custom.prf`

<mark style="color:blue;">**Add the following lines to custom.prf**</mark>

`#Custom test for finding 777 directories`\
&#x20;    `test_dirs_777() {` \
&#x20;    `echo "Checking for directories with 777 permissions..."`\
&#x20;    `find / -type d -perm 777 2>/dev/null > /tmp/777_dirs.txt`\
&#x20;    `if [ -s /tmp/777_dirs.txt ]; then`\
&#x20;         `echo "Warning: Found directories with 777 permissions!"`\
&#x20;         `cat /tmp/777_dirs.txt`\
&#x20;    `else`\
&#x20;         `echo "No directories with 777 permissions found."`\
&#x20;`fi }`\
`#Run the custom test`\
`test_dirs_777`

<mark style="color:blue;">**Now run the following command and you're done:**</mark>

`sudo lynis audit system --profile /etc/lynis/custom.prf`

Also we can setup <mark style="color:green;">Logwatch</mark> to monitor <mark style="color:green;">serverlogs</mark>

## <mark style="color:red;">2)Configure and secure SSH:</mark>

### <mark style="color:blue;">Install SSH</mark>

`sudo apt install openssh-server`

[<mark style="color:purple;">https://hostman.com/tutorials/how-to-install-and-configure-ssh-on-ubuntu-22-04/#step-2--install-ssh-on-ubuntu</mark>](https://hostman.com/tutorials/how-to-install-and-configure-ssh-on-ubuntu-22-04/#step-2--install-ssh-on-ubuntu)

### <mark style="color:blue;">Configure SSH</mark>

Disable root login and change the default SSH port to enhance security:

`sudo nano /etc/ssh/sshd_config`

### <mark style="color:blue;">Modify or add the following lines:</mark>

`Port 2200` or any port you would like to change to

`PermitRootLogin no`

### <mark style="color:blue;">Reload the SSH service:</mark>

`sudo systemctl reload ssh` Secure SSH Access

### <mark style="color:blue;">Now to prevent a disconnection from your ssh session do the following:</mark>

Add user another than root and give me permissions for sudo su to change some things:

`sudo usermod -aG sudo username` (change to your username)\
Verify the user has sudo priveleges :

&#x20;`su - username`

&#x20;`sudo whoami`

&#x20;Note:check ufw status to see which ports firewall is accessing and use ufw enable ssh to enable remote access

1. <mark style="color:green;">Use Key-Based Authentication</mark>
2.  <mark style="color:green;">Generate a key using putty gen or using a command(Public/Private)</mark>

    <mark style="color:green;">Generate a key pair on your local machine:</mark>

    * `ssh-keygen -t rsa -b 4096`  For example he gave me a private/public key called SHA256

### <mark style="color:blue;">Copy the public key to the server:</mark>

`ssh-copy-id -i ~/.ssh/id_rsa.pub newuser@your_server_ip`

* <mark style="color:green;">Disable password authentication by editing using</mark> `nano /etc/ssh/sshd_config add this:`

`PasswordAuthentication no`

* <mark style="color:green;">Reload SSH service again :</mark>  `sudo systemctl reload ssh`

### <mark style="color:blue;">Using SSH: Connecting to a Server</mark>

To connect to a remote server using SSH, you can use an SSH client like OpenSSH, which is typically installed on most Linux and macOS systems by default. Windows users can use tools like PuTTY or the built-in SSH client in Windows 10 and later.

Example command to connect  :`ssh username@remote_host`

Here, username is the user account on the remote machine, and remote\_host is the IP address or hostname of the remote server.

2. <mark style="color:green;">Install</mark> <mark style="color:green;"></mark><mark style="color:green;">**Fail2Ban**</mark>

### <mark style="color:blue;">Protect against brute-force attacks:</mark>

It is an open-source intrusion prevention software framework that protects computer servers from brute-force attacks. Fail2ban works by monitoring log files:Update fw rules.

`sudo apt install fail2ban`

`sudo systemctl enable fail2ban`

`sudo systemctl start fail2ban`

* Configure Fail2Ban as needed by editing `/etc/fail2ban/jail.local`
* Check what to configure on Google!

3\.   <mark style="color:green;">To display a list of IP addresses currently banned by fail2ban, type the following command:</mark>

`copyiptables -S`

4\. <mark style="color:green;">For example, the following line shows an IP address that the</mark> <mark style="color:green;"></mark><mark style="color:green;">**SSH jail**</mark> <mark style="color:green;"></mark><mark style="color:green;">has banned:</mark>

`-A fail2ban-SSH -s 10.0.1.124/32 -j REJECT --reject-with icmp-port-unreachable`

### <mark style="color:blue;">Advanced SSH Features</mark>

1. <mark style="color:green;">Port Forwarding/Tunneling:</mark>
2. <mark style="color:green;">Local Port Forwarding:</mark>

`ssh -L local_port:destination_host:destination_port username@remote_host`

This command forwards traffic from local\_port on the client machine to destination\_port on destination\_host through remote\_host.

* <mark style="color:green;">Remote Port Forwarding:</mark>

`ssh -R remote_port:local_host:local_port username@remote_host`

This forwards traffic from remote\_port on the server to local\_port on the client machine.

### <mark style="color:blue;">Secure File Transfer:</mark>

* <mark style="color:green;">SCP:</mark>

`scp local_file username@remote_host:/remote/directory`

This command securely copies a file to the remote server.

* <mark style="color:green;">SFTP:</mark>

`sftp username@remote_host`

## &#x20;<mark style="background-color:red;">Q</mark> <mark style="background-color:red;"></mark><mark style="background-color:red;">& A:</mark>

### <mark style="background-color:green;">Question:</mark>

What are the implications of this?

Is it possible that this can cause us to get "locked" out of the server in some way? Example:

* A server has 3 users: root, zakaria and rami.
* rami and zakaria have limited permissions (for example, they can't move files from one directory to another or they can't create files in certain directories)
* rami ssh'd into the server and disabled root login.
* What happens then if rami or zakaria will need to do an action that they do not have permission to?

### <mark style="background-color:green;">Answer:</mark>

<mark style="color:red;">Disabling root have many Implications on the server:</mark>

Disabling root will reduce the risk of getting **bruteforce** attacks because normally hackers attack root user because it’s a known name and they don’t know other usernames so what we can do to prevent this type of attacks is to disable root and create a user and give him some permissions so when they type sudo su in their user they can perform only some administrative tasks based on their **permissions**.

<mark style="color:red;">Sometimes you will be locked out of the server why and when?</mark>

**First** when the sudo configuration (while editing ssh config)is incorrect or if no users have sudo access,we will face many problem especially in management.

**Second,** when we make a user permission for example if I gave to rami some permissions and I want him to remote the server using ssh.While configuring the SSH we will generate an SSH key (public and private) for rami so if he is locked from the server he can use a command using the SSH private key reenter the server (<mark style="color:red;">Note:</mark><mark style="color:green;">Dont’t forget to save the Key in a txt file on the server and on an external disk or my local machine so it will never get lost</mark>)

**Third ,**It is important to give just one or two users sudo privileges for necessary administrative functions and thats a part of management to make user’s **permissions** depending on the **user’s tasks**.

<mark style="color:red;">If Rami or Zakaria need to perform an action that they do not have permission to and root login is disabled:we have many options:</mark>

.If both users don’t have sudo access <mark style="color:green;">**they cant**</mark> make any action.

.If both users have limited sudo access,they can only access <mark style="color:green;">**some administrative**</mark> tasks that are given by the admin

So to avoid being prohibited to make any action the <mark style="color:red;">**third option**</mark> is the best:

.Before disabling the root in the configuration we will give just one user for example zakaria **FULL sudo access** so this user called zakaria will be able to manage anything by using sudo su.

We have another option but <mark style="color:red;">**it’s not very recommend**</mark> (in case we forgot to put the third option)is to make a <mark style="color:green;">fallback plan</mark> :we create a documented procedure to temporary re-enable root login if necessary

## <mark style="color:red;">3)Firewall setup:UFW rules etc…</mark>

1. <mark style="color:green;">Enable UFW (Uncomplicated Firewall)</mark>
2. <mark style="color:green;">Allow necessary ports and deny others:</mark>

`sudo ufw allow 2200/tcp`

`sudo ufw allow 80/tcp`

`sudo ufw allow 443/tcp`

`sudo ufw enable`

### <mark style="color:blue;">Secure Network Services</mark>

1. <mark style="color:green;">Disable Unused Services</mark>

* List all services:

`sudo systemctl list-unit-files --type=service`

* Disable services not in use:

`sudo systemctl disable service_name`

`sudo systemctl stop service_name`

2. <mark style="color:green;">Use Strong Passwords</mark>

* Ensure all users have strong, unique passwords.
* Consider using a password manager to generate and store passwords securely.

### <mark style="color:blue;">The default firewall configuration tool for Ubuntu is UFW (Uncomplicated Firewall)</mark>

1. <mark style="color:green;">Enable UFW or disable</mark>

`sudo ufw enable/disable`

`sudo ufw status(check ufw status)`

`sudo ufw reset(reset settings ufw)`

## <mark style="color:red;">4)Monitoring and audit:</mark>

Is a Continuous observation and analysis of systems, networks, and applications to ensure they are operating correctly, securely, and efficiently

### <mark style="color:blue;">Setup Log Monitoring</mark>

Logwatch is a log analysis and reporting tool for Linux and Unix-based systems. It is designed to parse and analyze log files generated by the system and various services, summarizing the information into a concise report that is typically emailed to the system administrator.

* <mark style="color:green;">Use tools like Logwatch to monitor server logs:</mark>

`sudo apt install logwatch`

`sudo logwatch --output mail --mailto your_email@example.com --detail high`

### <mark style="color:blue;">Regular Security Audits</mark>

Lynis is an open-source security auditing tool designed for Unix-based systems, including Linux, macOS, and other Unix derivatives. It performs in-depth security scans of the operating system, software packages, and configuration files to identify potential security issues and vulnerabilities

* <mark style="color:green;">Periodically</mark> <mark style="color:green;"></mark><mark style="color:green;">**audit**</mark> <mark style="color:green;"></mark><mark style="color:green;">your server’s security settings and configurations.</mark>
* <mark style="color:green;">Tools like</mark> <mark style="color:green;"></mark><mark style="color:green;">**Lynis**</mark> <mark style="color:green;"></mark><mark style="color:green;">can help with comprehensive security audits:</mark>

`sudo apt install lynis`

`sudo lynis audit system` (to install the system)

### <mark style="color:blue;">Install Intrusion Detection System (IDS)</mark>

* <mark style="color:green;">Tools like AIDE (Advanced Intrusion Detection Environment) can help detect changes to the filesystem:</mark>

`sudo apt install aide`

`sudo aideinit`

`sudo cp /var/lib/aide/aide.db.new /var/lib/aide/aide.db`

`sudo aide --check`

## <mark style="background-color:red;">Q & A:</mark>

### <mark style="background-color:green;">Question:</mark>

In order to set up a <mark style="color:blue;">**monitoring or observability tool**</mark> (such as **grafana**), how can we do so “securely” if we have disabled remote access to the database of a server? (since Grafana will require to read some data from the database remotely, since it is a cloud app.)

### <mark style="background-color:green;">Answer:</mark>

<mark style="color:blue;">**Grafana Cloud**</mark> is a management platform that provides monitoring, logging, and visualization capabilities. It integrates several powerful open-source tools, such as Grafana for dashboards, Prometheus for metrics, and Loki for logs, into a single cloud-based service.&#x20;

Using <mark style="color:blue;">**Prometheus**</mark> while setting up Grafana offers several benefits that enhance the overall monitoring and visualization capabilities of your system.

Prometheus excels at collecting, storing, and querying time-series data, while Grafana offers powerful visualization capabilities. Together, they enable real-time monitoring, advanced querying

First we configured and created an account on grafana cloud and did d subdomain called [https://lalngo.grafana.net/](https://lalngo.grafana.net/) . We chose to connect our linux server first(cpu usage,data consump etc...)

<figure><img src=".gitbook/assets/Screenshot 2024-06-03 130818.png" alt="" width="375"><figcaption></figcaption></figure>

### <mark style="color:green;">Configuring Alloy:</mark>

<mark style="color:yellow;">Alloy</mark> is made to analyse system softwares and to make the data link between the ubuntu server and Grafana you can set it up using this guide:

[<mark style="background-color:blue;">https://grafana.com/docs/alloy/latest/get-started/install/linux/</mark>](https://grafana.com/docs/alloy/latest/get-started/install/linux/)

We made a new <mark style="color:yellow;">**Collection**</mark>, generated an API key and pasted the code in the command line on my server ( created a dashboard and the following mustbe shown)

<figure><img src=".gitbook/assets/Screenshot 2024-06-03 131121.png" alt=""><figcaption><p>Linux Server Dashboard</p></figcaption></figure>

We can do many other things using Grafana like log monitoring,security alerts,data management etc...&#x20;

Now if we need to collect for example SSH <mark style="color:yellow;">**authentication failures**</mark>

<mark style="color:yellow;">**Prometheus :**</mark>** **<mark style="color:blue;">**Collect SSH Authentication Failure Logs:**</mark> First, you need to collect SSH authentication failure logs from your Linux server. These logs are usually stored in `/var/log/auth.log` or `/var/log/secure`, depending on your Linux distribution.

* <mark style="color:green;">**Logs and Extract Metrics:**</mark> Use a log parsing tool like Logstash, **Prometheus's** log exporter to parse the SSH authentication failure logs and extract relevant metrics. You can extract metrics such as the number of failed login attempts, the IP addresses of the failed login attempts, etc.
* <mark style="color:green;">**Send Metrics to Prometheus:**</mark> Send the extracted metrics to Prometheus, a monitoring and alerting toolkit commonly used with Grafana.&#x20;
* <mark style="color:green;">**Set Up Grafana Dashboard:**</mark> Create a Grafana dashboard to visualize the SSH authentication failure metrics. Use Prometheus .
* <mark style="color:green;">**Create Alerts (Optional):**</mark> If you want to be notified when SSH authentication failures exceed a certain threshold, you can set up alerts in Grafana.&#x20;

[<mark style="background-color:blue;">https://prometheus.io/docs/prometheus/latest/installation/</mark>](https://prometheus.io/docs/prometheus/latest/installation/)

[<mark style="background-color:blue;">https://grafana.com/docs/agent/latest/flow/tutorials/collecting-prometheus-metrics/</mark>](https://grafana.com/docs/agent/latest/flow/tutorials/collecting-prometheus-metrics/)

*   **Collect SSH Authentication Failure Logs:** Ensure that your Linux server is logging SSH authentication failures. Typically, these failures are logged in `/var/log/auth.log` or `/var/log/secure`. You can verify this by checking these log files:

    ```bash
    tail /var/log/auth.log
    tail /var/log/secure
    ```
*   **Install and Configure Prometheus:** Install Prometheus on your server. You can download the latest version from the Prometheus website or use a package manager if your distribution provides one. Once installed, configure Prometheus to scrape SSH authentication failure metrics. Add the following configuration to your `prometheus.yml` file:

    ```yaml
    scrape_configs:
      - job_name: 'ssh'
        static_configs:
          - targets: ['localhost:9116']
    ```

    This assumes that you'll be using an exporter to expose SSH authentication failure metrics on port 9116.
* **Install and Configure SSH Exporter:** Install the Prometheus SSH Exporter, which collects SSH login failures and exposes them as Prometheus metrics. You can find the exporter on GitHub: [https://github.com/mintel/ssh\_exporter](https://github.com/mintel/ssh\_exporter). Follow the instructions in the README to build and install the exporter on your server.
*   **Start Prometheus and SSH Exporter:** Start both Prometheus and the SSH Exporter services:

    ```bash
    sudo systemctl start prometheus
    sudo systemctl start ssh_exporter
    ```
*   **Verify Metrics in Prometheus:** Access Prometheus web interface (usually at `http://localhost:9090`) and verify that SSH authentication failure metrics are being scraped. You can run queries to verify metrics like `ssh_failed_logins_total`:

    ```
    ssh_failed_logins_total
    ```
*   **Install Grafana:** Install Grafana on your server. You can download the latest version from the Grafana website or use a package manager if available. Once installed, start the Grafana service:

    ```bash
    le codesudo systemctl start grafana-server
    ```

### <mark style="color:blue;">What if we have disabled remote access to the database of a server?</mark>

If we disable remote access to the database on our server, <mark style="color:green;">Grafana can still securely read data from my database</mark> by utilizing methods that don't require direct remote access (setup ssh tunnel or use proxy)

<mark style="color:green;">Grafana does not require root access to read data from your machine</mark>. Instead, it can be configured to read data from various sources in a secure manner. Here’s how you can set up Grafana to securely read data from your machine without needing root access:

### <mark style="color:blue;">Use a VPN or Secure Tunnel</mark>

* Establish a Virtual Private Network (VPN) connection between Grafana Cloud and your internal network where the database resides.
* Alternatively, set up an SSH tunnel from a machine with SSH access to the database.

1. <mark style="color:green;">**SSH Tunnel:**</mark>
   * Create an SSH tunnel on a machine within your network that has SSH access to the database.
   * Forward the required database ports through this tunnel.

### &#x20;<mark style="color:blue;">Use an API Gateway</mark>

<mark style="color:green;">**Concept:**</mark>

* Deploy an API gateway to securely expose specific database endpoints.
* The API gateway handles authentication and encryption, providing secure access to the database data.

1. <mark style="color:green;">**Deploy an API gateway:**</mark>
   * Set up an API gateway service (e.g., AWS API Gateway) that connects to your database.
   * Define the endpoints and operations (e.g., read, write) that the gateway will expose.
2. <mark style="color:green;">**Secure the API gateway:**</mark>
   * Configure the API gateway to use HTTPS for secure communication.
   * Implement authentication methods, such as API keys or OAuth tokens, to control access.

### &#x20;<mark style="color:blue;">Use a Database Proxy</mark>

**Concept:**

* A database proxy manages and secures connections to your database.
* The proxy can handle authentication, connection pooling, and encryption.

**Steps:**

1. **Deploy a database proxy:**
   * Use a database proxy service (e.g., AWS RDS Proxy, PgBouncer) to manage connections to your database.
2. **Configure the proxy:**
   * Set up the proxy to accept connections from Grafana Cloud.
   * Ensure the proxy manages authentication and encrypts the traffic between Grafana Cloud and the database.

## <mark style="color:red;">5) Secure Shared Memory</mark>

it can help prevent certain types of attacks and unauthorized access to sensitive data. Shared memory is a mechanism that allows multiple processes to access common memory space, and securing it involves restricting access to this shared resource.

1. <mark style="color:green;">Edit /etc/fstab</mark>

`sudo nano /etc/fstab`

2. <mark style="color:green;">Add the Following Line</mark>

`tmpfs /run/shm tmpfs defaults,noexec,nosuid 0 0`

3. <mark style="color:green;">Remount All File Systems</mark>

`sudo mount -a`

`ipcs -m` (list of shared memory)

### <mark style="color:blue;">Regularly Monitor and Audit Shared Memory Usage</mark>

Regular monitoring and auditing of shared memory usage can help detect and respond to suspicious activity.

<mark style="color:green;">Tools:</mark>

`Auditd` : Use the audit daemon to log access to shared memory segments.

<mark style="color:green;">Install auditd:</mark>

`sudo apt-get install auditd`

* <mark style="color:green;">Add audit rules to monitor shared memory usage</mark> (e.g., in /etc/audit/rules.d/audit.rules):

`sudo nano /etc/audit/rules.d/audit.rules`

[<mark style="color:green;">Add rules like:</mark>](#user-content-fn-1)[^1]

`-a always,exit -F arch=b64 -S shmget -S shmat -S shmdt -S shmctl -k shared_memory`

* <mark style="color:green;">Restart auditd:</mark>

`sudo systemctl restart auditd`

* <mark style="color:green;">View logs:</mark>

`sudo ausearch -k shared_memory`

## <mark style="color:red;">6)Harden Network Security</mark>

TCP Wrappers allow you to define rules for which hosts (IP addresses) are allowed or denied access to specific services.TCP Wrappers is a host-based network access control system used to filter network access to Internet services based on IP addresses, hostnames, or other criteria. It provides an additional layer of security by allowing or denying access to various network services running on a Unix

### <mark style="color:blue;">1.Disable Unused Network Services</mark>

* <mark style="color:green;">List all active services:</mark>
* <mark style="color:green;">If you don’t have netstat(apt install net-tools)</mark>

`sudo netstat -tulnp`

<mark style="color:green;">Disable unnecessary services:</mark>

`sudo systemctl disable service_name`

### <mark style="color:blue;">2.Install and Configure TCP Wrappers</mark>

`sudo apt install tcpd -y (to install it)`

* <mark style="color:green;">Edit /etc/hosts.allow to specify allowed connections:</mark>

`sudo nano /etc/hosts.allow`

add rule : `sshd : 192.168.1.100` (for example allow this id)

* <mark style="color:green;">Edit /etc/hosts.deny to deny all others:</mark>

`sudo nano /etc/hosts.deny`

Add rule: # Deny all other access `(ALL : ALL`) for example

&#x20;Restart if necessary : `sudo systemctl restart sshd`

## <mark style="color:red;">7) Protect Against SYN Flood Attacks:</mark>

SYN flood attacks are a type of Denial of Service (DoS) attack that exploits the TCP handshake process to overwhelm a target system, rendering it unable to process legitimate traffic

### <mark style="color:blue;">SYN Flood attacks setup</mark>

1. <mark style="color:green;">Edit sysctl.conf</mark>

`sudo nano /etc/sysctl.conf`

2. <mark style="color:green;">Add the Following Lines</mark>

`net.ipv4.tcp_syncookies = 1`

`net.ipv4.conf.all.rp_filter = 1`

`net.ipv4.conf.default.rp_filter = 1`

`net.ipv4.tcp_timestamps = 0`

`net.ipv4.icmp_echo_ignore_broadcasts = 1`

`net.ipv4.icmp_ignore_bogus_error_responses = 1`

`net.ipv4.tcp_fin_timeout = 15`

`net.ipv4.tcp_keepalive_time = 300`

&#x20;

`sudo sysctl -p`(to apply changes)

## <mark style="color:red;">8) Configure AppArmor:</mark>

AppArmor stands for "Application Armor". It is a Linux kernel security module that provides mandatory access control (MAC) security by restricting the capabilities of programs. AppArmor uses profiles, which are sets of rules defining what system resources (like files, capabilities, network access) an application can access, thereby reducing the potential damage from a compromised application.

Install: `sudo apt install apparmor apparmor-utils -y`

1. <mark style="color:blue;">Enable AppArmor Profiles</mark>

`sudo aa-enforce /etc/apparmor.d/*`

2. <mark style="color:blue;">Generate Profile for a specific app</mark>

`sudo aa-genprof /usr/bin/myapp`

<mark style="color:green;">Place the Profile in the AppArmor Directory:</mark>

Once generated, the profile will be placed in the `/etc/apparmor.d`

1. <mark style="color:green;">Edit the profile</mark>

`sudo nano /etc/apparmor.d/usr.bin.myapp`

Check here:

**https://ubuntu.com/tutorials/beginning-apparmor-profile-development#1-overview**

## <mark style="color:red;">9)Secure Apache:</mark>

Enabling <mark style="color:blue;">**mod\_security**</mark> and <mark style="color:blue;">**mod\_evasive**</mark> adds additional layers of security to your Apache web server by providing protection against various types of attacks and helping mitigate denial-of-service (DoS) attacks.

1. <mark style="color:blue;">Install Apache</mark>

`sudo apt install apache2 -y`

2. <mark style="color:blue;">Disable Directory Listing</mark>

* <mark style="color:green;">Edit the default Apache configuration:</mark>

`sudo nano /etc/apache2/apache2.conf`

* <mark style="color:green;">Add or modify the following:</mark>

`< Directory /var/www/html/moodle >`

`Options -Indexes`

`</Directory>`

By adding this you are disabling directory indexing for the /var/www/ directory, which is a common security measure to prevent unauthorized access to directory contents.

#### <mark style="color:blue;">3.Install mod security and enable it :</mark>

`sudo apt update`

`sudo apt-get install libapache2-mod-security2`

<mark style="color:green;">4.Enable</mark> <mark style="color:green;"></mark><mark style="color:green;">**mod\_security**</mark> <mark style="color:green;"></mark><mark style="color:green;">and</mark> <mark style="color:green;"></mark><mark style="color:green;">**mod\_evasive**</mark>

`sudo apt install libapache2-mod-security2 libapache2-mod-evasive -y`

`sudo a2enmod security2`

`sudo a2enmod evasive`

1. <mark style="color:blue;">**Configure mod\_evasive:**</mark>

`sudo nano /etc/apache2/mods-available/evasive.conf`

by configuring **mod\_evasive** you will be able to get notification email when someone attacks you:so add the following:

`DOSLogDir "/var/log/apache2/"`

`DOSWhitelist 127.0.0.1`

`DOSSystemCommand "sudo /usr/bin/sendmail -t 'YourEmailAddress@example.com'"`

To check if everything is working you can make a DOS or DDOS attack using:

&#x20; `ab -n 10000 -c 1000`[ `http://your_server_ip/`](http://your\_server\_ip/)

so here we are sending 10000 packets so the evasive  mod must detect them

<mark style="color:green;">To check if it works got to :</mark> `nano /var/log/apache2/error.log`

And check the logs it should appear evasive error

## <mark style="background-color:red;">Q & A:</mark>

### <mark style="background-color:green;">Question:</mark>

What are the implications of enabling these directives? In other words, does enabling these directives have any sort of effect on other aspects of the server?

Try to find out what the limitations/warnings of these settings are and add them to the section below.

### <mark style="background-color:green;">Answer:</mark>

To recap: <mark style="color:red;">**ModSecurity**</mark> is a web application firewall (WAF) that provides comprehensive protection against a wide range of web attacks, such as SQL injection, cross-site scripting (XSS), and other code injection attacks. It can inspect incoming and outgoing HTTP traffic and apply rules to detect and block malicious requests.

<mark style="color:red;">**ModEvasive**</mark> provides protection against Denial of Service (DoS), Distributed Denial of Service (DDoS), and brute force attacks. It works by monitoring incoming traffic for patterns indicative of such attacks and temporarily blocking IP addresses that proceed number of requests sent

Let's start with <mark style="color:blue;">**Modsecurity**</mark> so as we mentionned it will protect me against web based attacks:

* <mark style="color:green;">**CPU and Memory Usage**</mark><mark style="color:green;">:</mark> Depending on the complexity of rulesets and the volume of traffic, mod\_security can **consume significant CPU and memory resources, potentially affecting server**&#x20;
* <mark style="color:green;">**Compatibility Issues**</mark><mark style="color:green;">:</mark>
  * <mark style="color:green;">**Application Behavior**</mark><mark style="color:green;">:</mark> mod\_security may interact unexpectedly with certain web applications, especially those utilizing unconventional or non-standard HTTP requests or responses.

So basically these directives are not considered as a negative effect on the server, their limitations are normal (data usage for example or too many logs...)



## <mark style="color:red;">10)Secure My SQL:</mark>

Protecting against malware and exploits,prevent sql injection attacks,ensure data integrity,prevent unauthorized access

1. <mark style="color:blue;">**Install MySQL**</mark>

`sudo apt install mysql-server -y`

2. <mark style="color:blue;">**Run Security Script**</mark>

`sudo mysql_secure_installation`

3. <mark style="color:blue;">**Disable Remote Root Access**</mark>

* <mark style="color:green;">Log into MySQL:</mark>

`sudo mysql -u root -p`

* <mark style="color:green;">Execute the following commands:</mark>

```sql
USE mysql;
UPDATE user SET Host='localhost' WHERE User='root';
FLUSH PRIVILEGES;
Exit;
```

4\.     <mark style="color:blue;">**Prevent SQL injection attacks:**</mark>

<mark style="color:green;">Example in PHP (using PDO): put this in config file:</mark>

```php
$pdo = new PDO('mysql:host=localhost;dbname=testdb', 'username', 'password');
$stmt = $pdo->prepare('SELECT * FROM users WHERE username = :username');
$stmt->bindParam(':username', $username);
$stmt->execute();
$results = $stmt->fetchAll();
```

<mark style="color:green;">Then go to mySQL:make a user secured password:</mark>

```sql
CREATE USER 'appuser'@'localhost' IDENTIFIED BY 'securepassword';
GRANT SELECT, INSERT, UPDATE, DELETE ON mydatabase.* TO 'appuser'@'localhost';
Remove anonymous users and test databases:
DELETE FROM mysql.user WHERE User='';
DROP DATABASE test;
FLUSH PRIVILEGES;
```

## <mark style="background-color:red;">Q & A:</mark>

### <mark style="background-color:green;">Question:</mark>

Can you give more details on how this config change will secure the SQL server more? How exactly does this prevent an SQL injection attack?

A real life example/scenario would be nice to have also if possible.

### <mark style="background-color:green;">Answer:</mark>

<mark style="color:blue;">**This config prevent being attacked**</mark> by using parameterized queries with bound parameters .In the PHP code The SQL query is prepared with placeholders (`:username`) instead of directly include user input. This prevents attackers from injecting malicious SQL code directly into the query.

<mark style="color:green;">**Bound Parameters**</mark><mark style="color:green;">:</mark> The user input (`$username`) is bound to the placeholder using `bindParam()`. This ensures that the user input is treated as data, not as part of the SQL command, preventing SQL injection attacks.

<mark style="color:green;">MySQL User Privileges:</mark>

<mark style="color:blue;">**Limited Privileges**</mark><mark style="color:blue;">:</mark> The MySQL user `appuser` (user name)is created with restricted privileges (`SELECT`, `INSERT`, `UPDATE`, `DELETE`) only on the specific database (`mydatabase`). This principle of least privilege ensures that even if an SQL injection vulnerability is exploited, the attacker's actions are limited to the allowed operations.

#### <mark style="color:blue;">Secure MySQL Configuration:</mark>

<mark style="color:green;">In the MySQL command:</mark>

```
Remove anonymous users and test databases:
```

* <mark style="color:green;">**Remove Anonymous Users and Test Databases**</mark><mark style="color:green;">:</mark> Anonymous users and test databases are removed from the MySQL instance. These entities could potentially be exploited by attackers to gain unauthorized access or gather information about the database structure.

<mark style="color:blue;">**So for the scenario let's take LAL company as an example:**</mark>

Securing the MySQL database with measures such as parameterized queries, restricted user privileges, and secure configuration is essential for protecting against SQL injection attacks and maintaining the security and integrity of web applications. While MySQL itself provides robust security features, it is crucial for us to implement additional security measures tailored to their specific use cases and requirements. But using this type of configuration anyways can help us protect our company from these type of attacks.

## <mark style="color:red;">11) Regular Backups:</mark>

We should make <mark style="color:yellow;">**2 backups**</mark> to secure our server data and sync it sec by sec

it is important so if one server goes down we can use the other,<mark style="color:yellow;">**Cron job**</mark> is to schedule backup time&#x20;

Backup is made with a .sh file that we can put  in the external disk also <mark style="color:yellow;">**rsync**</mark>.

1. <mark style="color:blue;">**Automate Backups Using cron and rsync**</mark>

* <mark style="color:green;">Create a backup script, e.g.</mark>, `/usr/local/bin/backup.sh:`

`sudo nano /usr/local/bin/backup.sh`

* <mark style="color:green;">Example script:</mark>

`#!/bin/bash`

`rsync -a --delete /var/www/ /backup/www/`

`rsync -a --delete /etc/ /backup/etc/`

* <mark style="color:green;">Make the script executable:</mark>

`sudo chmod +x /usr/local/bin/backup.sh`

<mark style="color:green;">add this script inside the file using nano:</mark>

`#!/bin/bash`

`# Variables`

`BACKUP_DIR="/path/to/backup"`

`MYSQL_USER="your_mysql_user"`

`MYSQL_PASSWORD="your_mysql_password"`

`DATABASE_NAME="your_database_name"`

`DATE=$(date +"%Y%m%d%H%M")`

<mark style="color:green;"># Create backup directory if it doesn't exist</mark>

`mkdir -p "$BACKUP_DIR"`

<mark style="color:green;"># Backup MySQL database</mark>

`mysqldump -u $MYSQL_USER -p$MYSQL_PASSWORD $DATABASE_NAME > "$BACKUP_DIR/$DATABASE_NAME-$DATE.sql"`

<mark style="color:green;"># Optional: Compress the backup file</mark>

`gzip "$BACKUP_DIR/$DATABASE_NAME-$DATE.sql"`

<mark style="color:green;"># Remove backups older than 7 days</mark>

`find "$BACKUP_DIR" -type f -name "*.gz" -mtime +7 -exec rm {} \;`

<mark style="color:green;"># Exit script</mark>

`exit 0`

* <mark style="color:green;">Add a</mark> <mark style="color:green;"></mark><mark style="color:green;">**cron job**</mark><mark style="color:green;">:to schedule backup time go to</mark> `/usr/local/bin`

`sudo crontab -e`

* <mark style="color:green;">Add the following line to run the script daily at 2 AM</mark>:

&#x20;   `0 2 * * * /usr/local/bin/backup.sh`

### <mark style="color:blue;">**How to add rsync:**</mark>

Note: you have to install rsync on both machines (local & remote)

**2 methods of rsync**:rsync using TCP and other using SSH

rsync is a powerful and versatile tool for copying and synchronizing files and directories between different locations over a network or locally. It is commonly used for backups and mirroring because it efficiently transfers only the changed parts of files, saving bandwidth and time.

<mark style="color:green;">Basic syntax of rsync:</mark>

`rsync [options] source destination`

`·  -a (archive mode): Preserves symbolic links, devices, attributes, permissions, ownerships, etc.`

`·  -v (verbose): Provides detailed output.`

`·  -z (compress): Compresses file data during the transfer.`

`·  -h (human-readable): Outputs numbers in a human-readable format.`

`·  -P: Combines --progress and --partial to show progress during transfer and keep partially transferred files.`

`·  --delete: Deletes files in the destination that are not in the source.`

<mark style="color:green;">To back up a directory locally, you can use:</mark>

`rsync -avh --delete /source/directory/ /backup/directory/`

<mark style="color:green;">Remote backup using SSH</mark>

`rsync -avh --delete -e ssh /localhost/usr/local/bin/backup.sh  root@ 192.168.115.132:/blackhost/Desktop`

<mark style="color:green;">Create backup script and put the same thing:</mark>

`#!/bin/bash`

`# Variables`

`SOURCE_DIR="/source/directory/"`

`DEST_DIR="/backup/directory/"`

`REMOTE_USER="user"`

`REMOTE_HOST="remote_server"`

`REMOTE_DIR="/remote/backup/directory/"`

<mark style="color:green;"># Perform local backup</mark>

`rsync -avh --delete $SOURCE_DIR $DEST_DIR`

<mark style="color:green;"># Perform remote backup over SSH</mark>

`rsync -avh --delete -e ssh $SOURCE_DIR $REMOTE_USER@$REMOTE_HOST:$REMOTE_DIR`

\


[^1]: 

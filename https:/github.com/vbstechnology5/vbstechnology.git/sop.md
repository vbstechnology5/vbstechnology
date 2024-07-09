# SOP

### Contents:

1. Setup and updates:
2. Configure and secure SSH(configure fail2ban)
3. Firewall setup:UFW rules etc…
4. Monitoring and audit
5. Protect Against SYN Flood Attacks
6. Configure AppArmor
7. Secure Apache from DDOS & DOS attacks
8. Secure My SQL from sql injection attacks
9. Regular Backups (using crontab & rsync)

\


## 1.Setup and updates:

Avoid using the root account for regular management.

Create a new user:

`sudo adduser newuser`

`sudo adduser newuser sudo`&#x20;

`sudo usermod -aG sudo newuser`

\


## 2.Configure and secure SSH:

Configure SSH

* Disable root login and change the default SSH port to enhance security:

`sudo nano /etc/ssh/sshd_config`

* Modify or add the following lines:

`Port 4589`&#x20;

`PermitRootLogin no`

* Reload the SSH service:

`sudo systemctl reload ssh`



Can you get locked out of the server? How and when?

First when the sudo configuration (while editing ssh config)is incorrect or if no users have sudo access,we will face many problems especially in management.



Next when we make a user permission for example if I gave to rami some permissions and I want him to remote the server using ssh.While configuring the SSH we will generate an SSH key (public and private) for rami so if he is locked from the server he can use a command using the SSH private key reenter the server (Note: Don't forget to save the Key in a txt file on the server and on an external disk or my local machine so it will never get lost)



Third,It is important to give just one or two users sudo privileges for necessary administrative functions and that's a part of management to make user’s permissions depending on the user’s tasks.

If Rami or Zakaria need to perform an action that they do not have permission to and root login is disabled:we have many options

* If both users don’t have sudo access they cant’t make any action.
* If both users have limited sudo access,they can only access some administrative tasks that are given by the admin

So to avoid being prohibited to make any action the third option is the best:

Before disabling the root in the configuration we will give just one user for example zakaria FULL sudo access so this user called zakaria will be able to manage anything by using sudo su

We have another option but it’s not very recommend (in case we forgot to put the thief option)is to make a fallback plan :we create a documented procedure to temporary re-enable root login if necessary

Add user another than root and give me permissions for sudo su to change some things:

`sudo usermod -aG sudo username`\
Verify the user has sudo privileges:

`su - username`

`sudo whoami`

&#x20;

1. Use Key-Based Authentication
2.
   * Generate a key pair on your local machine : `ssh-keygen -t rsa -b 4096`
   * For example he gave me a private/public key called SHA256

Copy the public key to the server:

`ssh-copy-id -i ~/.ssh/id_rsa.pub newuser@your_server_ip`

* Disable password authentication by editing using nano /etc/ssh/sshd\_config:

&#x20;`PasswordAuthentication no`

* Reload SSH service again:

`sudo systemctl reload ssh`

3.Install Fail2Ban

* Protect against brute-force attacks:
* It is an open-source intrusion prevention software framework that protects computer servers from brute-force attacks. Fail2ban works by monitoring log files:Update fw rules.

`sudo apt install fail2ban`

`sudo systemctl enable fail2ban`

`sudo systemctl start fail2ban`

* Configure Fail2Ban as needed by editing /etc/fail2ban/jail.local.
* Check what to configure on Google!

#### How Fail2ban Works

1. **Log Monitoring:**

* Fail2ban continuously monitors specified log files for predefined patterns that indicate possible brute force attacks, such as multiple failed login attempts.

1. D**etection:**

* Fail2ban uses filters to identify suspicious activity. These filters are defined using regular expressions that match specific patterns in the log files.

1. B**anning:**

* When a pattern matching a potential brute force attack is detected, Fail2ban triggers actions to mitigate the attack. The most common action is to ban the offending IP address for a certain period.

1. Configurable Actions:

* Fail2ban can take various actions when a potential threat is detected, including:
* Adding a rule to iptables to block the IP address
* Sending email notifications to administrators
* Logging the event for further analysis

1. Customizable Settings:

* Fail2ban allows administrators to configure various settings, including:
* The number of failed attempts before an IP is banned (maxretry)
* The duration of the ban (bantime)
* Which log files to monitor
* Specific filters and actions for different services

3\.    To display a list of IP addresses currently banned by fail2ban, type the following command:

`copyiptables -S`

4\.    For example, the following line shows an IP address that the SSH jail has banned:

`-A fail2ban-SSH -s 10.0.1.124/32 -j REJECT --reject-with icmp-port-unreachable`



## 3) Firewall setup: Uncomplicated Firewall (UFW)

Yes we can put the UFW instead of the main firewall but we cannot put it together, they may conflict with each other because of redundancy. UFW is an interface to manage main firewall rules its easy to use and it manages iptables etc… If you prefer simplicity and user friendly and not advanced features UFW is for you.

1. Enable/disable UFW (Uncomplicated Firewall)

`sudo ufw enable/disable`

`sudo ufw status(check ufw status)`

`sudo ufw reset(reset settings ufw)`

* Allow necessary ports and deny others:

`sudo ufw allow 2200/tcp`

`sudo ufw allow 80/tcp`

`sudo ufw allow 443/tcp`

`sudo ufw enable`

Secure Network Services

1. Disable Unused Services
2.

    * List all services:

    `sudo systemctl list-unit-files --type=service`

    * List all services:

    `sudo systemctl list-unit-files --type=service`

    * Disable services not in use:

`sudo systemctl disable service_name`

`sudo systemctl stop service_name`

&#x20;

## 4) Monitoring and Auditing:

Is a continuous observation and analysis of systems, networks, and applications to ensure they are operating correctly, securely, and efficiently

Disadvantages of logwatch:

Not Instantaneous: Logwatch is not designed for real-time log monitoring. It generates reports based on past log entries, typically on a daily or weekly basis.

I recommend for LAL NGO to use Grafana its better and have more security features and it provides real time analysis

Backup

Wait for the [step 9](sop.md#id-4-monitoring-and-auditing) to do it

1. Regular Backups
2.
   * Schedule regular backups of critical data and configurations.
   * Tools like rsync or automated solutions like Deja Dup can be useful.
3. Test Backups
4.
   * Regularly test your backups to ensure data can be restored successfully.



## 5) Protect Against SYN Flood Attacks:

SYN flood attacks are a type of Denial of Service (DoS) attack that exploits the TCP handshake process to overwhelm a target system, rendering it unable to process legitimate traffic

1. Edit sysctl.conf

`sudo nano /etc/sysctl.conf`

2. Add the Following Lines

`net.ipv4.tcp_syncookies = 1`

`net.ipv4.conf.all.rp_filter = 1         #Enables reverse path filtering on all interfaces, which helps prevent IP spoofing by ensuring that the source of incoming packets is reachable via the same interface.`

`net.ipv4.conf.default.rp_filter = 1`

`net.ipv4.tcp_timestamps = 0`

`net.ipv4.icmp_echo_ignore_broadcasts = 1`

`net.ipv4.icmp_ignore_bogus_error_responses = 1`

`net.ipv4.tcp_fin_timeout = 15`

`net.ipv4.tcp_keepalive_time = 300`

&#x20;

`sudo sysctl -p (to apply changes)`

These lines will enhance system network security for example net.ipv4.tcp\_syncookies = 1

Enables SYN cookies, which protect against SYN flood attacks by allowing the server to handle more SYN requests.These settings collectively improve the security posture of your server by mitigating common network-based attacks such as SYN floods, IP spoofing, Smurf attacks, and timestamp-based attacks.Last line is for Adjusting TCP timeouts and keepalive intervals can impact how quickly connections are cleaned up or detected as dead. Ensure that these values are appropriate for your typical network traffic and application behavior



## 6) Configure AppArmor:

AppArmor stands for "Application Armor". It is a Linux kernel security module that provides mandatory access control (MAC) security by restricting the capabilities of programs. AppArmor uses profiles, which are sets of rules defining what system resources (like files, capabilities, network access) an application can access, thereby reducing the potential damage from a compromised application.

Limits the actions of applications, reducing the risk of exploits affecting the entire system.\\

Assess your security needs. If you run applications that process sensitive data or if your server is exposed to the internet, AppArmor can provide an extra layer of security.

So yes apparmor is useful for LAL and especially for tabshoura app we can configure it to secure our data on it because our server is exposed to the internet so apparmor is an additional layer.

Install: `sudo apt install apparmor apparmor-utils -y`

1. Enable AppArmor Profiles

`sudo aa-enforce /etc/apparmor.d/*`

2. Generate Profile for a specific app

`sudo aa-genprof /usr/bin/myapp`

Place the Profile in the AppArmor Directory:

Once generated, the profile will be placed in the /etc/apparmor.d

1. Edit the profile

`sudo nano /etc/apparmor.d/usr.bin.myapp`

Check here:

`https://ubuntu.com/tutorials/beginning-apparmor-profile-development#1-overview`

## 7) Secure Apache:

Enabling mod\_security and mod\_evasive adds additional layers of security to your Apache web server by providing protection against various types of attacks and helping mitigate denial-of-service (DoS) attacks.

1. Install Apache

`sudo apt install apache2 -y`

2. Disable Directory Listing
3.
   * Edit the default Apache configuration:

`sudo nano /etc/apache2/apache2.conf`

* Add or modify the following:

`<Directory /var/www/>`

`Options -Indexes`

`</Directory>`

By adding this, you are disabling directory indexing for the /var/www/ directory, which is a common security measure to prevent unauthorized access to directory contents.

ModSecurity is an open-source web application firewall (WAF) module that provides real-time application layer protection for web applications and APIs. It is typically deployed as an Apache or Nginx module, intercepting HTTP requests and responses to apply a set of rules that help protect against various types of web-based attacks and vulnerabilities.

Web Application Firewall (WAF):

* Attack Detection: Monitors incoming HTTP requests and outgoing responses in real-time to detect and block malicious activity, such as SQL injection, cross-site scripting (XSS), and cross-site request forgery (CSRF).
* Rule-based Filtering: Applies a set of predefined and customizable rules to identify and block suspicious or malicious requests.
* Also it generates detailed logs of HTTP transactions

It’s used to prevent SQL Injection attacks&#x20;

Install mod\_security and enable it :

`sudo apt update`

`sudo apt-get install libapache2-mod-security2`

3. Enable mod\_security and mod\_evasive

`sudo apt install libapache2-mod-security2 libapache2-mod-evasive -y`

`sudo a2enmod security2`

`sudo a2enmod evasive`

1. Configure mod\_evasive:

`sudo nano /etc/apache2/mods-available/evasive.conf`

by configuring mod\_evasive you will be able to get notification email when someone attacks you:so add the following:

`DOSLogDir "/var/log/apache2/"`

`DOSWhitelist 127.0.0.1`

`DOSSystemCommand "sudo /usr/bin/sendmail -t 'YourEmailAddress@example.com'"`

`To check if everything is working you can make a DOS or DDOS attack using:`

&#x20;`//   ab -n 10000 -c 1000`[ `http://your_server_ip/`](http://your\_server\_ip/)

so here we are sending 10000 packets so the evasive  mod must detect them

To check if it works got to : `nano /var/log/apache2/error.log`

And check the logs it should appear evasive error

## 8) Secure My SQL:

Protecting against malware and exploits, prevent sql injection attacks, ensure data integrity, prevent unauthorized access

1. Install MySQL

`sudo apt install mysql-server -y`

2. Run Security Script

`sudo mysql_secure_installation`

3. Disable Remote Root Access
4.
   * Log into MySQL:

`sudo mysql -u root -p`

* Execute the following commands:

U`SE mysql;`

`UPDATE user SET Host='localhost' WHERE User='root';`

`FLUSH PRIVILEGES;`

`Exit;`

4\.      Prevent SQL injection attacks:

PDO is a PHP extension  that provides a consistent interface for accessing databases in PHP applications.Provides a consistent API for connecting to and querying different types of databases. Prepared statements and parameterized queries in PDO effectively prevent SQL injection attacks by separating SQL code from user input.

SQL injection attacks occur when an attacker manipulates SQL queries through user input, potentially gaining unauthorized access to the database or manipulating data. PDO mitigates SQL injection attacks primarily through two mechanisms:

1. Prepared Statements:
2.
   * Parameterized Queries: Instead of embedding user input directly into SQL queries, PDO uses placeholders (? or named parameters like :name) for variables in SQL statements.
   * Separation of Data and Code: User input is treated as data (parameters), not as executable SQL code. This separation prevents attackers from injecting malicious SQL commands.
3. Automatic Parameter Escaping:
4.
   * Character Escaping: PDO automatically escapes special characters in parameters, such as quotes ('), preventing them from being interpreted as part of the SQL syntax.
   * Database Driver Support: Each PDO database driver handles parameter binding and escaping according to the specific rules of the underlying DBMS, ensuring compatibility and security.

We can use it for our website if we can but not recommended have many disadvantages!

&#x20;     Example in PHP (using PDO): put this in config file:



`$pdo = new PDO('mysql:host=localhost;dbname=testdb', 'username', 'password');`

`$stmt = $pdo->prepare('SELECT * FROM users WHERE username = :username');`

`$stmt->bindParam(':username', $username);`

`$stmt->execute();`

`$results = $stmt->fetchAll();`



Then go to mySQL : make a user secured password:

`CREATE USER 'appuser'@'localhost' IDENTIFIED BY 'securepassword';`

`GRANT SELECT, INSERT, UPDATE, DELETE ON mydatabase.* TO 'appuser'@'localhost';`



Remove anonymous users and test databases:

`DELETE FROM mysql.user WHERE User='';`

`DROP DATABASE test;`

`FLUSH PRIVILEGES;`

## 9) Regular Backups:

We should make 2 backups to secure our server data and sync it sec by sec. It is important so if one server goes down we can use the other. Cron job is for scheduling backup time.

1. Full system backup (image backup):this captures the entire server fro disaster recovery&#x20;
2. Data Backup (component backup) :this captures specefic components or critical data

Backup is made with a .sh file that we can put  in the external disk

For data backup we recommend you to use rsync

1. Automate Backups Using cron and rsync
2.
   * Create a backup script, e.g., /usr/local/bin/backup.sh:

`sudo nano /usr/local/bin/backup.sh`

* Example script:

`#!/bin/bash`

`rsync -a --delete /var/www/ /backup/www/`

`rsync -a --delete /etc/ /backup/etc/`

* Make the script executable:

`sudo chmod +x /usr/local/bin/backup.sh`

&#x20;

Add this script inside the file using nano:

`# Variables`

`BACKUP_DIR="/path/to/backup"`

`MYSQL_USER="your_mysql_user"`

`MYSQL_PASSWORD="your_mysql_password"`

`DATABASE_NAME="your_database_name"`

`DATE=$(date +"%Y%m%d%H%M")`

&#x20;

Create backup directory if it doesn't exist

`mkdir -p "$BACKUP_DIR"`

Backup MySQL database

`mysqldump -u $MYSQL_USER -p$MYSQL_PASSWORD $DATABASE_NAME > "$BACKUP_DIR/$DATABASE_NAME-$DATE.sql"`

Optional: Compress the backup file

`gzip "$BACKUP_DIR/$DATABASE_NAME-$DATE.sql"`

Remove backups older than 7 days

`find "$BACKUP_DIR" -type f -name "*.gz" -mtime +7 -exec rm {} \;`

Exit script

`exit 0`

* Add a cron job:to schedule backup time go to /usr/local/bin

`sudo crontab -e`

* Add the following line to run the script daily at 2 AM:

`0 2 * * * /usr/local/bin/backup.sh`

This Backup script is made for both full and component backup

`BACKUP_DIR="/path/to/backup"`

`DATE=$(date +"%Y%m%d%H%M")`

Create backup directory if it doesn't exist

`mkdir -p "$BACKUP_DIR"`

Full System Backup (Image Backup) ###

Example: Backup entire system (adjust paths as needed)

`rsync -a --delete / /backup/full-system/` &#x20;

Backup the entire root directory

Data Backup (Component Backup) ###

Example: Backup MySQL database

`MYSQL_USER="your_mysql_user"`

`MYSQL_PASSWORD="your_mysql_password"`

`DATABASE_NAME="your_database_name"`

Backup MySQL database

`mysqldump -u $MYSQL_USER -p$MYSQL_PASSWORD $DATABASE_NAME > "$BACKUP_DIR/$DATABASE_NAME-$DATE.sql"`

Optional: Compress the backup file

`gzip "$BACKUP_DIR/$DATABASE_NAME-$DATE.sql"`

Remove backups older than 7 days

`find "$BACKUP_DIR" -type f -name "*.gz" -mtime +7 -exec rm {} \;`

Exit script

`exit 0`

Additional tools:

[Duplicati](https://www.duplicati.com/) is a versatile and open-source backup solution that offers a user-friendly interface for backing up data to various cloud storage providers, local storage, or network shares.it is a solution for backing up servers, offering features like encryption, scheduling, monitoring, and various storage options.

#### Setting Up Duplicati for Server Backup

**1. Installation**

* Download and Install: Visit the[ Duplicati website](https://www.duplicati.com/) and download the installer suitable for your server's operating system (Windows, Linux, macOS).
* Installation Steps: Follow the installation instructions provided for your operating system. Typically, Duplicati requires minimal dependencies and can be installed easily.

**2. Configuration**

* Web Interface: After installation, Duplicati provides a web-based interface that you access via a web browser. By default, it runs on http://localhost:8200.
* Initial Setup: Upon first access, configure basic settings such as storage providers (like Amazon S3, Google Drive, or local folder), encryption settings, and backup schedules.



### **How to add rsync:**

Note: you have to install rsync on both machines (local & remote)

2 methods: rsync using TCP or using SSH

rsync is a powerful and versatile tool for copying and synchronizing files and directories between different locations over a network or locally. It is commonly used for backups and mirroring because it efficiently transfers only the changed parts of files, saving bandwidth and time.

Basic syntax of rsync:

rsync \[options] source destination

·  -a (archive mode): Preserves symbolic links, devices, attributes, permissions, ownerships, etc.

·  -v (verbose): Provides detailed output.

·  -z (compress): Compresses file data during the transfer.

·  -h (human-readable): Outputs numbers in a human-readable format.

·  -P: Combines --progress and --partial to show progress during transfer and keep partially transferred files.

·  --delete: Deletes files in the destination that are not in the source.

To back up a directory locally, you can use:

`rsync -avh --delete /source/directory/ /backup/directory/`

Remote backup using SSH

`rsync -avh --delete -e ssh /localhost/usr/local/bin/backup.sh  root@ 192.168.115.132:/blackhost/Desktop`

Create backup script and put the same thing:

`SOURCE_DIR="/source/directory/"`

`DEST_DIR="/backup/directory/"`

`REMOTE_USER="user"`

`REMOTE_HOST="remote_server"`

`REMOTE_DIR="/remote/backup/directory/"`

Perform local backup

`rsync -avh --delete $SOURCE_DIR $DEST_DIR`

Perform remote backup over SSH

`rsync -avh --delete -e ssh $SOURCE_DIR $REMOTE_USER@$REMOTE_HOST:$REMOTE_DIR`



Steps:

* Disable root login, and password authentication from /etc/ssh/sshd\_config
* Create user with sudo privilege, and add the users ssh-key to the server
* Enable key authentication only
* Change ssh port 4589
* Configure UFW to allow only the needed ports and protocol  (ssh & apache Full 443)
* Install Fail2Ban&#x20;
* Install Lynis (for directory permission scanning)&#x20;
* Apache Disable Directory Listing
* Mysql Disable Remote Root Access



To check:

Is there some sort of tool (or solution) to scan the server to determine whether there are some problematic directories? (for example, directories that have 777 permissions that we have forgotten about), or for example certain files that are owned by "root". We want to make sure we are following the security best practices when it comes to managing a server.



## Moodle Related Security: (additional advice from Ken Task)

* In apache config turn off ServerSignature. Forcing an error discloses versions of apache and platform.
* Consider 'hardening' the server - don't display directories without a default page - your /local/ directory is viewable right now. Seeing the names of the directories (really local plugins in Moodle-ese lingo) in that listing could give a 'black hat' something to work with.
* Remove upgrade.txt files found in multiple directories of moodle code as they disclose  what base version your server is running: ===4.3===

#### Why Turn Off ServerSignature?

Turning off ServerSignature is recommended for security reasons because it prevents Apache from including the server version in error pages and other server-generated documents. This information can be useful for attackers trying to exploit vulnerabilities specific to certain Apache versions or platforms.



\

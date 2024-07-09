---
description: >-
  SSH tunelling is secure due to its ability to secure data and to encrypt them
  over the internet. His security depends on how it's implemented and managed.
---

# SSH Tunelling

### <mark style="color:blue;">How SSH Tunneling Works</mark>

1. <mark style="color:green;">**Establishing the Tunnel**</mark><mark style="color:green;">:</mark> An SSH client initiates a connection to an SSH server. This connection is encrypted, ensuring that any data transmitted over it remains confidential and secure from eavesdropping.
2. <mark style="color:green;">**Port Forwarding**</mark><mark style="color:green;">:</mark> Once the SSH tunnel is established, the client can request the server to forward traffic from a specific local port to a port on the server. This is done using the `-L` flag for local port forwarding or the `-R` flag for remote port forwarding. For example:

<pre class="language-sh"><code class="lang-sh"><strong>ssh -L &#x3C;LOCAL_PORT>:&#x3C;SERVER_IP>:&#x3C;REMOTE_PORT> &#x3C;USER>@&#x3C;SERVER>
</strong></code></pre>

To set up SSH tunneling from a server to Grafana, follow these steps:

1. <mark style="color:green;">**Ensure SSH Access:**</mark> Make sure you have SSH access to the server where Grafana is running. You need the hostname or IP address and the SSH port (usually 22).
2. <mark style="color:green;">**Identify Grafana Port:**</mark> Determine the port Grafana is running on (default is 3000).
3.  <mark style="color:green;">**Open SSH Tunnel:**</mark> Use an SSH client to create the tunnel. The basic command structure is:

    {% code overflow="wrap" %}
    ```sh
    shCopier le codessh -L [local_port]:localhost:[remote_grafana_port] [username]@[server_address]
    ```
    {% endcode %}

    Here’s a concrete example:

    ```sh
    ssh -L 8080:localhost:3000 user@example.com
    ```

    * `-L` specifies the local port to forward.
    * `8080` is the local port on your machine.
    * `localhost` is the address on the remote server.
    * `3000` is the port Grafana is listening on the remote server.
    * `user@example.com` is your SSH username and server address.
4.  <mark style="color:green;">**Access Grafana Locally:**</mark> After setting up the SSH tunnel, open a web browser and go to:

    ```sh
    http://localhost:8080
    ```

    This will route your local requests through the SSH tunnel to the remote Grafana server.
5. <mark style="color:green;">**Automating the Tunnel (Optional):**</mark> You can use SSH configuration files or tools like `autossh` to maintain the tunnel.

### <mark style="color:blue;">Example Using SSH Configuration File</mark>

<mark style="color:green;">Edit or create the SSH configuration file</mark> (`~/.ssh/config`) to include:

```sh
Host grafana_tunnel
  HostName example.com
  User user
  LocalForward 8080 localhost:3000
```

Now, you can start the tunnel with:

```sh
ssh grafana_tunnel
```

#### Example Using `autossh` (Optional)

Install `autossh` (on Ubuntu, use `sudo apt-get install autossh`) and then run:

```sh
autossh -M 0 -f -N -L 8080:localhost:3000 user@example.com
```

* `-M 0` disables the monitoring port.
* `-f` runs the command in the background.
* `-N` tells SSH not to execute remote commands.

This setup ensures a persistent tunnel, automatically reconnecting if the connection drops.

By following these steps, you can securely access Grafana running on a remote server via an SSH tunnel.

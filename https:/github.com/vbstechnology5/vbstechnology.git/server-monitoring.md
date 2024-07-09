---
description: Grafana tutorial Prometheus server monitoring
---

# Server Monitoring

### <mark style="color:blue;">What is Grafana ?</mark>

<mark style="color:yellow;">**Grafana**</mark> is an open-source analytics and monitoring solution primarily used for visualizing time-series data. It allows users to create, explore, and share dashboards with metrics and logs, enabling effective monitoring and observability of systems and applications.Grafana can connect to various data sources and applications.

<mark style="color:yellow;">**Grafana Cloud**</mark> is a hosted version of Grafana . It offers a scalable and fully managed environment for running Grafana, Prometheus, and Graphite, along with other tools for observability. Grafana Cloud simplifies the setup and management of monitoring infrastructure, providing users with a platform for building dashboards, analyzing data, and alerting on critical issues. It also offers features like long-term data storage, user authentication, and access control.

What are <mark style="color:blue;">**logs**</mark> and where is it saved if something suspicious happened we can check log and to do it:

`cd /var/log/grafana (to see grafana logs)`

Configure <mark style="color:blue;">**prometheus**</mark> server to pull metrics from the target(target can be docker,kubernets,linux,etc...) and grafana takes from prometheus server and translate it to quieries and you can visualize with user friendly dashboard

### <mark style="color:blue;">Install Prometheus on Ubuntu server</mark>

### <mark style="color:green;">Download the latest Prometheus release</mark>

`wget https://github.com/prometheus/prometheus/releases/download/v2.30.3/prometheus-2.30.3.linux-amd64.tar.gz`

### <mark style="color:green;">Extract the downloaded archive</mark>

`tar xvfz prometheus-*.tar.gz`

### <mark style="color:green;">Navigate to the extracted directory</mark>

Rename the directory and enter it using  `cd prometheus`

### <mark style="color:green;">Copy Prometheus binary to /usr/local/bin</mark>

`sudo cp prometheus /usr/local/bin/`

### <mark style="color:green;">Copy Prometheus configuration file to /etc/prometheus</mark>

`sudo mkdir -p /etc/prometheus sudo cp prometheus.yml /etc/prometheus/`

### <mark style="color:green;">Copy Prometheus console templates to /etc/prometheus</mark>

`sudo cp -r consoles/ console_libraries/ /etc/prometheus/`

<mark style="color:green;">Now got to prometheus config file:</mark>

`nano /etc/systemd/system/prometheus.service`

1.  <mark style="color:green;">**Create a Prometheus Configuration File**</mark><mark style="color:green;">:</mark> Edit the Prometheus configuration file `/etc/prometheus/prometheus.yml` to define the targets you want to monitor and any other configuration options. Here's a basic example:

    ```yaml
    yamlCopy codeglobal:
      scrape_interval:     15s
      evaluation_interval: 15s

    scrape_configs:
      - job_name: 'prometheus'
        static_configs:
          - targets: ['localhost:9090']
      # Add more scrape_configs for additional targets
    ```
2.  <mark style="color:green;">**Create a Prometheus Service File**</mark><mark style="color:green;">:</mark> Create a systemd service file for Prometheus at `/etc/systemd/system/prometheus.service`:

    ```ini
    [Unit]
    Description=Prometheus
    Wants=network-online.target
    After=network-online.target

    [Service]
    User=prometheus
    Group=prometheus
    Type=simple
    ExecStart=/usr/local/bin/prometheus \
      --config.file /etc/prometheus/prometheus.yml \
      --storage.tsdb.path /var/lib/prometheus \
      --web.console.templates=/etc/prometheus/consoles \
      --web.console.libraries=/etc/prometheus/console_libraries

    [Install]
    WantedBy=multi-user.target
    ```
3.  <mark style="color:green;">**Create Prometheus User and Directories**</mark><mark style="color:green;">:</mark> Create a dedicated user and directories for Prometheus:

    ```bash
    sudo useradd --no-create-home --shell /bin/false prometheus
    sudo mkdir -p /var/lib/prometheus
    sudo chown prometheus:prometheus /var/lib/prometheus
    sudo systemctl daemon-reload
    sudo systemctl enable prometheus
    sudo systemctl start prometheus
    ```
4. **Verify Prometheus Installation**: Access Prometheus web UI by navigating to `http://your_server_ip:9090` in your web browser. You should see the Prometheus interface.

This setup provides a basic Prometheus installation. You can further configure Prometheus, add exporters for monitoring different services, set up alerting rules, and integrate with Grafana for visualization.

<mark style="color:red;">**Faced this error: Prometheus**</mark> <mark style="color:red;"></mark><mark style="color:red;">web 2 interface not loading</mark>&#x20;

So i checked my firewall ,I was blocking the 9090 port due to security SOP

<mark style="color:green;">**Check Firewall Settings**</mark><mark style="color:green;">:</mark> Ensure that the firewall settings on your server allow incoming connections on port 9090, which is the default port for the Prometheus web interface. You can check the firewall status and allow traffic on port 9090 using:

```lua
sudo ufw status
sudo ufw allow 9090
```

`systemctl status prometheus`&#x20;

`systemctl restart prometheus`

<mark style="color:blue;">Now lets configure the web through grafana & Prometheus!</mark>

Go to **grafana** go to settings add data source (Prometheus) put your web2 server link choose a dashboard and you're ready to explore your Prometheus dashboard

<figure><img src=".gitbook/assets/Screenshot 2024-06-07 132938.png" alt=""><figcaption><p>Grafana prometheus dashboard</p></figcaption></figure>

## <mark style="color:blue;">I wanna make a security dashboard from this website and it requires</mark> <mark style="color:red;">elasticsearch</mark> <mark style="color:blue;">as a datasource:</mark>&#x20;

#### [<mark style="color:purple;">https://grafana.com/grafana/dashboards/?search=security</mark>](https://grafana.com/grafana/dashboards/?search=security)

## <mark style="color:blue;">Download Elasticsearch</mark>

<mark style="color:green;">Install java package</mark>

`sudo apt install openjdk-11-jdk`

<mark style="color:green;">Now clone a repository</mark>

`wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.14.0-amd64.deb`

<mark style="color:green;">To dpkg the file try:</mark>

`sudo dpkg -i elasticsearch-7.14.0-amd64.deb`

`sudo systemctl enable elasticsearch`

`sudo systemctl start elasticsearch`&#x20;

<mark style="color:green;">To edit the config file:</mark>

`sudo nano /etc/elasticsearch/elasticsearch.yml`

[<mark style="color:purple;">https://www.elastic.co/guide/en/elasticsearch/reference/current/settings.html</mark>](https://www.elastic.co/guide/en/elasticsearch/reference/current/settings.html) and check here how you can edit the config file and run it open port 9200

<mark style="color:red;">**Note:**</mark>dont forget to access port 9200 from firewall ufw

## Now How to configure grafana alloy to access elasticsearch

## Install Grafana Alloy on Linux <a href="#install-grafana-alloy-on-linux" id="install-grafana-alloy-on-linux"></a>

You can install Alloy as a systemd service on Linux.

### Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

Some Debian-based cloud Virtual Machines don’t have GPG installed by default. To install GPG in your Linux Virtual Machine, run the following command in a terminal window.

shell![Copy code to clipboard](https://grafana.com/media/images/icons/icon-copy-small-2.svg)Copy

```shell
sudo apt install gpg
```

### Install <a href="#install" id="install"></a>

To install Alloy on Linux, run the following commands in a terminal window.

1.  Import the GPG key and add the Grafana package repository.

    debian-ubunturhel-fedorasuse-opensuse![Copy code to clipboard](https://grafana.com/media/images/icons/icon-copy-small-2.svg)Copy

    ```debian-ubuntu
    sudo mkdir -p /etc/apt/keyrings/
    wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
    echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
    ```
2.  Update the repositories.

    debian-ubunturhel-fedorasuse-opensuse![Copy code to clipboard](https://grafana.com/media/images/icons/icon-copy-small-2.svg)Copy

    ```debian-ubuntu
    sudo apt-get update
    ```
3.  Install Alloy.

    debian-ubunturhel-fedorasuse-opensuse![Copy code to clipboard](https://grafana.com/media/images/icons/icon-copy-small-2.svg)Copy

    ```debian-ubuntu
    sudo apt-get install alloy
    ```
4.  ### You have to install grafana alloy to access sevrer data and make dashboards etc...

    very simple and easy to use : [https://grafana.com/docs/alloy/latest/get-started/install/linux/](https://grafana.com/docs/alloy/latest/get-started/install/linux/)

Yes ,We can make grafana accessible to a new user and put it as a viewer mode only and this should be done using key API and using this [https://grafana.com/docs/grafana/latest/developers/http\_api/](https://grafana.com/docs/grafana/latest/developers/http\_api/)


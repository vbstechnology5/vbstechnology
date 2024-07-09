---
description: Share photos and simply AI will detect your pictures
---

# Immich server installation

Immich is an application designed to organize photos using facial and product recognition technology(AI) it's similar to google photos and can be downloaded on mobile or on a server.

Immich requires two softwares:

<mark style="color:blue;">**Docker**</mark> & <mark style="color:blue;">**Docker compose**</mark>

## <mark style="color:blue;">\[GUIDE] How to setup Immich from scratch in 10 minutes.</mark> <a href="#post-title-t3_1ccxm2c" id="post-title-t3_1ccxm2c"></a>

Setting up <mark style="color:purple;">**Immich**</mark> is very easy and better than google photos.

Requirements :4GB RAM **minimum** and a **good** GPU

### <mark style="color:blue;">Installation on ubuntu:</mark>

#### <mark style="color:green;">1.Switch to root and go to root directory</mark>

**`sudo su`**

**`cd`**

#### <mark style="color:green;">2.Run the command below</mark>

**`curl -s`** [`https://get.docker.com`](https://get.docker.com/) **`| bash`**

#### <mark style="color:green;">3. Make immich directory and go to immich directory</mark>

**`mkdir immich`**

**`cd immich`**

#### <mark style="color:green;">4. Download docker compose file</mark>

**`wget`** [`https://raw.githubusercontent.com/immich-app/immich/main/docker/docker-compose.yml`](https://raw.githubusercontent.com/immich-app/immich/main/docker/docker-compose.yml)

#### <mark style="color:green;">5. Download the pre-configuration file</mark>

**`wget`** [`https://raw.githubusercontent.com/immich-app/immich/main/docker/example.env`](https://raw.githubusercontent.com/immich-app/immich/main/docker/example.env)

#### <mark style="color:green;">6. Edit the example.env file</mark>

**`nano example.env`**

and then change "UPLOAD\_LOCATION" line to /root/immich/data/ edit using **nano**

**`UPLOAD_LOCATION=/root/immich/data/`**

#### <mark style="color:green;">7. And then save exit nano then rename example.env to .env</mark>

**`mv example.env .env`**

#### <mark style="color:green;">8. Make data directory (this is where all of your photos will be restored)</mark>

**`mkdir data`**

#### <mark style="color:green;">9. Run docker-compile to run immich</mark>

**`docker-compose up -d`**

#### <mark style="color:green;">10. You all set! access immich on your web-browser</mark>

[http://your-immich-ip:2283](http://your-immich-ip:2283/)

### How to import google photos

1. Go to Google Takeout and log-in with your google account.
2. Export photos in zip file and size of 50gb each file
3. download zip files from your email (google will send you the export file within a day)
4. download immich-go and extract the file [https://github.com/simulot/immich-go/releases/download/0.13.2/immich-go\_Linux\_x86\_64.tar.gz](https://github.com/simulot/immich-go/releases/download/0.13.2/immich-go\_Linux\_x86\_64.tar.gz)
5. after extract you will get the excutable file "immich-go"
6. run the command like the example `./immich-go -server=http://immichserver:2283 -key=zzV6k65KGLNB9mpGeri9n8Jk1VaNGHSCdoH1dY8jQ upload -create-albums -google-photos takeout-*.zip`

key is the api key that you can get it from immich setting page on webbrowser, takeout-\* is the file names you will get it from Google.

I hope this guide is helpful. If you have any questions or encounter any errors, just reply here, and I'll be glad to assist you.

## <mark style="color:green;">Install apache and certbot</mark>

`sudo apt update`\
`sudo apt install apache2 certbot python3-certbot-apache`\
`sudo a2enmod proxy`\
`sudo a2enmod proxy_http`\
`sudo a2enmod proxy_wstunnel`\
`sudo a2enmod headers`\
`sudo a2enmod rewrite`\
`sudo a2enmod ssl`

## <mark style="color:green;">Configure Apache reverse proxy</mark>

`sudo nano /etc/apache2/sites-available/mycooldomain.com.conf`\
`should have`\
`<VirtualHost *:80>`\
&#x20;   `ServerName` [`mycooldomain.com`](http://mycooldomain.com/)\
&#x20;   `ProxyPreserveHost On`\
&#x20;   `ProxyPass /` [`http://localhost`](http://localhost/)`:XXXX/`\
&#x20;   `ProxyPassReverse /` [`http://localhost`](http://localhost/)`:XXXX/`\
`</VirtualHost>`

### <mark style="color:green;">Enable the domain name and reboot apache</mark>

`sudo a2ensite mycooldomain.com`\
`sudo systemctl reload apache2sudo certbot --apache -d mycooldomain.com`

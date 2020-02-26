![](https://www.primechaintech.com/demo/img/github.jpg)

# Primechain

Installing, configuring, securing, troubleshooting, updating, and maintaining a blockchain ecosystem is a complex and time consuming task. Plus, there is a severe shortage of skilled blockchain developers. 

And that's why we have built Primechain.

**Primechain is a blockchain ecosystem that builds itself in 6 minutes (or less) with a functional web application, mobile Progressive Web App, and a REST API service.**

Table of Contents
-----------------
1. [Prerequisites](#1-prerequisites)
2. [Getting Started](#2-getting-started)
3. [Setting up ngnix and SSL](#3-setting-up-ngnix-and-ssl)
4. [Adding nodes](#4-adding-nodes)
5. [Updating Primechain](#5-updating-primechain)
6. [API Documentation](#6-api-documentation)
7. [Sandbox](#7-sandbox)
8. [Basic troubleshooting](#8-basic-troubleshooting)
9. [Obtaining third party API keys](#9-obtaining-third-party-api-keys)

## 1. Prerequisites
- To setup Primechain you need an 
  - Ubuntu 16.0.4 machine (1 GB RAM, 1 CPU) with CURL and git. 
  - The ports used are 22, 80, 443, 1410, 2512, 15590 and 61172.

**Notes:** 
- For full functionality of PWA, SSL enabed domain is needed. 
- For system generated emails (password reset etc), enter your sendgrid key.

## 2. Getting Started

Login to server / VM as a sudo or root user. Then run the following:
```
sudo git clone https://primechainuser@github.com/Primechain/primechain
<Enter-Password>
cd primechain/setup
sudo bash -e primechain_setup.sh <ip-address> <email-address>
```
***Note:***
1. Instead of the IP address you can enter the domain name above. Or after setup, go to the .env file and change the IP address or domain name.

2. The email address is the admin email address

**The setup should take about 6 minutes. Once done, you will see something like this:**
```
=============================================
ADMIN LOGIN CREDENTIALS FOR WEB APPLICATION
=============================================

#######################################################
#  Email address: info@primechain.in #
#  Password: 5Ofxy3bmMx0Z9xfelnDoHWbaGs5T2RyItZ1n4RYL #
#######################################################


===================================================
WEB APPLICATION UP AND RUNNING IN THE FOLLOWING URL
===================================================
http://example.com:1410


===================================================
API APPLICATION UP AND RUNNING IN THE FOLLOWING URL
===================================================
http://example.com:2512/api/v1/get_api_key

All your credentials are in root/primechain-api.out

```
To view mysql, mongo and multichain credentials
```
nano ~root/primechain-api.out
```

***The web application credentials can be obtained from:**
```
su primechain-user 
cd ~
cd primechain
sudo nano .env
```
***Update the below in .env, to use sendgrid for transactional emails.***
```
MAIL_SERVICE_NAME=SENDGRID
MAIL_USERNAME=<your-username>
MAIL_PASSWORD=<your-password>
```
Enter your **google** and **facebook** credentials if you want to use login through these services. If not, comment out the relevant code in `src/web/views/users/account/login.hbs`

Copy **Primechain-API** username and password if you will be using the API service.


To increase server timeout, login as root into your VM and then:
```
nano /etc/ssh/sshd_config

# Then add the following lines
ClientAliveInterval 120
ClientAliveCountMax 720
```

## 3. Setting up ngnix and SSL
Login to the VM as root and then
```
sudo ufw allow https
sudo apt install nginx
sudo nano /etc/nginx/sites-available/default

# Uncomment the following in SSL configuration

listen 443 ssl default_server;
listen [::]:443 ssl default_server;

# Add the following to the location part of the server block
# Add www.yourdomain.com only if you have made suitable A record entry in DNS

    server_name yourdomain.com www.yourdomain.com;

    location / {
        proxy_pass http://<ip-address>:1410; #whatever port your app runs on
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

# Check NGINX config
sudo nginx -t

# Restart NGINX
sudo service nginx restart

sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com

# Only valid for 90 days, test the renewal process with
certbot renew --dry-run

```
Now visit https://yourdomain.com and you should see your web app.

## 4. Adding nodes
You can connect multiple non-seed nodes to a running Primechain blockchain. Login to a newly created Ubuntu VM that you will use as a non-seed node.
```
sudo git clone https://primechainuser@github.com/Primechain/primechain_nodes
cd primechain_nodes
sudo bash -e primechain_nodes_setup.sh <primechain-seed-node-ip>
```
This does 
1. hardens the operating system, 
2. sets up the blockchain on the non-seed server

After some time, you will something like the following:

```
-------------------------------------------
INITIATING CONNECTION TO BLOCKCHAIN.....
-------------------------------------------

MultiChain 2.0.3 Daemon (latest protocol 20011)

Starting up node...

Retrieving blockchain parameters from the seed node 52.172.139.41:61172 ...
Blockchain successfully initialized.

Please ask blockchain admin or user having activate permission to let you connect and/or transact:
multichain-cli primechain grant 1aMiTQjLXABeoKtVgBcr5zzVXtT1eRvRTfY3RJ connect
multichain-cli primechain grant 1aMiTQjLXABeoKtVgBcr5zzVXtT1eRvRTfY3RJ connect,send,receive

GRANT PERMISSION FROM THE PRIMECHAIN MASTER NODE AND TYPE yes TO CONTINUE...
```

Login to the Primechain Seed Node and run the following:
```
su primechain-user
cd ~
multichain-cli primechain grant 1aMiTQjLXABeoKtVgBcr5zzVXtT1eRvRTfY3RJ connect
```
**Note:** Don't forget to use the correct address in "multichain-cli primechain grant" above.

After this, you will see a transaction ID like `fae186b309cf396f3a5e0f8dddbdb7d9ab4dd8bd08c69d5b044be19f030f4fc7`

Wait a few seconds and then go to the command line of the non-seed node and type ```yes``` and press Enter.
Few seconds later, you will see something like this:
```
----------------------------------------
BLOCKCHAIN SUCCESSFULLY SET UP!
----------------------------------------
--------------------------------------------
PRIMECHAIN NODE CREDENTIALS
--------------------------------------------
rpcuser=Tzt5COtBS9jeADGOlwMRj9akTyWlCgkZDcqgkQZc
rpcpassword=fzQQMIMJa1mKlDfghQPBQ0uYxgmqdLaYHqjYtTxe

========================================
SET UP COMPLETED SUCCESSFULLY!
========================================
```
Note down the rpcuser and rpcpassword. 

## 5. Updating Primechain 

Login to the server / VM as a sudo or root user.

```
sudo su primechain-user 
cd ~
cd primechain

# Then one of the following
sudo git pull
sudo git pull && pm2 restart 1
sudo git pull && npm i && pm2 restart 1
```

## 6. API Documentation
[https://www.primechaintech.com/documentation](https://www.primechaintech.com/documentation)

## 7. Sandbox
[https://primechainsandbox.com](https://primechainsandbox.com)

## 8. Basic troubleshooting
***Stop / start multichain***
Login to the server / VM as a sudo or root user.
For ***stopping*** multichain:
```
sudo su primechain-user 
cd ~
multichain-cli Primechain stop
```
For ***starting*** multichain:
```
sudo su primechain-user 
cd ~
multichaind Primechain --daemon
```

## 9. Obtaining third party API keys

To use any of the included APIs or OAuth authentication methods, you
will need
to obtain appropriate credentials: Client ID, Client Secret, API Key, or
Username & Password. You will need to go through each provider to
generate new
credentials.

<img
src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Google_2015_logo.svg/1000px-Google_2015_logo.svg.png"
width="200">

- Visit <a href="https://cloud.google.com/console/project"
target="_blank">Google Cloud Console</a>
- Click on the **Create Project** button
- Enter *Project Name*, then click on **Create** button
- Then click on *APIs & auth* in the sidebar and select *API* tab
- Click on **Google+ API** under *Social APIs*, then click **Enable
API**
- Click on **Google Drive API** under *G Suite*, then click **Enable
API**
- Click on **Google Sheets API** under *G Suite*, then click **Enable
API**
- Next, under *APIs & auth* in the sidebar click on *Credentials* tab
- Click on **Create new Client ID** button
- Select *Web Application* and click on **Configure Consent Screen**
- Fill out the required fields then click on **Save**
- In the *Create Client ID* modal dialog:
  - **Application Type**: Web Application
  - **Authorized Javascript origins**: http://localhost:8080
  - **Authorized redirect URI**:
http://localhost:8080/auth/google/callback
- Click on **Create Client ID** button
- Copy and paste *Client ID* and *Client secret* keys into `.env`

**Note:** When you ready to deploy to production don't forget to
add your new URL to *Authorized Javascript origins* and *Authorized
redirect URI*,
e.g. `http://my-awesome-app.herokuapp.com` and
`http://my-awesome-app.herokuapp.com/auth/google/callback` respectively.
The same goes for other providers.

<hr>

<img
src="https://en.facebookbrand.com/wp-content/uploads/2019/04/f_logo_RGB-Hex-Blue_512.png"
width="90">

- Visit <a href="https://developers.facebook.com/"
target="_blank">Facebook Developers</a>
- Click **My Apps**, then select **Add a New App* from the dropdown menu
- Enter a new name for your app
- Click on the **Create App ID** button
- Find the Facebook Login Product and click on **Facebook Login**
- Instead of going through their Quickstart, click on **Settings** for
your app in the top left corner
- Copy and paste *App ID* and *App Secret* keys into `.env`
- **Note:** *App ID* is **FACEBOOK_ID**, *App Secret* is
**FACEBOOK_SECRET** in `.env`
- Enter `localhost` under *App Domains*
- Choose a **Category** that best describes your app
- Click on **+ Add Platform** and select **Website**
- Enter `http://localhost:8080` under *Site URL*
- Click on the *Settings* tab in the left nav under Facebook Login
- Enter `http://localhost:8080/auth/facebook/callback` under Valid OAuth
redirect URIs

**Note:** After a successful sign in with Facebook, a user will be
redirected back to the home page with appended hash `#_=_` in the URL.
It is *not* a bug. See this [Stack
Overflow](https://stackoverflow.com/questions/7131909/facebook-callback-appends-to-return-url)
discussion for ways to handle it.

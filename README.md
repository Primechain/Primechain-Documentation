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
  * 8.1 [Stopping multichain](#81-stopping-multichain)
  * 8.2 [Starting multichain](#82-starting-multichain)
  * 8.3 [Third party credentials](#83-third-party-credentials)
  * 8.4 [MYSQL, MongoDB and API credentials](#84-mysql-mongodb-and-api-credentials)
  * 8.5 [Increase server timeout](#85-increase-server-timeout)
  
9. [Obtaining third party API keys](#9-obtaining-third-party-api-keys)

10. [MongoDB Compass](#10-mongodb-compass)

## 1. Prerequisites
- To setup Primechain you need: 
  - Ubuntu 16.0.4 machine (1 GB RAM, 1 CPU) with CURL and git. 
  - The ports used are 22, 80, 443, 1410, 2512, 15590 and 61172.

**Notes:** 
- For full functionality of PWA, SSL enabed domain is needed. 
- For system generated emails (password reset etc), register for Sendgrid.

## 2. Getting Started

Login to server / VM as a sudo or root user. Then run the following:
```
sudo git clone https://primechainuser@github.com/Primechain/primechain
<Enter-Password>
cd primechain/setup
sudo bash -e primechain_setup.sh <ip-address or domain-name> <email-address>
```
***Note:***
1. If required, after setup, go to the .env file and change the IP address or domain name.
```
sudo su primechain-user
cd ~
cd primechain
sudo nano .env
```

2. The email address is the admin email address. This can be changed later from mongodb in the `users` collection.

**The setup should take about 6 minutes. Once done, you will see something like this:**
```
=============================================
ADMIN LOGIN CREDENTIALS FOR WEB APPLICATION
=============================================

#######################################################
#  Email address: info@example.com #
#  Password: 5Ofxy3bmMx0Z9xf22lnDoHWbaGs5T2RyItZ1n4RY #
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
Access your IP address or domain in a browser to see:

![Primechain Login Screen](https://www.primechaintech.com/images/primechain_login_screen.jpg)

***Note:*** You need to add the port number e.g. `http://64.227.17.54:1410/login`

Login using the admin email and password to see this:

![Primechain Main Screen](https://www.primechaintech.com/images/primechain_main_screen.jpg)

Cick on the buddha icon on the top right and change your password.

## 3. Setting up ngnix and SSL
Login to the VM as root and then
```
sudo ufw allow https
sudo apt install nginx
sudo nano /etc/nginx/sites-available/default
```
Uncomment the following in SSL configuration
```
listen 443 ssl default_server;
listen [::]:443 ssl default_server;
```

Replace the following to the location part of the server block.

Add www.yourdomain.com only if you have made suitable A record entry in DNS
```
    server_name example.com.com www.example.com;

    location / {
        proxy_pass http://<ip-address>:1410; #whatever port your app runs on
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
```
Check NGINX config
```
sudo nginx -t
```
Restart NGINX
```
sudo service nginx restart
```
Then add certificate from LetsEncrypt. 
***Note:*** You need a domain name to get an SSL certificate. It will not work on an IP address. 
```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx
sudo certbot --nginx -d example.com -d www.example.com
```
Only valid for 90 days, test the renewal process with
```
certbot renew --dry-run
```
Now visit https://example.com and you should see your web app.

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
rpcuser=Tzt5COtBS9jeADGOlwMbg9akTyWlCgkZDcqgkQZc
rpcpassword=fzQQMITyu1mKlDfghQPBQ0uYxgmqdLaYHqjYtTxe

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
Note:
1. If you have only made hbs changes, then use `sudo git pull`
2. If you have made changes to js, then use `sudo git pull && pm2 restart 1`
1. If you need to update node modules then use `sudo git pull && npm i && pm2 restart 1`

## 6. API Documentation
[https://www.primechaintech.com/documentation](https://www.primechaintech.com/documentation)

## 7. Sandbox
[https://primechainsandbox.com](https://primechainsandbox.com)

## 8. Basic troubleshooting

  #### 8.1 Stopping multichain
Login to the server / VM as a sudo or root user.
```
sudo su primechain-user 
cd ~
multichain-cli Primechain stop
```
  #### 8.2 Starting multichain
  Login to the server / VM as a sudo or root user.
```
sudo su primechain-user 
cd ~
multichaind Primechain --daemon
```

  #### 8.3 Third party credentials
  
Login to the VM as root and then:
```
su primechain-user 
cd ~
cd primechain
sudo nano .env
```
You will be able to access the following credentials:

***APPLICATION INFORMATION***
```
NODE_ENV=production
APPLICATION_HOSTNAME=example.com
APPLICATION_PORT=1410
APPLICATION_NAME=primechain
APPLICATION_SESSION_SECRET=<value>
```
***SENDGRID***
```
MAIL_SERVICE_NAME=SENDGRID
MAIL_USERNAME=<usernam>
MAIL_PASSWORD=<password>
```
***GOOGLE API CREDENTIALS***
```
GOOGLE_ID=<client id>
GOOGLE_SECRET=<client secret>
```
***FACEBOOK API CREDENTIALS***
```
FACEBOOK_ID=<App ID>
FACEBOOK_SECRET=<App Secret>
```
***MONGODB DATABASE PATH***
```
MONGODB_URI=mongodb://primechainuser:<password>@localhost:27$
```
***PRIMECHAIN-API INFORMATION***
```
PRIMECHAIN_USERNAME=<username>
PRIMECHAIN_PASSWORD=<password>
PRIMECHAIN_API_URN=example.com
PRIMECHAIN_API_PORT=2512
```

### 8.4 MYSQL, MongoDB and API credentials
Login to the VM as root and then:
```
sudo nano primechain-api.out
```
You will be able to access the following credentials:
***MYSQL DATABASE CREDENTIALS***
```
dbrootpass=<root-password>
dbuser=<username>
dbpass=<password>
```

***API CREDENTIALS***
```
rpcuser=<username>
rpcpassword=<password>
```

***MONGODB DATABASE CREDENTIALS***
```
dbname= primechain
dbuser= <username>
dbpass=<password>
```
### 8.5 Increase server timeout
To increase server timeout, login as root into your VM and then:
```
nano /etc/ssh/sshd_config

# Then add the following lines
ClientAliveInterval 120
ClientAliveCountMax 720
```

## 9. Obtaining third party API keys

### 9.1 Google

1. Visit <a href="https://cloud.google.com/console/project" target="_blank">Google Cloud Console</a>.

2. Click on the **Create Project** button.

3. Enter *Project Name*, then click on **Create** button.

4. Click on ***APIs & services*** in the top navigation bar.

5. Click on ***OAuth consent screen***. Choose user type as ***External*** and click on ***Create***. Fill the relevant details and click on ***Save***.

6. Click on ***Credentials***.

7. Click on ***Credentials --> Create credentials --> OAuth client ID***. 
- Choose ***Application type*** as ***Web Application***.
- Choose ***Authorized Javascript origins*** as something like https://example.com
- Choose ***Authorized redirect URI*** as http://example.com/auth/google/callback

8. Copy your client ID and client secret.

9. Login to the VM as root and update the below in .env:
```
GOOGLE_ID=<client id>
GOOGLE_SECRET=<client secret>
```
9. Restart pm2 `pm2 restart 1`
<hr>

### 9.2 Facebook

1. Visit <a href="https://developers.facebook.com/" target="_blank">Facebook Developers</a>

2. Click **My Apps**, then select ***Add a New App*** from the dropdown menu

3. Click on ***Setup*** in ***Facebook Login*** box.

4. Click on ***WWW***

5. Enter Site URL as https://example.com and click on Save

6. Click on ***Settings --> Basic*** and copy App ID and App Secret. Add App domain as https://example.com. Click on Save changes

7. In Products -- Facebook Login --> Settings, in Valid OAuth Redirect URIs, enter https://example.com/auth/facebook/callback

8. Login to the VM as root and update the below in .env:
```
FACEBOOK_ID=<App ID>
FACEBOOK_SECRET=<App Secret>
```

9. Restart pm2 `pm2 restart 1`

**Note:** After a successful sign in with Facebook, a user will be redirected back to the home page with appended hash `#_=_` in the URL. It is *not* a bug. See this [Stack Overflow](https://stackoverflow.com/questions/7131909/facebook-callback-appends-to-return-url) discussion for ways to handle it.

## 10. MongoDB Compass

Login as root to the VM and then run:
```
sudo nano /etc/mongod.conf
```
Edit `bindIp: 127.0.0.1` to `bindIp: 127.0.0.1,<ip-address>`

Then:
```
sudo service mongod restart
sudo service mongod status
```
Download Mongo Compass Community edition and create a new connection.
![Mongo Compass Community](https://www.primechaintech.com/images/mongo_connection.jpg)

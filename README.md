![](https://www.primechaintech.com/demo/img/github.jpg)

# Primechain

Installing, configuring, securing, troubleshooting, updating, and maintaining a blockchain ecosystem is a complex and time consuming task. Plus, there is a severe shortage of skilled blockchain developers. 

And that's why we have built Primechain.

**Primechain is a blockchain ecosystem that builds itself in 6 minutes (or less) with a functional web application, mobile Progressive Web App, and a REST API service.**

Table of Contents
-----------------
1. [Prerequisites](#1-prerequisites)
2. [Getting Started](#2-getting-started)
3. [Updating Primechain](#3-updating-primechain)

## 1. Prerequisites
- To setup Primechain you need an 
  - Ubuntu 16.0.4 machine (1 GB RAM, 1 CPU) with CURL and git. 
  - The ports used are 22, 80, 1410, 2512, 15590 and 61172.

**Notes:** 
- For full functionality of PWA, SSL enabed domain is needed. 
- For system generated emails (password reset etc), enter your sendgrid key.

## 2. Getting Started

Login to server / VM as a sudo or root user. Then run the following:
```
sudo git clone https://primechainuser@github.com/Primechain/primechain
<Enter-Password>
cd primechain/setup
sudo bash -e primechain_setup.sh <ip-address>
```
**The setup should take about 6 minutes. Once it is setup, all the credentials can be obtained from:**
```
su primechain-user 
cd ~
cd primechain
sudo nano .env
```
Update the below in .env, to use sendgrid for transactional emails.
```
MAIL_SERVICE_NAME=SENDGRID
MAIL_USERNAME=<your-username>
MAIL_PASSWORD=<your-password>
```
2. Copy Primechain-API username and password if you will be using the API service.

## 3. Updating Primechain 

Login to the server / VM as a sudo or root user.

```
sudo su primechain-user 
cd ~
cd primechain
git pull && pm2 restart bin/www
```

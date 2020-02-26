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
4. [API Documentation](#4-api-documentation)
5. [Sandbox](#5-sandbox)

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

**The setup should take about 6 minutes. You will see something like this:
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

The web application credentials can be obtained from:**
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
Enter your **google** and **facebook** credentials if you want to use login through these services. If not, comment out the relevant code in `src/web/views/users/account/login.hbs`

Copy **Primechain-API** username and password if you will be using the API service.


To increase server timeout, login as root into your VM and then:
```
nano /etc/ssh/sshd_config

# Then add the following lines
ClientAliveInterval 120
ClientAliveCountMax 720
```


Obtaining API Keys
------------------

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


## 3. Updating Primechain 

Login to the server / VM as a sudo or root user.

```
sudo su primechain-user 
cd ~
cd primechain
git pull && pm2 restart bin/www
```
## 4. API Documentation
[https://www.primechaintech.com/documentation](https://www.primechaintech.com/documentation)

## 5. Sandbox
[https://primechainsandbox.com](https://primechainsandbox.com)

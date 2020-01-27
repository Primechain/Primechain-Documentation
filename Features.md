![](https://www.primechaintech.com/demo/img/github.jpg)

# Primechain


Installing, configuring, securing, troubleshooting, updating, and maintaining a blockchain ecosystem is a complex and time consuming task. Plus, there is a severe shortage of skilled blockchain developers. 

And that's why we have built Primechain.

Table of Contents
-----------------

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [List of Packages](#list-of-packages)
- [Updating Primechain](#updating-primechain)
- [Create a plugin](#create-a-plugin)
- [Components](#components)

Features
--------

- **Local Authentication** using Email and Password
- Flash notifications
- MVC Project Structure
- Bootstrap 4 + Extra Themes
- Contact Form (powered by Mailgun, Sendgrid or Mandrill)
- **Account Management**
 - Gravatar
 - Profile Details
 - Change Password
 - Forgot Password
 - Reset Password

Prerequisites
-------------

**Primechain is a blockchain ecosystem that builds itself in 6 minutes (or less) with a functional web application, mobile Progressive Web App, and an API service.**

- To setup Primechain you need an 
  - Ubuntu 16.0.4 machine (1 GB RAM, 1 CPU) with CURL and git. 
  - The ports used are 22, 80, 1410, 2512, 15590 and 61172.

**Notes:** 
- For full functionality of PWA, SSL enabed domain is needed. 
- For system generated emails (password reset etc), enter your sendgrid key.

Getting Started
---------------

Login to server / VM as a sudo or root user. Then run the following:
```
sudo git clone https://primechainuser@github.com/Primechain/primechain
9q0W4gcSDGbjkBGUKWHk0MytFPDWcDLcUBZOI1yc
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


Project Structure
-----------------

| Name                                                                 | Description                                                  |
| -------------------------------------------------------------------- | ------------------------------------------------------------ |
| **bin**/www                                                          | Starting point for running application.                      |
| **config**/passport.js                                               | Passport Local and OAuth strategies, plus login middleware.  |
| **functions**/**components**/**blockchain**/information.js           | Blockchain information                                       |
| **functions**/**components**/**blockchain**/parameters.js            | Blockchain parameters                                        |
| **functions**/**components**/**blockchain**/runtime-parameters.js    | Blockchain runtime parameters                                |
| **functions**/**components**/**data**/download-decrypt.js            | Download decrypted data from blockchain                      |
| **functions**/**components**/**data**/download.js                    | Download plain data from blockchain                          |
| **functions**/**components**/**data**/publish-encrypt.js             | Publish encrypt data to blockchain                           |
| **functions**/**components**/**data**/publish.js                     | Publish plain data to blockchain                             |
| **functions**/**components**/**data-channels**/create.js             | Create a new data channel in blockchain                      |
| **functions**/**components**/**data-channels**/grant.js              | Grant permission to write in data channel                    |
| **functions**/**components**/**data-channels**/read.js               | Read all data channels in blockchain                         |
| **functions**/**components**/**data-channels**/revoke.js             | Revoke permission to write in data channel                   |
| **functions**/**components**/**esignature**/create.js                | Create an electronic signature                               |
| **functions**/**components**/**esignature**/verify.js                | Verify an electronic signature                               |
| **functions**/**components**/**file**/publish-encrypt.js             | Publish a file to the blockchain                             |
| **functions**/**components**/**file**/download-decrypt.js            | Download a file from the blockchain                          |
| **functions**/**components**/**permissions**/index.js                | Loading file for the permissions                             |
| **functions**/**components**/**permissions**/list.js                 | List all current entity blockchain permissions               |
| **functions**/**components**/**permissions**/manage.js               | Manaage current entity blockchain permissions                |
| **functions**/**components**/**sam**/create.js                       | Create an asset in the blockchain                            |
| **functions**/**components**/**sam**/list.js                         | View my assets in the blockchain                             |
| **functions**/**components**/**sam**/send.js                         | Send an asset in the blockchain                              |
| **functions**/**components**/**sam**/send.js                         | Send an asset in the blockchain                              |
| **functions**/**components**/**wizard**/index.js                     | CRUD functionalites wizard                                   |
| **functions**/**plugins**/**dave**/index.js                          | Loading point for the folder                                 |
| **functions**/**plugins**/**dave**/bank-guarantee.js                 | Functionalities of the bank guarantee                        |
| **functions**/**users**/**account**/**activity-logs**/index.js       | Activity-log functions                                       |
| **functions**/**users**/**account**/**change-password**/index.js     | Change password functions                                    |
| **functions**/**users**/**login**/index.js                           | Login functions                                              |
| **functions**/**users**/**lost-password**/index.js                   | Lost password functions                                      |
| **functions**/**users**/**reset-password**/index.js                  | Reset password functions                                     |
| **functions**/**users**/**signup**/index.js                          | Signup functions                                             |
| **helpers**/common.js                                                | Common functions                                             |
| **helpers**/mailer.js                                                | Mailer functions                                             |
| **helpers**/request.js                                               | Request functions                                            |
| **setup**/application.sh                                             | Application shell file                                       |
| **setup**/primechain_setup.sh                                        | Primechain setup shell file                                  |
| **setup**/primechain-api.conf                                        | Primechain configuration file                                |
| **setup**/primechain-api.sql                                         | Primechain SQL fiile                                         |
| **setup**/ubuntu_core.sh                                             | Ubuntu Core setup file                                       |
| **setup**/ubuntu_create_linux_admin_user.sh                          | Ubuntu create user                                           |
| **setup**/ubuntu_hardening.sh                                        | Ubuntu hardnening shell file                                 |
| **setup**/ubuntu_mongodb_setup.sh                                    | Ubuntu mongodb setup                                         |
| **setup**/ubuntu_mysql_setup.sh                                      | Ubuntu mysql setup                                           |
| **setup**/ubuntu_ngnix_setup.sh                                      | Ubuntu ngnix setup                                           |
| **setup**/ubuntu_nodejs.sh                                           | Ubuntu nodejs setup                                          |
| **setup**/ubuntu_nodejs.sh                                           | Ubuntu nodejs setup                                          |
| **src**/primechain-api                                               | Source code for API                                          |
| **src**/web                                                          | Source code for web application                              |
| .gitignore                                                           | Folder and files ignored by git.                             |
| app.js                                                               | The main application file.                                   |
| package.json                                                         | NPM dependencies                                             |

**Note:** There is no preference how you name or structure your views.
You could place all your templates in a top-level `views` directory without
having a nested folder structure, if that makes things easier for you.
Just don't forget to update `extends ../layout`  and corresponding
`res.render()` paths in controllers.

List of Packages
----------------

| Package                         | Description                                                             |
| ------------------------------- | ------------------------------------------------------------------------|
| bcrypt                          | Library for hashing and salting user passwords.                         |
| body-parser                     | Node.js body parsing middleware.                                        |
| compression                     | Node.js compression middleware.                                         |
| connect-mongo                   | MongoDB session store for Express.                                      |
| dotenv                          | Loads environment variables from .env file.                             |
| errorhandler                    | Development-only error handler middleware.                              |
| express                         | Node.js web framework.                                                  |
| express-flash                   | Provides flash messages for Express.                                    |
| express-session                 | Simple session middleware for Express.                                  |
| mongoose                        | MongoDB ODM.                                                            |
| morgan                          | HTTP request logger middleware for node.js.                             |
| nodemailer                      | Node.js library for sending emails.                                     |
| passport                        | Simple and elegant authentication library for node.js.                  |
| passport-local                  | Sign-in with Username and Password plugin.                              |
| request                         | Simplified HTTP request library.                                        |
| validator                       | A library of string validators and sanitizers.                          |


Updating Primechain 
-------------------

Login to the server / VM as a sudo or root user.

```
sudo su primechain-user 
cd ~
cd primechain
git pull && pm2 restart bin/www
```

Create a plugin
----------------

1. Create a data channel
2. Create a xxx file
3. Create a xxx file
4. Create a xxx file
5. Modify the main menu
6. Modify the mobile menu

Components
----------


| Component         | Description                                               | Links |
| ----------------- | ----------------------------------------------------------|--|
| Node.js®          | JavaScript runtime built on Chrome's V8 JavaScript engine | [[Source]](https://github.com/nodejs)  [[License]](https://github.com/nodejs/node/blob/master/LICENSE)|
| Express               | A web framework for Node.js        | [[Source]](aaaa)  [[License]](aaaa) |
| MySQL               | Open source database        | [[Source]](aaaa)  [[License]](aaaa) |
| MongoDB               | General purpose, document-based, distributed database.        | [[Source]](aaaa)  [[License]](aaaa) |
| Multichain               | Open source blockchain platform.        | [[Source]](aaaa)  [[License]](aaaa) |
| Primechain® REST API               | Representational State Transfer Application Programming Interface        | [[Source]](aaaa)  [[License]](aaaa) |
| Primechain® web application               | Blockchain connected web application built using HTML5 and bootstrap.        | [[Source]](aaaa)  [[License]](aaaa) |
| Primechain® PWA               | Progressive web application built using bootstrap.        | [[Source]](aaaa)  [[License]](aaaa) |


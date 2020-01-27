# Project Structure

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

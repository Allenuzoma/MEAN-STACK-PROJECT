# MEAN-STACK-PROJECT


**Step 2: Install MongoDB**
 The first step is to install the prerequisite packages needed during the installation. To do so, run the following command.
 sudo apt install software-properties-common gnupg apt-transport-https ca-certificates -y

* To install the most recent MongoDB package, you need to add the MongoDB package repository to your sources list file on Ubuntu. Before that, you need to import the public key for MongoDB 
 on your system using the wget command as follows:

      curl -fsSL https://pgp.mongodb.com/server-7.0.asc |  sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor

+ Next, add MongoDB 7.0 APT repository to the /etc/apt/sources.list.d directory:
           echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee 
           /etc/apt/sources.list.d/mongodb-org-7.0.list

+ The command adds the MongoDB 7.0 sources list file to the /etc/apt/sources.list.d/ directory. This file contains a single line that reads:
         echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse"

+  Once the repository is added, reload the local package index:
         sudo apt update

  The command refreshes the local repositories and makes Ubuntu aware of the newly added MongoDB 7.0 repository.
+ With that out of the way, install the mongodb-org meta-package that provides MongoDB:
          sudo apt install mongodb-org -y

+ The command installs the MongoDB database server along with the database core components including the shell tools. Once the installation is complete, verify the version of MongoDB 
  installed:

           mongod --version

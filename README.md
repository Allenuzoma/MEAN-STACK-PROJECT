# MEAN-STACK-PROJECT


**Step 2: Install MongoDB**

1. Installation of MongoDB

   
Due to the fact I tried havin issues with my MongoDB 6.0 installation as seen below, I had to install an updated version of MongoDB 7


![issues with mongo db 6 0](https://github.com/user-attachments/assets/9035c43f-db95-41d6-83de-487aecace2e9)


 The first step is to install the prerequisite packages needed during the installation. To do so, run the following command.
 sudo apt install software-properties-common gnupg apt-transport-https ca-certificates -y

 ![install sofware properties ang gnupg](https://github.com/user-attachments/assets/1f786bec-d2cd-49b3-bc3b-65650e22f0ea)


* To install the most recent MongoDB package, you need to add the MongoDB package repository to your sources list file on Ubuntu. Before that, you need to import the public key for MongoDB 
 on your system using the wget command as follows:

             curl -fsSL https://pgp.mongodb.com/server-7.0.asc |  sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor

  ![install sofware properties ang gnupg](https://github.com/user-attachments/assets/77a56c14-074d-4612-b5fa-9b491f9a394e)


+ Next, add MongoDB 7.0 APT repository to the /etc/apt/sources.list.d directory:

  
              echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee 
                /etc/apt/sources.list.d/mongodb-org-7.0.list

  ![adding mongodb sources list file to the etc directory](https://github.com/user-attachments/assets/dc7b3a50-19ed-4178-897b-c7a5c0fd918c)


  The command adds the MongoDB 7.0 sources list file to the /etc/apt/sources.list.d/ directory. This file contains a single line that reads:

  
         echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse"

+  Once the repository is added, reload the local package index:

         sudo apt update

   ![sudo apt update after creating source list file](https://github.com/user-attachments/assets/5a82d946-32c7-481c-9f2e-b28e3e6a27e0)


 The command refreshes the local repositories and makes Ubuntu aware of 
 the newly added MongoDB 7.0 repository.

+ Now we will install the mongodb-org meta-package that provides MongoDB:

          sudo apt install -y mongodb-org


  ![installing mongodb 7](https://github.com/user-attachments/assets/c7a312de-9400-4fb6-8b87-aa5aee316cc4)


  ![installing mongodb 7    ](https://github.com/user-attachments/assets/db4af510-a11e-4b3b-99b6-a4c67f6b4e1d)

The command installs the MongoDB database server along with the 
database core components including the shell tools.

+  We can verify the version of MongoDB installed:

           mongod --version
  
![verifying my mongodb version](https://github.com/user-attachments/assets/d78f0629-0b99-4a4a-b8e7-cd4f44adb76c)

Next is to start the mongodb service and check the status using systemctl

    sudo systemctl start mongod
    
    sudo systemctl status mongod

![failure w exit code](https://github.com/user-attachments/assets/5f7e81b2-8d4d-4d20-b1f2-7664dd6d1360)

The above image showed an error message  "Failed with result 'exit-code'". I was able to resolve this with the following code:


    sudo rm -rf /tmp/mongodb-27017.sock
    sudo service mongod start

 + Verifying the status of the mongodb service   
    sudo systemctl status mongod
    


![failure w exit code resolution](https://github.com/user-attachments/assets/51c90928-b2b9-456b-863e-f472d837f738)

2. Install the Body-parser

   sudo apt install body-parser

   ![install body parser](https://github.com/user-attachments/assets/6efee89f-1443-4142-bbba-60b911ef250a)

3. Next we will create a folder named books and enter it:

    mkdir books&&cd books

4. Run the command to make the books a package:

   npm init


![npm init books](https://github.com/user-attachments/assets/f43aafe4-7c25-4e2e-be45-550008de8f00)


5. Create a file named server.js and copy the following code into it
   

var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3000);
app.listen(app.get('port'), function(){
    console.log('Server up: http://localhost:' + app.get('port'));
});
  
![server js in books dir](https://github.com/user-attachments/assets/392dc6ff-bcde-4cf2-8aa1-4bfb7d81e521)


**Install Express and set up routes to the server**

We will install express and mongoose using the command:


sudo npm install express
sudo apt install mongoose

![install express and mongoose](https://github.com/user-attachments/assets/c3fe8749-d3fb-4810-b5a5-13c0e98945d6)

# MEAN Stack Deployment to Ubuntu in AWS

The MEAN stack is a full-stack JavaScript framework consisting of MongoDB, Express.js, Angular, and Node.js. 
Here's a brief breakdown:

MongoDB: A NoSQL database for storing book data, such as title, author, genre, etc.
Express.js: A backend framework for handling server-side logic, such as APIs to add, update, or retrieve books.
Angular: A frontend framework for building the user interface, allowing users to interact with the Book Register (e.g., adding or viewing books).
Node.js: A runtime environment for running JavaScript on the server, handling requests and connecting to the database.
For a simple Book Register, MEAN is useful because it uses a single programming language (JavaScript) across the entire stack, making development more streamlined. You can easily implement CRUD operations (Create, Read, Update, Delete) for managing books, and the stack provides scalability, especially for web apps that may expand over time.

**Step 0: Preparing Prerequisites**


1. Create and launch an EC2 instance:

   
![instance launching](https://github.com/user-attachments/assets/feed93d5-381b-4803-b95f-70e60cea2e76)



![instance details](https://github.com/user-attachments/assets/b03f755c-1635-4162-b330-0e253effd85a)


2. SSH into the instance:
 
   
 ![ssh login to the instance](https://github.com/user-attachments/assets/fca09396-0ead-4c87-9a57-551ee51b929c)






**Step 1: Install Node Js**

Node Js is a javascript runtime used to set up Express routes and AngularJs controllers
1. Update and upgrade Ubuntu using the command:


            sudo apt update && sudo apt upgrade

   

2. Add certificates using the command:


            sudo apt -y install curl dirmngr apt-transport-https lsb-release cacertificates
            curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -            

3. Install nodejs using the command:


            sudo apt install -y nodejs


**Step 2: Install MongoDB**

   Due to the issues with my MongoDB 6.0 installation as seen below, I had to install an updated version of MongoDB 7


![issues with mongo db 6 0](https://github.com/user-attachments/assets/9035c43f-db95-41d6-83de-487aecace2e9)


 1. The first step is to install the prerequisite packages needed during the installation. To do so, run the following command:



    
            sudo apt install software-properties-common gnupg apt-transport-https ca-certificates -y
    

 ![install sofware properties ang gnupg](https://github.com/user-attachments/assets/1f786bec-d2cd-49b3-bc3b-65650e22f0ea)


2.  To install the most recent MongoDB package, you need to add the MongoDB package repository to your sources list file on Ubuntu. Before that, you need to import the public key for 
    MongoDB on your system using the wget command as follows:
    


             curl -fsSL https://pgp.mongodb.com/server-7.0.asc |  sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor



     ![install sofware properties ang gnupg](https://github.com/user-attachments/assets/77a56c14-074d-4612-b5fa-9b491f9a394e)



            


4.   Next, to get the right MongoDB 7.0 APT repository and add it to the /etc/apt/sources.list.d directory:

  
              echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee 
                /etc/apt/sources.list.d/mongodb-org-7.0.list

  ![adding mongodb sources list file to the etc directory](https://github.com/user-attachments/assets/dc7b3a50-19ed-4178-897b-c7a5c0fd918c)
  


     The command adds the MongoDB 7.0 sources list file to the /etc/apt/sources.list.d/ directory. This file contains a single line that reads:

  
            echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse"

            

5.  Once the repository is added, reload the local package index:

   

         sudo apt update
    

   ![sudo apt update after creating source list file](https://github.com/user-attachments/assets/5a82d946-32c7-481c-9f2e-b28e3e6a27e0)

   


    The command refreshes the local repositories and makes Ubuntu aware of the newly added MongoDB 7.0 repository.

6. Now we will install the mongodb-org meta-package that provides MongoDB:

          sudo apt install -y mongodb-org


  ![installing mongodb 7](https://github.com/user-attachments/assets/c7a312de-9400-4fb6-8b87-aa5aee316cc4)


  ![installing mongodb 7    ](https://github.com/user-attachments/assets/db4af510-a11e-4b3b-99b6-a4c67f6b4e1d)

The command installs the MongoDB database server along with the 
database core components including the shell tools.



7.  We can verify the version of MongoDB installed:

           mongod --version

    
  
![verifying my mongodb version](https://github.com/user-attachments/assets/d78f0629-0b99-4a4a-b8e7-cd4f44adb76c)



8. Next is to start the mongodb service and check the status using systemctl:



          sudo systemctl start mongod
          
          sudo systemctl status mongod

![failure w exit code](https://github.com/user-attachments/assets/5f7e81b2-8d4d-4d20-b1f2-7664dd6d1360)




The above image showed an error message  "Failed with result 'exit-code'". I was able to resolve this with the following code:


          sudo rm -rf /tmp/mongodb-27017.sock
          sudo service mongod start

 9. Verifying the status of the mongodb service:



          sudo systemctl status mongod
    


![failure w exit code resolution](https://github.com/user-attachments/assets/51c90928-b2b9-456b-863e-f472d837f738)

10. Install the Body-parser:

    

         sudo apt install body-parser
   

   ![install body parser](https://github.com/user-attachments/assets/6efee89f-1443-4142-bbba-60b911ef250a)

   

11. Next we will create a folder named books and enter it:

    

                   mkdir books&&cd books


    

13. Run the command to make the books a package:
    

                  npm init


![npm init books](https://github.com/user-attachments/assets/f43aafe4-7c25-4e2e-be45-550008de8f00)


14. Create a file named server.js and copy the following code into it:



            const express = require('express');
            const bodyParser = require('body-parser');
            const mongoose = require('mongoose');
            const path = require('path');
            const app = express();
            const PORT = process.env.PORT || 3300;
            
            // MongoDB connection
            mongoose.connect('mongodb://localhost:27017/test', {
              useNewUrlParser: true,
              useUnifiedTopology: true,
            })
            .then(() => console.log('MongoDB connected'))
            .catch(err => console.error('MongoDB connection error:', err));
            
            // Middleware
            app.use(express.static(path.join(__dirname, 'public')));
            app.use(bodyParser.json()); // Using body-parser to parse JSON request bodies
            
            // Routes
            require('./apps/routes')(app);
            
            // Start the server
            app.listen(PORT, () => {
              console.log(`Server up: http://localhost:${PORT}`);
            });



  
![server js ](https://github.com/user-attachments/assets/ded0cbc9-bda6-438d-8300-31177e98740b)





**Step 3: Install Express and set up routes to the server**

1. We will install express and mongoose using the command:





         sudo npm install express
         sudo apt install mongoose

   

![install express and mongoose](https://github.com/user-attachments/assets/c3fe8749-d3fb-4810-b5a5-13c0e98945d6)


2. In the Books directory we will create a folder called apps:



         mkdir apps&&cd apps

   

4. in this folder called apps, we will create a routes.js file and copy the following command into it:



                        const Book = require('./models/book');
                        const path = require('path');
                        
                        // Export the Router
                        module.exports = function(app) {
                          // GET route to fetch all books
                          app.get('/book', async (req, res) => {
                            try {
                              // Retrieve all books from the database
                              const books = await Book.find();
                              // Send the books as a JSON response
                              res.json(books);
                            } catch (err) {
                              // If an error occurs, send a 500 status with an error message
                              res.status(500).json({ message: 'Error fetching books', error: err.message });
                            }
                          });
                        
                          // POST route to add a new book
                          app.post('/book', async (req, res) => {
                            try {
                              // Create a new Book instance with data from the request body
                              const book = new Book({
                                name: req.body.name,
                                isbn: req.body.isbn,
                                author: req.body.author,
                                pages: req.body.pages
                              });
                              // Save the new book to the database
                              const savedBook = await book.save();
                              // Send a 201 status with a success message and the saved book data
                              res.status(201).json({
                                message: 'Successfully added book',
                                book: savedBook
                              });
                            } catch (err) {
                              // If an error occurs, send a 400 status with an error message
                              res.status(400).json({ message: 'Error adding book', error: err.message });
                            }
                          });
                        
                        
                           // PUT route to update a book
                          app.put('/book/:isbn', async (req, res) => {
                            try {
                              // Find the book by ISBN and update it
                              const updatedBook = await Book.findOneAndUpdate(
                                { isbn: req.params.isbn },
                                {
                                  name: req.body.name,
                                  author: req.body.author,
                                  pages: req.body.pages
                                },
                                { new: true, runValidators: true }
                              );
                        
                              if (!updatedBook) {
                                return res.status(404).json({ message: 'Book not found' });
                              }
                        
                              res.json({
                                message: 'Successfully updated the book',
                                book: updatedBook
                              });
                            } catch (err) {
                              res.status(500).json({ message: 'Error updating book', error: err.message });
                            }
                          });
                        
                          // DELETE route to remove a book by ISBN
                          app.delete('/book/:isbn', async (req, res) => {
                            try {
                              // Find and delete the book with the specified ISBN
                              const result = await Book.findOneAndDelete({ isbn: req.params.isbn });
                              if (!result) {
                                // If no book is found, send a 404 status
                                return res.status(404).json({ message: 'Book not found' });
                              }
                              // If the book is successfully deleted, send a success message with the deleted book data
                              res.json({
                                message: 'Successfully deleted the book',
                                book: result
                              });
                            } catch (err) {
                              // If an error occurs, send a 500 status with an error message
                              res.status(500).json({ message: 'Error deleting book', error: err.message });
                            }
                          });
                        
                          // Catch-all route to serve the main HTML file
                          app.get('*', (req, res) => {
                            // Send the index.html file for any unmatched routes
                            res.sendFile(path.join(__dirname, '../public', 'index.html'));
                          });
                        };







 ![routes js in apps folder](https://github.com/user-attachments/assets/f52eadf1-6a39-4ff4-930c-e77a6198160c)








4. In the apps directory create a folder called models and create a file called books and copy the following code into book.js file:





                        const mongoose = require('mongoose');
                        
                        // Define the schema for the Book model
                        const bookSchema = new mongoose.Schema({
                          name: { 
                            type: String, 
                            required: true 
                          },
                          isbn: { 
                            type: String, 
                            required: true, 
                            unique: true, 
                            index: true  // Indexing isbn for faster queries
                          },
                          author: { 
                            type: String, 
                            required: true 
                          },
                          pages: { 
                            type: Number, 
                            required: true, 
                            min: 1  // Ensure the book has at least one page
                          }
                        }, {
                          timestamps: true  // Automatically add createdAt and updatedAt fields
                        });
                        
                        // Create and export the Book model
                        module.exports = mongoose.model('Book', bookSchema);




![book js in models in apps](https://github.com/user-attachments/assets/a1a28186-904f-47a1-808d-690bfd4789bb)





**Step 3: Access the routes with AngularJs**
1. Go back to the books directory and create a folder called public and create a file script.js:

   



               mkdir public&&cd public&&nano script.js



2. Copy the following code into the script.js:



                  var app = angular.module('myApp', []);
                  app.controller('myCtrl', function($scope, $http) {
                      // Fetch books from the server
                      $http({
                          method: 'GET',
                          url: '/book'
                      }).then(function successCallback(response) {
                          $scope.books = response.data;
                      }, function errorCallback(response) {
                          console.log('Error:' + response);
                      });
                  
                      // Delete a book
                      $scope.del_book = function(book) {
                          $http({
                              method: 'DELETE',
                              url: '/book/' + book.isbn // Use book.isbn directly in the URL
                          }).then(function successCallback(response) {
                              console.log(response);
                              // Optionally, refresh the book list or remove the deleted book from $scope.books
                          }, function errorCallback(response) {
                              console.log('Error:' + response);
                          });
                      };
                  
                      // Add a new book
                      $scope.add_book = function() {
                          var body = {
                              name: $scope.Name,
                              isbn: $scope.Isbn,
                              author: $scope.Author,
                              pages: $scope.Pages
                          };
                  
                          $http({
                              method: 'POST', // Changed to POST for adding a book
                              url: '/book',
                              data: body,
                              headers: {
                                  'Content-Type': 'application/json' // Set the content type to JSON
                              }
                          }).then(function successCallback(response) {
                              console.log(response);
                              // Optionally, refresh the book list or add the new book to $scope.books
                          }, function errorCallback(response) {
                              console.log('Error:' + response);
                          });
                      };
                  });

   




![script js in public in books](https://github.com/user-attachments/assets/44ea16ad-cf05-49d6-b1e8-a960fad49700)

![script js in public in books 2](https://github.com/user-attachments/assets/59a3ce41-29f7-440b-8741-fee9c657dad2)




3. In the public folder, create the index.js file and copy the code into index.js:





            <!doctype html>
            <html ng-app="myApp" ng-controller="myCtrl">
            <head>
                <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
                <script src="script.js"></script>
            </head>    
                <body>
                    <div>
                        <table>
                            <tr>
                                <td>Name:</td>
                                <td><input type="text" ng-model="Name"></td>
                            </tr>
                            <tr>
                                <td>Isbn:</td>
                                <td><input type="text" ng-model="Isbn"></td>
                            </tr>
                            <tr>
                                <td>Author:</td>
                                <td><input type="text" ng-model="Author"></td>
                            </tr>
                            <tr>
                                <td>Pages:</td>
                                <td><input type="text" ng-model="Pages"></td>
                            </tr>
                        </table>
                        <button ng-click="add_book()">Add</button>
                    </div>
                    <hr>
                    <div>
                        <table>
                            <tr>
                                <th>Name</th>
                                <th>Isbn</th>
                                <th>Author</th>
                                <th>Pages</th>
                            </tr>
            
                            <tr ng-repeat="book in books">
                                <td>{{book.name}}</td>
                                <td>{{book.isbn}}</td>
                                <td>{{book.author}}</td>
                                <td>{{book.pages}}</td>
            
                                <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
                            </tr>
                        </table>
                    </div>
                </body>
            </html>

   

![index html in public](https://github.com/user-attachments/assets/6e4d1f5d-306e-47ee-a78b-836087f57156)




Now enter the command to start the server:



         node server.js

We can now see that the server runs and can be connected on port 3300
Test the following command on a terminal to test:



         curl -s http://localhost:3300


Open port 3300 by adding an incoming rule in the security group of the EC2 instance.

Now the app can be accessed on a browser with public IP address or public DNS name.









![finishing](https://github.com/user-attachments/assets/5e24ceaf-730e-42f0-bbd3-e67e36d3a62b)











![image](https://github.com/user-attachments/assets/1ef2d29e-b9ca-4cee-bede-60d6fe2e202d)# mern-stack-implemetation


In this project, we're going to be implementing a web solution based on the MERN stack.

The MERN stack is a popular set of technologies used to build full-stack web applications. "MERN" stands for:

MongoDB: A NoSQL database used to store application data in a flexible, JSON-like format.

Express.js: A back-end web application framework for Node.js, used to build the server-side logic and handle HTTP requests/responses.

React.js: A front-end JavaScript library used to build interactive user interfaces.

Node.js: A JavaScript runtime that allows you to run JavaScript code on the server.


 ![image](https://github.com/user-attachments/assets/6a70eb00-7bc2-4086-aba0-491ff5d44246)


We'll be deploying a simple To-Do appplication

 **STEP 0: REQUIREMTNS**

 We're going to be needing an active AWS account and a virtual server UBuntu server OS.

 I'll be exploring and making use of the windows terminal.You can try it out as well.


 **STEP 1: BACKEND CONFIGURATION**

i. We start  by ensuring that our package information is up to date by running an update.

  Update Ubuntu

           sudo apt update
       

 ii.  Secondly,we're going to be upgrading Ubuntu to install the latest versions of the packages currently installed in the system.

           sudo apt upgrade

![image](https://github.com/user-attachments/assets/92c53fe3-8872-488e-aac2-b82dbce3562e)




 iii.  Now let's get the location of Nodejs software from Ubuntu repositories 

            curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -

![image](https://github.com/user-attachments/assets/a1b00c40-4380-4f69-8988-2244f1ca110e)


  iv.   Now let's Install Nodejs on our new server 

              sudo apt-get install -y nodejs

  ![image](https://github.com/user-attachments/assets/c9764b2c-9789-42dd-919d-008f43208721)


  The above command installs both Nodejs and Node Package Manager(NPM)


  verify each using the 

                node -v

                npm -v

  ![image](https://github.com/user-attachments/assets/646297a7-1dd8-4184-9f4d-4d8ae81f81cf)

  
      
  v.  Now let'setup our application architecture,we start by creating a directory...

              mkdir Todo

   We can confirm the existence of our new directory using 

              ls 

  while in our root directory where we initiated it's creation.


  now let's navigate to our Todo directory.

            cd Todo

   ![image](https://github.com/user-attachments/assets/6f1108a6-7720-406e-9bdd-670bd78d40fe)

   vi.     Next we run 

                        npm init

  This   command initializes our project and creates a package.json file that contains important details about our applicaion and dependencies that are needed for it to run successfully.


   ![image](https://github.com/user-attachments/assets/34e7eb99-e41e-4050-a544-8956fb67126d)


   vi.    Let's install express our backend framework.

             npm install express

  ![image](https://github.com/user-attachments/assets/6029b1fc-0bd2-4ef1-a822-66dfe2bca49f)

  vii.    Then we create our starting file

                touch index.js

  
  ![image](https://github.com/user-attachments/assets/fd328b1e-79a5-4d71-b10b-5e5bbb00fdb1)


  viii.   We installl the dotenv module next 

                  npm install dotenv

  ![image](https://github.com/user-attachments/assets/428cb8ff-7204-40ef-b0f2-304f7ef00910)


  ix.      Now  we'll open our index.js file  using vim

                   vim index.js

  now paste the code below,the code creates a basic Express server.It allows cross-origin requests (CORS).It listens on a port (configured via environment variables or defaults to 5000).It responds with 'Welcome to Express' to any incoming HTTP request.

    const express = require('express');
    require('dotenv').config();

    const app = express();

    const port = process.env.PORT || 5000;

    app.use((req, res, next) => {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
    next();
    });

    app.use((req, res, next) => {
    res.send('Welcome to Express');
    });

    app.listen(port, () => {
    console.log(`Server running on port ${port}`);
    });


![image](https://github.com/user-attachments/assets/6b6a8b94-86a2-4e73-844a-657c3aa58261)


x.  Our port is running on port 5000 so we need to open this port in EC2 security group by editing the inbound rules.

   Navigate to security grooups and edit inbound rules to carry out to open this port

  ![image](https://github.com/user-attachments/assets/1e7a6eaa-7331-4bc8-813c-805c59eb899c)

vi.  Then we open our brwoser and access our public IP via port 5000

  ![image](https://github.com/user-attachments/assets/fa29ccc0-c44f-4097-ac8a-219f8cf4fd59)


**ROUTES**

 In terms of the functionalities of our Todo app there  are three actions we want it to  carry out:

   1.Create a new task 

   2. Display list of all tasks

   3. Delete a completed task

  Each of the tasks will be within it's own endpoint and will use HTTP request methods(POSR,GET and DELETE)


  So we're going to be creating routes that will define the endpoint of each of these tasks,

  i. Create directory for  routes

          mkdir routes


   ii.  Navigate to the routes folder

          cd routes

  iii.   Create api.js in that folder 

            touch api.js

  iv.     Open the file and paste the code below

              vim api.js

    const express = require ('express');

    const router = express.Router();

    router.get('/todos', (req, res, next) => {
     });                                                                                                                                             
    router.post('/todos', (req, res, next) => {
    });

    router.delete('/todos/:id', (req, res, next) => {
    });

    module.exports = router;


**MODELS**

   In our Todo app, the Model defines the structure of todo items (ID, task, status), manages data operations (create, read, update, delete), enforces rules (like ensuring tasks are not empty), and handles database interactions. It ensures data integrity and maintains a clear separation between data and application logic.

   We'll use the models to define our database schema. A schema is a blueprint or structure that defines how data is organized in a database. 


   Now let's create a schema and a model.

   i.  Change diectory back to the parent folder Todo and Install mongoose, our database.

          npm install mongoose

  ![image](https://github.com/user-attachments/assets/47540981-3ffc-40b5-9957-9f82333a7154)

  ii.   Create a models directory.

              mkdir models

  iii. Navigate to Models folder and create todo.js file 

               touch todo.js

  iv.  NOw open todo.js and paste this code

            const mongoose = require('mongoose');
            const Schema = mongoose.Schema;

            // Create schema for todo
            const TodoSchema = new Schema({
            action: {
             type: String,
            required: [true, 'The todo text field is required']
            }
            });

            // Create model for todo
            const Todo = mongoose.model('todo', TodoSchema);
            
             module.exports = Todo;

![image](https://github.com/user-attachments/assets/6aebe196-b6c3-4f5b-b0f4-68f778184af1)

**ROUTES UPDATE**

  v.  Now we'll need to update our routes from the api.js to make use of our newly created models.

  ![image](https://github.com/user-attachments/assets/d603c8db-2fd6-4164-9f35-93d1e8c9d1f4)

  Navigate to routes directory and open api.js and delete the content using

        :%d

Then paste  this..

      const express = require('express');
      const router = express.Router();
      const Todo = require('../models/todo');

      router.get('/todos', (req, res, next) => {
      Todo.find({}, 'action')
        .then(data => res.json(data))
        .catch(next);
       });

       router.post('/todos', (req, res, next) => {
        if (req.body.action) {
        Todo.create(req.body)
            .then(data => res.json(data))
            .catch(next);
         } else {
        res.json({ error: "The input field is empty" });
        }
        });

       router.delete('/todos/:id', (req, res, next) => {
       Todo.findOneAndDelete({ "_id": req.params.id })
        .then(data => res.json(data))
        .catch(next);
        });

       module.exports = router;

  ![image](https://github.com/user-attachments/assets/2a4716cf-6ea3-4a1f-b762-cd7abd535f65)


**MONGOOSE DATABASE**

We'll be making use of mongoose mlab to setip our database 

i. Sign in to mongoose or follow up the sing up process here https://www.mongodb.com/products/try-free/platform/atlas-signup-from-mlab if you don't have one.

  Create cluster

![image](https://github.com/user-attachments/assets/04fcf0aa-71c0-4160-bd6c-bceb3ab5005f)





![image](https://github.com/user-attachments/assets/168c63f2-35b5-4903-8f8e-063784de4aa9)

Go to Network access so we change the setting for our database accessibility

![image](https://github.com/user-attachments/assets/aba4ef9c-5505-41b5-94be-a5822402f9bb)

![image](https://github.com/user-attachments/assets/793f0a11-4d3a-4a14-ae67-4dbfb21c07d3)

![image](https://github.com/user-attachments/assets/0fc575bd-688f-4430-93e2-5505870206c0)


Now we're going to create a .env file in our parent directory

         touch .env

  then use vim to open thile file

            vimm .env

 We then paste our connection string in the file       


  ![image](https://github.com/user-attachments/assets/2637f03a-6415-45bc-ba91-dc8e6ce2cc90)

  Now we update index.js to reflect the use of .env so our database can be accessed  and delete the content using

       :%d


Then replace with

     const express = require('express');
    const bodyParser = require('body-parser');
    const mongoose = require('mongoose');
    const routes = require('./routes/api');
     require('dotenv').config();

    const app = express();
    const port = process.env.PORT || 5000;

     mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
    .then(() => console.log(`Database connected successfully`))
    .catch(err => console.log(err));

    mongoose.Promise = global.Promise;

    app.use(bodyParser.json());
    app.use('/api', routes);

    app.listen(port, () => {
    console.log(`Server running on port ${port}`);
     });


 ![image](https://github.com/user-attachments/assets/0ad8e6f3-211d-4602-9e7f-79e216b2c8db)


Start your server 

     node index.js

![image](https://github.com/user-attachments/assets/d2c65caf-9108-47cd-a0fd-269303588b74)



**FRONTEND USING REACT**


We'll need a user interface for our application  but before we get that done,we'll be testing the endpoints of our RESTful API using POSTMAN


  1.  In our Todo directory run

            npx create-react-app client


      





          









   



    

          

             

                  

                  




              

       


           


 


 

 

 

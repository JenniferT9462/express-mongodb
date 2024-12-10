# express-mongodb

## Overview

This project demonstrates how to connect a Node.js Express application to a MongoDB Atlas database, set up a secure development environment, and deploy the application to Render. It covers creating and configuring a MongoDB Atlas cluster, setting up a simple Express server with Mongoose for database interaction, and securely managing sensitive information using environment variables. 

**Deployed Site:** <https://express-mongodb-zxyc.onrender.com/>

## Setup MongoDB Atlas
1. Create an account <https://www.mongodb.com/cloud/atlas/register>
2. Select the `free tier` and preferred cloud provider. In this case I selected `AWS`. The other options are `Google Cloud` and `Azure`. 
3. On setup it will create a `Database User` for you. You can also access this in the `Database Access` menu on the sidebar to edit or add a new user. 
4. Make sure that your IP address is active by going to the `Network Access` section in the sidebar. Here you can add IP address that can access your cluster as well. NOTE: On sign up it will automatically add your IP address for you. 
5. In the sidebar under `Database`, your new cluster will be listed. You can click on that to access the cluster dashboard. 
From there you click `Connect`. A dialog box will pop up, select `drivers` and select `Node.js`. Instructions on how to connect your cluster will appear. 
6. In the instructions it tells you to `npm install mongodb` in your project and copy your `connection string`. NOTE: In your `connection string` that is provided, you will have to replace <username><password> with your own. You can find this under `Database Access` section in the sidebar. You can also edit the password and generate a new password. 
- Example `connection string`: 

    ```bash
   mongodb+srv://<username>:<password>@cluster0.x3zgp.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0


## Connect to MongoDB from Express

1. Project setup:
    - In your terminal make sure you `cd` into the directory that you want your project to go.
    - Make a new directory for your project:
        ```bash
        mkdir express-mongodb
    - Go into that directory:
        ```bash
        cd express-mongodb
    - Initialize `Node.js`:
        ```bash
        npm init -y
    - Install dependencies for the project:
        ```bash
        npm install express mongoose dotenv
    - Open your new project in VSCode:
        ```bash
        code .
2. Create a Server:
    - Create a file named `index.js`.
    - Copy this server code into `index.js`:
        ```js
        // index.js
        const express = require('express');
        const mongoose = require('mongoose');
        const app = express();
        const port = 3000;

        // Middleware to parse JSON bodies
        app.use(express.json());

        // Connect to MongoDB
        mongoose
        .connect('your-mongodb-connection-string-here', {
            useNewUrlParser: true,
            useUnifiedTopology: true,
        })
        .then(() => {
            console.log('Connected to MongoDB');
        })
        .catch((error) => {
            console.error('Error connecting to MongoDB:', error);
        });

        app.get('/', (req, res) => {
            res.send('Hello, World!');
        });

        app.listen(port, () => {
            console.log(`Server is running at http://localhost:${port}`);
        });
3. In the code where it says `your-mongodb-connection-string-here`, replace that with your connection string. NOTE: Make sure your `username`:`password` are correct in your `connection string`. 
4. Test the Connection:
    - Start the server by running `node index.js`
    - In the terminal it should log `Connected to MongoDB`.
    - Navigate to `localhost/3000` in your browser to confirm that the page displays `Hello World!`. 
5. Now that we know that our connection to MongoDB is working. In order to keep our `connection string` secure, we will need to store it in a `.env` file. 
    - Create a file named `.env`.
    - Define a variable for your `connection string`. NOTE: Your variable should be in all caps and no spaces between the variable and `=` or after.
        Example: `ATLAS_URL=mongodb+srv://<username>:<password>@cluster0.x3zgp.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0`
    - In order for us to use the environment variable in our server, we will need to import an configure dotenv:
        ```js
        require('dotenv').config()
    - We can add a console.log to confirm it is working, but remove after confirming:
        ```js
        console.log(process.env)
    - To use the environment variable in the server code, you can store it in a new variable to use it where you originally put your `connection string`:
        ```js
        const atlasUrl = process.env.ATLAS_URL
    - Replace your `connection string` with the new variable. 

## Deploying your Project
- You should install `nodemon` to your project in VSCode before starting the deployment process. NOTE: When creating a new project on render it looks for `Build Commands` and `Start Commands`. I noticed when I have `nodemon` installed these sections are auto filled and I do not run into any errors when deploying. 
- Create a new `Web Service` on <https://render.com/>
- Next to the `Manual Deploy` dropdown there is a `Connect` dropdown, click on it and a box with a list of `IP` addresses will appear.
- You will have to add them individually to your `MongoDB` cluster. You can do this by going to `Network Access` and then `Add a IP address`. 
- Also make sure that your `IP` address is active on that list as well. 
- In settings of your new `Web Service` look for `Environment Variables` and add your `Connection String` and save. 
- Now you should be able to deploy your project. 

## Initializing a Github Repository
- In your bash terminal, add a `.gitignore`:
    ```bash
    touch .gitignore
- Include `node_modules` and `.env`:
    ```bash
    echo "node_modules/" >> .gitignore
    echo ".env" >> .gitignore
- Create a new repository on Github, without a README.md or .gitignore.
- Back in bash initialize a empty repo:
    ```bash
    git init
- Add files to be staged for commit:
    ```bash
    git add .
- Initial commit:
    ```bash
    git commit -m "initial commit"
- Add a main branch:
    ```bash
    git branch -M main
- Add your new repository:
    ```bash
    git remote add origin https://github.com/username/reponame.git
- Push to Github
    ```bash
    git push -u origin main

## Conclusion

By following this guide, developers can quickly set up a MongoDB Atlas-connected Express application, deploy it to the web, and securely manage their development environment.
    
## Acknowledgments
- `dotenv` NPM - <https://www.npmjs.com/package/dotenv>
- MongoDB Docs - <https://www.mongodb.com/docs/atlas/getting-started/>
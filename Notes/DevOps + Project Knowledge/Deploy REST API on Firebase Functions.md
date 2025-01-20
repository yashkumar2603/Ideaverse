# Prerequisites

Before we dive in, ensure you have the following:

- A Firebase account.
- Node.js and npm are installed on your machine.
- The Firebase CLI installed globally (`npm install -g firebase-tools`).

# Step 1: Initialize Your Node.js Project

Start by setting up your Node.js application. If you already have an application, you can skip to the next step. Otherwise, create a new directory for your project and initialize it with npm:

mkdir my-time-server  
cd my-time-server  
npm init -y

Install Express, a minimal and flexible Node.js web application framework:

npm install express

# Step 2: Create Your REST API

For demonstration purposes, let’s create a simple REST API that returns the current time in HTML. Create a file named `server.js` and add the following code:

const express = require('express');  
const app = express();  
  
app.get('/time', (req, res) => {  
    const currentTime = new Date().toLocaleTimeString();  
    res.send(`<!DOCTYPE html><html><head><title>Current Time</title></head><body><p>${currentTime}</p></body></html>`);  
});  
  
const PORT = process.env.PORT || 3000;  
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));

This code sets up an Express server with a single route (`/time`) that responds with the current time.

# Step 3: Set Up Firebase

Log in to Firebase using the CLI and initialize your project:

firebase login  
firebase init

Select `Functions` and follow the prompts to set up your Firebase project.

# Step 4: Prepare Your Application for Firebase Functions

Move your application code into the `functions` directory created by Firebase. You might need to adjust your application to work as a stateless function, which is a requirement for Firebase Functions.

# Step 5: Write a Firebase Function to Serve Your App

Edit the `index.js` file in the `functions` directory to import your app and create a function that serves it:

const functions = require('firebase-functions');  
const app = require('./server'); // Adjust the path as necessary  
  
exports.app = functions.https.onRequest(app);

# Step 6: Deploy Your API to Firebase

Deploy your function to Firebase by running:

firebase deploy --only functions

Firebase will provide a URL for your deployed function. Access it by appending your endpoint, such as `/time`, to see your REST API in action.
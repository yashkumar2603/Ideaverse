**These assignments are very nice** 

#### Assignment 1 -
This was basically just assignment 2 but no rate checking, we just had to see the total number of requests, so just make a global variable and then keep increasing its value by 1 with every request hitting ur server.

#### Assignment 2 -
```JavaScript
// You have to create a middleware for rate limiting a users request based on their username passed in the header

const express = require('express');
const app = express();

// Your task is to create a global middleware (app.use) which will
// rate limit the requests from a user to only 5 request per second
// If a user sends more than 5 requests in a single second, the server
// should block them with a 404.
// User will be sending in their user id in the header as 'user-id'
// You have been given a numberOfRequestsForUser object to start off with which
// clears every one second

app.use(function(req,res, next) {
  const user = req.headers["user-id"];
  if(numberOfRequestsForUser[user]){
    numberOfRequestsForUser[user] = numberOfRequestsForUser[user] + 1;
  }

  if(numberOfRequestsForUser[user] > 5) {
    res.status(404).send("too many requests");
  }
  else {
    next();
  }
});

let numberOfRequestsForUser = {};
setInterval(() => {
    numberOfRequestsForUser = {};
}, 1000)

app.get('/user', function(req, res) {
  res.status(200).json({ name: 'john' });
});

app.post('/user', function(req, res) {
  res.status(200).json({ msg: 'created dummy user' });
});

module.exports = app;
```
#### Assignment 3 -
```JavaScript
//  Implement an authentication middleware that checks for a valid API key in the request headers.

const express = require('express');
const app = express();
const VALID_API_KEY = '100xdevs_cohort3_super_secret_valid_api_key'; // key is 100xdevs-api-key use that API key for authenticate user


// Middleware to check for a valid API key
function authenticateAPIKey(req, res, next) {
    //  authenticate APIKey here
    if(req.headers["100xdevs-api-key"] && req.headers["100xdevs-api-key"] === VALID_API_KEY) {
        next();
    }
    else {
        res.status(401).send({message:"Invalid or missing API key"});
    }
}

app.use(authenticateAPIKey);

app.get('/', (req, res) => {
    res.status(200).json({ message: 'Access granted' });
});

module.exports = app; 
```

#### Assignment 4 -
```JavaScript
//  Create a middleware that logs all incoming requests to the console.

const express = require('express');
const app = express();

function logRequests(req, res, next) {
    // write the logic for request log here

    const log = `${req.method} ${req.url} - ${new Date().toISOString()}`;
    console.log(log);
    next();
}

app.use(logRequests);

app.get('/', (req, res) => {
    res.status(200).json({ message: 'Hello, world!' });
});

module.exports = app;
```

[Build a REST API with Node JS and Express | CRUD API Tutorial - YouTube](https://www.youtube.com/watch?v=l8WPWK9mS5M)
Test with insomnia or postman
Postman both in on browser and windows app available.
Express is a framework for NodeJS

In the `package.json` file after doing `npm init -y` if we add the field `"type":"module"` after the main file field, then we become able to use the new syntax, `import express from 'express';`
instead of `const express = require('express')`

Sample express scripts -
```JavaScript
import express from 'express';
import bodyParser from 'body-parser';

const app = express();   //our whole app now lives in this variable
const PORT = 5000;   //since frontends use 3000, we using 5000, take anything

app.use(bodyParser.json());   //we are telling express that we will use JSON format

app.listen(PORT, () => console.log(`Server running on port: http://localhost:${PORT}`));
//This tells that we will listen for requests at PORT and when we run the server, this will be logged on console
```

Now, if we change anything in the code, then we have to close the server, re-run it again for the changes to take place and be reflected. To avoid this, we can install `Nodemon`, by doing, `npm install --save-dev nodemon` 
the `--save-dev` flag is used because this is only for development time, wont be used when we deploy or put in production. 
Now, update the scripts in `package.json` to `"start":"nodemon index.js"` 
The console logs we get from this are going to be in the terminal, rather than the dev tools, that is how express works.

Right now, it will say `Cannot GET /` because we have not setup any routing yet.
To setup routing, we first add the `/` route, or the default route
```JavaScript
app.get('/', (req, res) => {    
	console.log('[TEST]!'); //logged in the terminal were the server was started.

	res.send('Hello from Homepage'); //sent/displayed on the HOME PAGE of the api made
});
```
We will be making this -
![[Pasted image 20240829014140.png]]
# Methods
HTTP methods are used to specify the type of action that the client wants to perform on a resource on the server.
<aside> 💡 You done NEED to use all the methods, but you always should. You can do everything you want with a `GET` or `POST` method, but it is usually advisable to use them right.

</aside>
### Common methods 
1. GET - Retrieve data from a server. (Get my TODOS)
2. POST - Submit data to be processed by a server. (Create a TODO)
3. PUT - Update or create a resource on the server (Update my todo)
4. DELETE - Remove a resource from the server. (Delete my todo)
# Routes
In the context of HTTP, **routes** are paths or endpoints that define how incoming requests are handled by a server. Routing is a mechanism used to direct incoming HTTP requests to the appropriate handler functions or resources based on the URL path.

### How to make routes 
Make a folder called as routes, then a `users.js` file, which is the route from which we have all other routes, this much is common, helps in understanding when we have a tree like structure with multiple branches for the routes.
in the users.js file,
```JavaScript
import express from 'express';
const router = express.Router();

router.get('/', (req,res) => {   //'/' and not '/users' is because we told about '/users' path in the index.js file 
	res.send('Hello');
});
//make different methods routes in this, this is for all the routes as server/users/whatever, this only is used to find the whatever part.
export default router;
```
Now, in the index.js file, 
```JavaScript
import express from 'express';
import bodyParser from 'body-parser';

import usersRoutes from './routes/users.js';

const app = express();  
const PORT = 5000;   

app.use(bodyParser.json());
//add this
app.use('/users', usersRoutes);   //we mentioned about the /users path here, dont need to do so in the users.js, it will '/' in the users.js
app.get('/', (req,res) => res.send('Hello from Homepage'));

app.listen(PORT, () => console.log(`Server running on port: http://localhost:${PORT}`));
```

Now, to display the existing users, the `/users/` GET route
```JavaScript
import express from 'express';
const router = express.Router();

const users =[
	{
		firstName:"Yash",
		lastName:"Kumar",
		age:25
	},
	{
		firstName:"Jane",
		lastName:"Doe",
		age:20
	}
]

router.get('/', (req,res) => {  
	console.log(users); //prints the whole users table.
	res.send('Hello');
});

//insert logic for POST and others

export default router;
```

Now, for adding users, the `/users/` POST route
Browsers can only make GET requests, by visiting the URLs, to make POST, we need JS code or POSTMAN typa stuff.
```JavaScript
router.post('/', (req,res) => {
	user = req.body;   //the user data that came in with the request.
	users.push(user);
	res.send(`User with name ${user.firstName} ${user.lastName} added to the database!`);
});
```
To add unique ids to each entry, use the uuid4 package and just add that to the req and then push.

Now, the find users by id route, 
```JavaScript 
router.get('/:id', (req,res) => {    //the : before the id signifies that id is a placeholder only and can have any value.
	const id = req.params.id;
	const found = users.find((user) => user.id === id);
	res.send(found);
});
```
to get the id, we use the `req.params.id` extract using `const id = req.params.id` or `const { id } = req.params` 

Now, to delete some user from the table, 
```JavaScript
router.get('/:id', (req,res) => {    //Note that the only difference in the function call is that the function is delete now.
	const id = req.params.id;
	users = users.filter((user) => user.id != id);
	res.send("Deleted the user!");
});
```
to make this work though, the table must actually be `let` defined, not `const`.

To update the user data, we will use the PATCH request, it is used when we want to partially modify something.
PUT method is when, we have to change the entirety of one block of data, if we want to change one param from the block, then 
```JavaScript
router.get('/:id', (req,res) => {    //the : before the id signifies that id is a placeholder only and can have any value.
	const id = req.params.id;
	const found = users.find((user) => user.id === id);
	const {firstName, lastName, age} = req.body;  //take whatever is provided from these.
	if(firstName){
		user.firstName = firstName;
	}
	if(lastName){
		user.lastName = lastName;
	}
	if(age){
		user.age = age;
	}
});
```
# Response
The response represents what the server returns you in response to the request.
It could be
Plaintext data - Not used as often
HTML - If it is a website
JSON Data - If you want to fetch some data (user details, list of todos…)
#### JSON
JSON stands for JavaScript Object Notation. It is a lightweight, text-based format used for data interchange
```JavaScript
{
  "name": "John Doe",
  "age": 30,
  "isEmployed": true,
  "address": {
    "street": "123 Main St",
    "city": "Anytown"
  },
  "phoneNumbers": ["123-456-7890", "987-654-3210"]
}
```
# Status codes
HTTP status codes are three-digit numbers returned by a server to indicate the outcome of a client’s request. They provide information about the status of the request and the server's response.
### 200 series (Success)
- **200 OK**: The request was successful, and the server returned the requested resource.
- **204 No Content**: The request was successful, but there is no content to send in the response
### 300 series (Redirection)
- **301 Moved Permanently**: The requested resource has been moved to a new URL permanently. The client should use the new URL provided in the response.
- **304 Not Modified**: The resource has not been modified since the last request. The client can use the cached version.
### 400 series (Client Error)
- **400 Bad Request**: The server could not understand the request due to invalid syntax.
- **401 Unauthorized**: The request requires user authentication. The client must provide credentials.
- **403 Forbidden**: The server understood the request but refuses to authorize it.
- **404 Not Found**: The requested resource could not be found on the server.
### 500 series (Server Error)
- **500 Internal Server Error**: The server encountered an unexpected condition that prevented it from fulfilling the request.
- **502 Bad Gateway**: The server received an invalid response from an upstream server while acting as a gateway or proxy.
# Headers
HTTP headers are key-value pairs included in HTTP requests and responses that provide `metadata` about the message.
#### Why not use body?
Even though you can use body for everything, it is a good idea to use `headers` for sending data that isn’t directly related with the `application logic`.
For example, if you want to create a new TODO, you will send the TODO payload in the body
```JavaScript
{
   description: "Go to gym"
}
```
But the Authorization information in the headers
```JavaScript
Authorization: harkirat
```


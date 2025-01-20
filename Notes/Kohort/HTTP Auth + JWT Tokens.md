Auth Works on Tokens (JWT Tokens), once we login, we get the token back and it is stored in the browser and that is how we get persistent session. Now, we send the token with every request, not the username and password everytime we make a request. We can even steal someone's tokens and pretend to be them using postman. 
Somewhat like a cheque book that the bank gives us, but this time it is all signed before only. 
# Auth workflow
The workflow for authentication usually looks as follows 
1. The user comes to your website
2. The user sends a request to `/signin` with their `username` and `password`
3. The user gets back a `token`
4. In every subsequent request, the user sends the token to identify it self to the backend.

<aside> 💡

Think of the token like a `secret` that the server has given you. You send that `secret` back to the server in every request so that the server knows who you are.

</aside>

# Creating an express app
Lets initialise an express app that we will use to generate an `authenticated backend` today.
- Initialise an empty Node.js project
    
    ```jsx
    npm init -y
    ```
    
- Create an `index.js` file, open the project in visual studio code
    
    ```jsx
    touch index.js
    ```
    
- Add `express` as a dependency
    
    ```jsx
    npm i express
    ```
    
- Create two new POST routes, one for `signing up` and one for `signing in`
    
    ```jsx
    const express = require('express');
    const app = express();
    
    app.post("/signup", (req, res) => {
    
    });
    
    app.post("/signin", (req, res) => {
    
    });
    
    app.listen(3000);
    ```
    
- Use `express.json` as a middleware to parse the post request body
    
    ```jsx
    app.use(express.json());
    ```
    
- Create an `in memory` variable called `users` where you store the `username` , `password` and a `token` (we will come to where this token is created later.
    
    ```jsx
    const users = []
    ```
    
- Complete the signup endpoint to store user information in the `in memory variable`
    
    ```jsx
    app.post("/signup", (req, res) => {
        const username = req.body.username;
        const password = req.body.password;
    
        users.push({
            username,
            password
        })
        res.send({
            message: "You have signed up"
        })
    });
    ```
    
- Create a function called `generateToken` that generates a random string for you
    
    ```jsx
    function generateToken() {
        let options = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'];
    
        let token = "";
        for (let i = 0; i < 32; i++) {
            // use a simple function here
            token += options[Math.floor(Math.random() * options.length)];
        }
        return token;
    }
    ```
    
- Finish the signin endpoint. It should generate a token for the user and put it in the `in memory` variable for that user
    
    ```jsx
    
    app.post("/signin", (req, res) => {
        const username = req.body.username;
        const password = req.body.password;
    
        const user = users.find(user => user.username === username && user.password === password);
    
        if (user) {
            const token = generateToken();
            user.token = token;
            res.send({
                token
            })
            console.log(users);
        } else {
            res.status(403).send({
                message: "Invalid username or password"
            })
        }
    });
    ```

<aside> 💡

This can be improved further by

1. Adding zod for input validation
2. Making sure the same user cant sign up twice
3. Persisting data so it stays even if the process crashes We’ll be covering all of this eventually

</aside>

# Creating an authenticated EP
Let’s create an endpoint (`/me` ) that returns the user their information `only if they send their
```jsx
app.get("/me", (req, res) => {
    const token = req.headers.authorization;
    const user = users.find(user => user.token === token);
    if (user) {
        res.send({
            username: user.username
        })
    } else {
        res.status(401).send({
            message: "Unauthorized"
        })
    }
})
```

# Tokens vs JWTs

There is a problem with using `stateful` tokens.
By stateful here, we mean that we need to store these tokens in a variable right now (and eventually in a database).

The problem is that we need to `send a request to the database` every time the user wants to hit an `authenticated endpoint`
Solution : JWTs
JWT works by building a token when logging in, if the credentials are right. 
If the credentials are right, we only use the username to make the JWT, don't use the password as JWT can be decoded by anyone fr
![[Pasted image 20241020132712.png]]
So now we are hitting DB only to get the Actual stuff, not for doing the authentication stuff and the linear search in the users array or anything like that,
# Replace token logic with jwt
Add the jsonwebtoken library as a dependency - [https://www.npmjs.com/package/jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken)


Get rid of our generateToken function

Create a JWT_SECRET variable

`const JWT_SECRET = "USER_APP";`

Create a jwt for the user instead of generating a token

```JavaScript
app.post("/signin", (req, res) => { const username = req.body.username; const password = req.body.password; const user = users.find(user => user.username === username && user.password === password); if (user) { const token = jwt.sign({ username: user.username }, JWT_SECRET); user.token = token; res.send({ token }) console.log(users); } else { res.status(403).send({ message: "Invalid username or password" }) } });](<app.post("/signin", (req, res) =%3E {
    const username = req.body.username;
    const password = req.body.password;

    const user = users.find(user => user.username === username && user.password === password);

    if (user) {
        const token = jwt.sign({
            username: user.username
        }, JWT_SECRET);

        user.token = token;
        res.send({
            token
        })
        console.log(users);
    } else {
        res.status(403).send({
            message: "Invalid username or password"
        })
    }
});```
Notice we put the username inside the token. The jwt holds your state. You no longer need to store the token in the global users variable
In the /me endpoint, use jwt.verify to verify the token
```JavaScript
app.get("/me", (req, res) => { const token = req.headers.authorization; const userDetails = jwt.verify(token, JWT_SECRET); const username = userDetails.username; const user = users.find(user => user.username === username); if (user) { res.send({ username: user.username }) } else { res.status(401).send({ message: "Unauthorized" }) } })](<app.get("/me", (req, res) =%3E {
    const token = req.headers.authorization;
    const userDetails = jwt.verify(token, JWT_SECRET);

    const username =  userDetails.username;
    const user = users.find(user => user.username === username);

    if (user) {
        res.send({
            username: user.username
        })
    } else {
        res.status(401).send({
            message: "Unauthorized"
        })
    }
})
```

# JWTs can be DECODED by anyone
JWTs can be decoded by anyone. They can be `verified` by only the server that issued them.
Ref - [https://jwt.io/](https://jwt.io/)
Try creating a jwt and decoding it on the website. You’ll notice it does decode. But that is fine
### Comparison to a cheque.
If you ever sign a `cheque`, you can show it to everyone and everyone can see that you are transferring $20 to a friend. But only the BANK `needs to verify` before debiting the users account.
Doesnt matter if everyone sees the cheque, they cant do anything with this information.
But the `bank` can `verify` the signature and do whatever the end users asked to do
JWTs can be coded by everyone
JWTs can be verified by only the person who issued them (using the JWT secret)

# Auth Middleware -
```JavaScript
function auth(req, res, next) {
    const token = req.headers.authorization;
    if (token) {
        jwt.verify(token, JWT_SECRET, (err, decoded) => {
            if (err) {
                res.status(401).send({
                    message: "Unauthorized"
                })
            } else {
                req.user = decoded;
                next();
            }
        })
    } else {
        res.status(401).send({
            message: "Unauthorized"
        })
    }
}

app.get("/me", auth, (req, res) => {
    const user = req.user;
    res.send({
        username: user.username
    })
})
```


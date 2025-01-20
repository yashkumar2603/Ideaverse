**Using middleware**
Express is a routing and middleware web framework that has minimal functionality of its own: An Express application is essentially a series of middleware function calls.
> Middleware functions are functions that have access to the request object (req), the response object (res), and the next middleware function in the application's request-response cycle. The next middleware function is commonly denoted by a variable named next.

Middleware functions can perform the following tasks:
• Execute any code.
• Make changes to the request and the response objects.
• End the request-response cycle.
Call the next middleware function in the stack.

We need not use the next parameter in the route handling,  we only need it when we are setting up the middle ware, it is not to be used in the endpoint/route handler, but only in the middleware(stuff in the middle).


![](https://petal-estimate-4e9.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Fcb3f8113-af87-4982-bc9b-7d777c7013cf%2FScreenshot_2024-08-31_at_7.17.08_PM.png?table=block&id=dd6185a5-43fd-4b20-af01-f36eadd33e0d&spaceId=085e8ad8-528e-47d7-8922-a23dc4016453&width=2000&userId=&cache=v2)
```JavaScript
app.use(function(req, res, next) {
    console.log("request received");
    next();
})

app.get("/sum", function(req, res) {
    const a = parseInt(req.query.a);
    const b = parseInt(req.query.b);

    res.json({
        ans: a + b
    })
});
```

### Modifying the request
```JavaScript
app.use(function(req, res, next) {
    req.name = "harkirat"
    next();
})
app.get("/sum", function(req, res) {
    console.log(req.name);
    const a = parseInt(req.query.a);
    const b = parseInt(req.query.b);

    res.json({
        ans: a + b
    })
});
```
### Ending the request/response cycle
```JavaScript
app.use(function(req, res, next) {
    res.json({
        message: "You are not allowed"
    })
    //no next() was called, so it doesnt propagate to the next middleware in line, ends here.
})
app.get("/sum", function(req, res) {
    console.log(req.name);
    const a = parseInt(req.query.a);
    const b = parseInt(req.query.b);
    res.json({
        ans: a + b
    })
});
```
#### Calling the next middleware function in the stack.
```JavaScript
app.use(function(req, res, next) {
    console.log("request received");
    next();
})
app.get("/sum", function(req, res) {
    const a = parseInt(req.query.a);
    const b = parseInt(req.query.b);

    res.json({
        ans: a + b
    })
});
```

## Using Middleware function -
```JavaScript
function isoldEnoughMiddleware(req, res, next) {
	if (age >= 14) {
		next();
	} else {
		res.json({msg: "Sorry you are not of age yet",})
	}
}
//or use this, if every route uses the same middleware.
//app.use(middlewareAll);
app.get("/ride2", isoldEnough Middleware, function (req, res) {
	res.json({msg: "You have successfully riden the ride 2",});
}); //mention middleware before req, res.
app.get("/ride1", is0ldEnough Middleware, function (req, res) {
	res.json({msg: "You have successfully riden the ride 1",});
}); //mention middleware before req, res.
app.listen(3000);
```

If the whole app has to use a middleWare, for example for auth, then just add,
`app.use(middlewareFunction);` before the first `app.get(` in the stack.
This `app.use` triggers only for the endpoints below it.

In between the many middlewares, the data is passed through the fact that all the middlewares basically share the same req and res objects and we can actually manipulate these only to pass data from one middleware to the other. dont need to do anything else.


### Authenticated requests -
I make an auth middleware, then use that auth middleware as the second argument in any route that i want authenticated, so i just pass the middleware name (auth) as the second argument to the authenticated route after the route endpoint.

Example :
```JavaScript
app.post("/todo", auth, function(req,res){
	//AUTHENTICATED ROUTE
	const userId = req.userId;
})

function auth(req, res, next) {
	const token = req.header.token;
	const decodedData = jwt.verify(token, JWT_SECRET);

	if(!decodedData){
		req.userId = decodedData.userId;
		next();
	} else {
		res.status(403).json({
			message:"Incorrect Credentials"
		})
	}
}

//we add a new field to the req body only, so that we dont pass it using somme complicated logic, we can simply access the req in any subsequent function and it will have this already done.
```
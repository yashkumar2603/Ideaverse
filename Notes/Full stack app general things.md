To avoid the complexity of CORS, we can do this, we can add a route to our backend only that returns the frontend to us, 
something like 
```JavaScript
app.get("/", function(req, res)){
	res.sendFile(__dirname + "/public/index.html");
})
//Now this will serve us the frontend whenever we hit the localhost:3000/ endpoint, now we can handle all the other logic in the frontend, but this supposedly would work only with simple html code, not with complex apps ig
```

Simple application `signup()` function on the frontend 
```JavaScript
async function signup() {
    const username = document.getElementById("signup-username").value;
    const password = document.getElementById("signup-password").value;

	await axios.post("http://localhost:3000/signup", {
	    username: username,
        password: password
    });
    alert("You are signed in");
}
```
This code sends the username and password to the backend signup function, which sings a new token for the user. 

Simple application `signin()` function on the frontend
```JavaScript
async function signin() {
    const username = document.getElementById("signin-username").value;
	const password = document.getElementById("signin-password").value;
    const response = await axios.post("http://localhost:3000/signin", {
	    username: username,
        password: password
    });

    localStorage.setItem("token", response.data.token);
	alert("You are signed in");
}
```
This code sends the username and id entered  to the backend URL for verification using the JWT_TOKEN and then returns the token for the user in the response object, then we can store the token in the localStorage for the website.

And on the frontend `logout()` function, we just have to clear the "token" item from the localStorage, this will make it so that the `signin()` or the `signup()` functions dont get any token to send, that makes the request to the backend. So finally the user logs out.

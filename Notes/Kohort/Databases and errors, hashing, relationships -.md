# Hashing password

### Why should you hash passwords?
Password hashing is a technique used to securely store passwords in a way that makes them difficult to recover or misuse. Instead of storing the actual password, you store a hashed version of it.
### salt
A popular approach to hashing passwords involves using a hashing algorithm that incorporates a salt—a random value added to the password before hashing. This prevents attackers from using precomputed tables (rainbow tables) to crack passwords.
### bcrypt
Bcrypt: It is a cryptographic hashing algorithm designed for securely hashing passwords. Developed by Niels Provos and David Mazières in 1999, bcrypt incorporates a salt and is designed to be computationally expensive, making brute-force attacks more difficult.
### Base code
We’re starting from yesterday’s code - [https://github.com/100xdevs-cohort-3/week-7-mongo](https://github.com/100xdevs-cohort-3/week-7-mongo)

### Adding password encryption

Install the bcrypt library -​
![](https://petal-estimate-4e9.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F7f4fba53-1604-4648-b38c-a2e96c6255a1%2FScreenshot_2024-09-15_at_6.43.20_PM.png?table=block&id=1027dfd1-0735-80dc-a276-dd4cefa5911b&spaceId=085e8ad8-528e-47d7-8922-a23dc4016453&width=1360&userId=&cache=v2)

```JavaScript
app.post("/signup", async function(req, res) { 
	const email = req.body.email; 
	const password = req.body.password; 
	const name = req.body.name; 
	const hasedPassword = await bcrypt.hash(password, 10); 
	await UserModel.create({ 
		email: email, 
		password: hasedPassword, 
		name: name 
	}); 
	res.json({ 
		message: "You are signed up" 
	}) 
});
```
Password format
![](https://petal-estimate-4e9.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F02fe0cd0-304f-47e0-af73-9bcae196ccd9%2FScreenshot_2024-09-15_at_6.48.32_PM.png?table=block&id=1027dfd1-0735-80e9-a607-cadd4065b32a&spaceId=085e8ad8-528e-47d7-8922-a23dc4016453&width=1360&userId=&cache=v2)

![](https://petal-estimate-4e9.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F507c3334-4c13-494a-bb9f-c137de151688%2FScreenshot_2024-09-15_at_6.51.34_PM.png?table=block&id=1027dfd1-0735-80fc-9b6a-d9da8594d593&spaceId=085e8ad8-528e-47d7-8922-a23dc4016453&width=1360&userId=&cache=v2)
So, putting it all together:

`$2b$`: Version of bcrypt.

`10$`: Cost factor (saltRounds).

`wyemvgfpjkEzg2dzuRyM9e`: Salt value (base64 encoded).

`LrQZnT69X/tj0KW/zM6TZhnrvT.TCne`: Hashed password (base64 encoded).

Update the signin function
```JavaScript
app.post("/signin", async function(req, res) { 
	const email = req.body.email; 
	const password = req.body.password; 
	
	const user = await UserModel.findOne({ 
		email: email, 
	}); 
	
	
	const passwordMatch = bcrypt.compare(password, user.password); 
	if (user && passwordMatch) { 
		const token = jwt.sign({ 
			id: user._id.toString() 
		}, JWT_SECRET); 
		res.json({ token })  
	} else { res.status(403).json({ 
			message: "Incorrect creds" 
		}) 
	} 
});
```

# Error handling
Right now, the server crashes if you sign up using duplicate email
How can you fix this?
### Approach #1 - Try catch
In JavaScript, a try...catch block is used for handling exceptions and errors that occur during the execution of code. It allows you to write code that can manage errors gracefully rather than crashing the application or causing unexpected behavior.
```JavaScript
try { 
	// Attempt to execute this code 
	let result = riskyFunction();  // This function might throw an error 
	console.log('Result:', result); 
} catch (error) { 
	// Handle the error if one is thrown 
	console.error('An error occurred:', error.message); 
} finally { 
	// This block will always execute 
	console.log('Cleanup code or final steps.'); 
}
```

# Relationships in mongo
In MongoDB, data is `related` across collections using something called `references`
In our TODO application, each todo `refers` to an entry in the `users` table.
![Screenshot 2024-09-14 at 6.47.59 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/085e8ad8-528e-47d7-8922-a23dc4016453/5af9696a-b003-4bfd-a9e3-2ac1eaaac9b0/Screenshot_2024-09-14_at_6.47.59_PM.png)
## Our original schema
```jsx
const Todo = new Schema({
    userId: ObjectId,
    title: String,
    done: Boolean
});
```
## Update schema with references
```jsx
const TodoSchema = new Schema({
  userId: { type: Schema.Types.ObjectId, ref: 'users' },
  title: String,
  done: Boolean
});
```
## Benefits?
You can `pre-populate` fields like `user information` since you’ve defined the exact relationship
```jsx

app.get("/todos", auth, async function(req, res) {
    const userId = req.userId;

    const todos = await TodoModel.find({
        userId
    }).populate('userId').exec();

    res.json({
        todos
    })
});
```
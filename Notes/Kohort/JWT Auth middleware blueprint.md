```Javascript
const jwt = require('jsonwebtoken');
const SECRET = process.env.SECRET || 'secret000'; 

const authenticateJwt = (req, res, next) => {
  const authHeader = req.headers.authorization;
  if (authHeader) {
    const token = authHeader.split(' ')[1];
    jwt.verify(token, SECRET, (err, user) => {
      if (err) {
        return res.status(403).json({ message: 'Forbidden: Invalid token' });
      }
      req.userId = user.userId;
      next();
    });
  } else {
    res.status(401).json({ message: 'Unauthorized: No token provided' });
  }
};

module.exports = {
  authenticateJwt,
  SECRET,
};
```

Now to use this in the endpoints, just use this.
See the code solutions for the week-5 assignments to get more idea on the connection of frontend and backend, the JWT auth flow and routes, use of middlewares.

```JavaScript
generate Token - jwt.sign( info,JWT_SECRET)  
verify a Token - jwt.verify(token,JWT_SECRET)  
decode a Token - jwt.decode(token)
```

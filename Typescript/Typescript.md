Create a function that takes another function as input, and runs it after 1 second.
```typescript
function delayedCall(fn: () => void) {
    setTimeout(fn, 1000);
}

delayedCall(function() {
    console.log("hi there");
})
```
Example of a basic thing in typescript

### the .tsconfig file
![[Pasted image 20241211224847.png]]

### Interfaces
![[Pasted image 20241211225643.png]]
Use of interface -
```typescript
// Todo.tsx
interface TodoType {
  title: string;
  description: string;
  done: boolean;
}

interface TodoInput {
  todo: TodoType;
}

function Todo({ todo }: TodoInput) {
  return <div>
    <h1>{todo.title}</h1>
    <h2>{todo.description}</h2>
    
  </div>
}
```
![[Pasted image 20241211230022.png]]

### Types
Similar to interfaces, these let to aggregate data together
```typescript
type User = {
	firstName: string;
	lastName: string;
	age: number
}
```
And, also let us do these things
![[Pasted image 20241211230248.png]]

#### Arrays
If you want to access arrays in typescript, it’s as simple as adding a `[]` annotation next to the type. Similar to C++

### Enums -
![[Pasted image 20241211234804.png]]

The common use case in Express -
```typescript
enum ResponseStatus {
    Success = 200,
    NotFound = 404,
    Error = 500
}

app.get("/', (req, res) => {
    if (!req.query.userId) {
			res.status(ResponseStatus.Error).json({})
    }
    // and so on...
		res.status(ResponseStatus.Success).json({});
})
```

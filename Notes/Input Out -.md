Hello world code is - 
```Go
package main  
  
import . "fmt"  
  
func main() {  
    Print("Hello World")  
}
```
We can either make the import as `import "fmt"` or as `import . "fmt"`,
in the second one we dont have to put `fmt.<func>` everytime we use something from it.

# Declarations -
```Go
//Declare 2 methods
var lol = "lol"  
//or  
lmao := "lmao"  

//print formatted 
Printf("lol %v %v", lol, lmao)
```
# Input -
```Go
//take input  
var user string  
Scanln(&user)
```

# Output -
```Go
Println(user)
```

# Array -
```Go
var bookings [50]string  //size then type
bookings[0] = "123"
```
to get size - `len(array)` 
### Variable size array -
Just like vectors in C++
It is called Slices in Golang.
It is an abstraction of arrays and anyways more efficient than arrays, so always use slices.
To define a slice just dont mention a fixed size when declaring array, that's it.
`var bookings []string` - this created a slice
Accessing elements is same (just like C++ vector)
To push in elements, the syntax is
```Go
var bookings []string
bookings = append(bookings, "append this pls")  //append and then assign back
```

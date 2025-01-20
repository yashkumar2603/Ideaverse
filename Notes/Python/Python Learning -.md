<font color="#e36c09">use this, very nice, for revision or knowledge or when stuck </font>
[Advanced Python - Complete Course - YouTube](https://youtube.com/playlist?list=PLqnslRFeH2UqLwzS0AwKDKLrpYBKzLBy2&si=qXMFgk-QwZbAnX0-)

**Slicing -** 
sliced = array[ start : stop : step ]
[n : 0 : -1] or [::-1] is reverse the string. 

**Unpack operator `*`**
if we do `*array`, all the elements of the array are taken up individually
for example 
```Python
def func(x,y)
	print(x, y)

pairs=[(1,2), (3,4)]

for pair in pairs:
	func(*pair); #unpacks and passes into the function as the two arhiements.

# for dictionary, we need such ** stars
func(**{'x':2, 'y':5})
```

**Raise Exceptions & try except block -** 
[Python's raise: Effectively Raising Exceptions in Your Code – Real Python](https://realpython.com/python-raise-exception/#chaining-exceptions-with-the-from-clause)

**Lambda Function + map and filter -**
[How to Use Python Lambda Functions – Real Python](https://realpython.com/python-lambda/)

```Python
(func = lambda param1, param2 : operations if(conditions))(p1, p2) #call inline
func(p1,p2) #call later
``` 
adding if statements isn't necessary obviously
Higher order function using  Lambda functions - 
```Python
>>> high_ord_func = lambda x, func: x + func(x)
>>> high_ord_func(2, lambda x: x * x)
6
>>> high_ord_func(2, lambda x: x + 3)
7
```

in the context of the lambda function, Python did not provide the name of the function, but only `<lambda>`. This can be a limitation to consider when an exception occurs, and a [traceback](https://realpython.com/python-traceback/) shows only `<lambda>`:
A lambda function can’t contain any statements. In a lambda function, statements like `return`, `pass`, `assert`, or `raise` will raise a [`SyntaxError`](https://realpython.com/invalid-syntax-python/) exception.
example usages :
```Python
>>> (lambda x, y, z: x + y + z)(1, 2, 3)
6
>>> (lambda x, y, z=3: x + y + z)(1, 2)
6
>>> (lambda x, y, z=3: x + y + z)(1, y=2)
6
>>> (lambda *args: sum(args))(1,2,3)
6
>>> (lambda **kwargs: sum(kwargs.values()))(one=1, two=2, three=3)
6
>>> (lambda x, *, y=0, z=0: x + y + z)(1, y=2, z=3)
6
```

#### Decorators
In Python, a [decorator](https://www.python.org/dev/peps/pep-0318/) is the implementation of a pattern that allows adding a behavior to a function or a class. It is usually expressed with the `@decorator` syntax prefixing a function. Here’s a contrived example:
```Python
def some_decorator(f):
    def wraps(*args):
        print(f"Calling function '{f.__name__}'")
        return f(args)
    return wraps

@some_decorator
def decorated_function(x):
    print(f"With argument '{x}'")
```
In the example above, `some_decorator()` is a function that adds a behavior to `decorated_function()`, so that invoking `decorated_function("Python")` results in the following output:
```Shell
Calling function 'decorated_function
With argument 'Python'
```

### Iterators, list comprehension
```Python
# Iterators
list = [4, 7, 9, 14, 19]
my_iter = iter(list)
my_iter  # prints the memory address of the iterator
print(next(my_iter))
# if we happen to go out of bounds, the exception StopIteration will be raised

# List Comprehension
arr = ["apple", "banana", "cherry"]
newarr = [x for x in arr if "a" in x]  # syntax for list comprehension
print(newarr)
# prints ['apple', 'banana'] as they have 'a' in them

# another example
nums = [y for y in range(100) if y % 7 == 0 and y % 5 == 0]
print(nums)
obj = ["Even" if i % 2 == 0 else "Odd" for i in range(10)]
obj  # prints ['Even', 'Odd', 'Even', 'Odd', 'Even', 'Odd', 'Even', 'Odd', 'Even', 'Odd']

# in the same syntax as list comprehension, we can also have set comprehension and dictionary comprehension
# replace [ ] with { } for comprehension of dictionaries
# replace [ ] with ( ) for comprehension of sets

# for example
squares = {x: x * x for x in range(6)}
squares  # prints {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25
```

Generators are basically us trying to define our own iterator, not used much i suppose, if u see just gpt for the explanation.

### Regular Expressions and Closures(peace) -
```Python
# regualr expressions
import re

pattern = '^a...s$'
test_string = 'abyss'

result = re.match(pattern, test_string)
#abyss wont match, tho it should match as the way this matching is done, will mathc the pattern end s with the second-last s in abyss and then the first a in abyss will be matched with the first a in the pattern then the number of empty places will be mismatch and hence no match in abyss, to make it work with abyss, we have to do a workaround
#Alias matches, as it is case insensitive

# There are multiple meta characters that are used in regular expressions, lookup as needed
# known as regex engine regular expression matching operations
# re.match() - checks for a match only at the beginning of the string
# re.search() - checks for a match anywhere in the string
# re.findall() - returns a list of all matches
# re.split() - returns a list where the string has been split
# re.sub() - replaces one or many matches with a string
# re.compile() - used to compile a regular expression pattern into a regular expression
#
#Closures - it is a function object that remembers valeus in enclosing scopes ecen if they are not present in memory
# unlike a plain function, it allows the function to access those captured variables through the closure's copies of their values or references, even when the function is called outside their scope
```

### Dunders(double under-score) / Magic Methods -
```Python
class Vector:
    def __init__(self, x, y):
        self.x = x 
        self.y = y
    def __add__(self, other):  #this is how we pass other objects
        return Vector(self.x + other.x, self.y + other.y)
    def __repr__(self): 
        return f"X: {self.x}, Y: {self.y}"

v1=Vector(10, 28)
v2=Vector(50, 68)
v3=v1+v2
print(v3)
```

The `__add__` overloads the + operator
the `__repr__` determines what happens when we simply go ahead and print it off
same thing can be achieved by using  `__string__`, which defines what happens when object converted to string
The dunder `__call__` defines what happens when the object was called as a function.

#### Decorator
```Python
def my_decorator(function):
    def wrapper():
        print("I am decorating")
        function()
    return wrapper

def hello():
    print("hello world")

my_decorator(hello)()
```

This is how we write the same thing as what a decorator would do 
Now to use the decorator - 
```Python
def my_decorator(function):
    def wrapper():
        print("I am decorating")
        function()
    return wrapper

@my_decorator
def hello():
    print("hello world")

hello() 
```
This is the same thing using a decorator

Now, an actual usage of this, 
```Python
import time

def timed(function):
    def wrapper(*args, **kwargs):
        start = time.time()
        value = function(*args, **kwargs)
        end = time.time()
        fname = function.__name__
        print(f"{fname} took {end-start} seconds to run")
        return value
    return wrapper

#now write ur function and decorate it with the timed decorator
```

If we would not have used the method of decorators, both logging and printing is single call would not have been possible

**Argument parsing**
```Python
# python3 file.py hehe
sys.argv = [0] #gives the filename - file
sys.argv = [1] #gives the first argument - hehe

# for optional arguements, 
import getopt

opts,args = getopt.getopt(sys.argv[1:], "f:m:t")
#: means we expect something after that, f, m,t are now flags
# python3 -f file.py -m hehe -t hello 
# -f is a flag, -m is a flag, -t is a flag
# returns 
# opts = [('-f', 'file.py'), ('-m', 'hehe'), ('-t', 'hello')]
# args = []
```

### OOPs basics
```Python
# OOPs stuff
class Person:
    def __init__(self, name, age):
        self.__name = name #__ for marking as private
        self.__age = age
        
    @property     #have to specify
    def Name(self):
        return self.__name
        
    @Name.setter   #have to specify
    def Name(self, value):
        self.__name = value

p1 = person("yash", 21, 'm')
print(p1.Name()) #yash
```

**parameter hinting in python**
```Python
#parameter hinting in python
def function(param : int) -> str:      # this will only accept int in input and string in reurns 
    return param
```

### Design Patterns in Python
[Factory Design Pattern - Advanced Python Tutorial #7 - YouTube](https://youtu.be/-a1PFtooGo4?si=0fI1ZQmoIsggBPq9)
[Proxy Design Pattern - Advanced Python Tutorial #8 - YouTube](https://www.youtube.com/watch?v=cMmuAbnG7UU&list=PL7yh-TELLS1FuqLSjl5bgiQIEH25VEmIc&index=8&pp=iAQB)
[Singleton Design Pattern - Advanced Python Tutorial #9 - YouTube](https://www.youtube.com/watch?v=Qb4rMvFRLJw&list=PL7yh-TELLS1FuqLSjl5bgiQIEH25VEmIc&index=9&pp=iAQB)
[Composite Design Pattern - Advanced Python Tutorial #10 - YouTube](https://www.youtube.com/watch?v=iSG87hpAFhQ&list=PL7yh-TELLS1FuqLSjl5bgiQIEH25VEmIc&index=10&pp=iAQB)
Merge 2 lists or sets using * operator
```Python
list1 = [1, 2, 3 ,4]
list2 = [5, 6, 7, 8]
final = [*list1, *list2 ]
```
use ** for dictionary

## Levels of copy and shallow, deep copy
```Python
#Levels of copying 
import copy
#Variable copying(Level 0)
x=4
y=x
y=5
print(x) # prints 4
print(y) #prints 5

#list copying(level 1)
or = [1,2,3,4]
cpy = copy.copy(or)
cpy[0]=-10
print(cpy)  #-10 2 3 4
print(or)  #1 2 3 4

#list of list copying(level 2)
or = [[0, 1,2,3], [5,6,7,8]]
cpy = copy.copy(or)
cpy[0][1]=-10
print(cpy) #-10 2 3 4   5 6 7 8
print(or)  #-10 2 3 4   5 6 7 8
# both came out same as in level 2,(2-D -> list of lists) we cannot use same thing as level 1

# to fix this, use copy.deepcopy
cpy=copy.deepcopy(or)
cpy[0][1]=-10
print(cpy) #-10 2 3 4   5 6 7 8
print(or)  #1 2 3 4   5 6 7 8
```
This deepcopy method also works for custom class and objects

Basic File handling 
```Python

# better method, using with open as statements
with open('notes.txt','w') as file:
    file.write('some todoo...')
    
# does same thing but manually and uglier
file = open('notes.txt','w')
try:
    file.write('some todoo...')
finally:
    file.close()
```

### Context Management -
```Python
# Context management
# __enter__ is executed on entering, __exit__ is executed on exiting, we can use the exception details to do stuff
class ManagedFile:
    def __init__(self, filename):
        print('init')
        self.filename = filename

    def __enter__(self):
        print('enter')
        self.file = open(self.filename, 'w')
        return self.filename

    def __exit__(self, exc_type, exc_value, exc_traceback):
        if self.file:
            self.file.close()
        if exc_type is not None:
            print("handle exception")
        print('exc:', exc_type, exc_value)
        print('exit')

with ManagedFile('notes.txt') as file:
    print('do something')
    file.write('some stuff')
    file.somemethod() #will raise an exception and we wont reach the continue

print('continuing...')
```

#### Dataclasses - 
[Python dataclasses will save you HOURS, also featuring attrs - YouTube](https://www.youtube.com/watch?v=vBH6GRJ1REM)
```Python
#dataclasses, implements all the necessary functions of a class for use
from dataclasses import dataclass

@dataclass 
class Comment:
    id:int 
    text:str 

#now, if we see we will have all the methods such as __init__, represent, equal to handling 

#now if we want to add a __hash__ method, and __setattr__(to set, now that the thing is immutable), we need to make it immutable, just add this
@dataclass(frozen = True)

#to get ordering and comaprison functions, equal to and less than, just do the
@dataclass(order = True)

#generally, keep the classes immmutable(so that we can use it as keys in a dictionary for example), if we need to change later on, we can use the replace method from the dataclasses library
```

```Python
# to know if a file ran or not, add this to it 
if __name__ == "__main__":
    print("this ran")

#now, what happens is that, if the file with this code was executed directly, then this will print, but if the file was imported and then ran, then this wont be executed 
```



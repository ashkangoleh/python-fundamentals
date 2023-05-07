# Functions

1. Defining a function
1. Parameters and arguments
1. Return values
1. Scope
1. Lambda functions
1. Built-in functions
1. Best practices

### Defining a function

In Python, you define a function using the `def` keyword followed by the function name and any parameters it takes. For example, here's a simple function that takes two parameters and prints their sum:

```python
def add_numbers(x, y):
    sum = x + y
    print(sum)
```
To call this function, you simply pass in the values for `x` and `y`:

```python
add_numbers(2, 3) # Output: 5
```


### Parameters and arguments

Parameters are the variables that are defined in the function definition, while arguments are the values that are passed to the function when it is called. Python supports several types of parameters, including positional, keyword, and default parameters.

Here's an example of a function that takes a positional parameter and a keyword parameter:

```python
def greet(name, greeting='Hello'):
    print(f"{greeting}, {name}!")
```

In this example, `name` is a positional parameter, which means it must be passed in the correct order when the function is called. `greeting` is a keyword parameter, which means it has a default value of `'Hello'` but can be overridden by passing a value when the function is called.

```python
greet('Alice') # Output: Hello, Alice!
greet('Bob', 'Hi') # Output: Hi, Bob!
```

### Return values

Functions can return a value using the `return` keyword. If a function does not have a return statement, it will return `None` by default.

Here's an example of a function that returns the sum of two numbers:

```python
def add_numbers(x, y):
    sum = x + y
    return sum
```

To use the return value of this function, you can assign it to a variable:

```python
result = add_numbers(2, 3)
print(result) # Output: 5
```

### Scope

Variables defined inside a function are only accessible within that function's scope. This means that they cannot be accessed outside of the function.

Here's an example of a function that defines a variable `count` and increments it each time the function is called:

```python
def increment_count():
    count = 0
    count += 1
    print(count)
```

If you try to access count outside of the function, you'll get a NameError:

```python
increment_count() # Output: 1
print(count) # NameError: name 'count' is not defined
```

### Lambda functions

Python also supports lambda functions, which are small, anonymous functions that can be defined in a single line of code. They are often used for simple operations and can be passed as arguments to other functions.

Here's an example of a lambda function that squares a number:

```python
square = lambda x: x ** 2
print(square(3)) # Output: 9
```

### Built-in functions

Python comes with a wide range of built-in functions that can be used to perform common operations, such as `print()`, `len()`, and `range()`.

Here's an example of using the `len()` function to get the length of a string:

```python
string = 'Hello, world!'
length = len(string)
print(length) # Output: 13
```


# Different Ways to Call Function

### How to call a function in Python?

#### Directly Function Call

This is the simplest and most intuitive way:
```python
def test():
    print("This is a test")
test()
```

#### Use partial() Function

In the built-in library of `functools`, there is a partial method dedicated to generating partial functions.

```python
def power(x, n):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s

from functools import partial
power_2 = partial(power, n=2)
power_2(3)  # output: 9
power_2(4)  # output: 16
```

#### Use eval()

If you need to execute the function dynamically, you can use eval + string to execute the function.

```python
# demo.pyimport sys
def pre_task():
    print("running pre_task")
def task():
    print("running task")
def post_task():
    print("running post_task")
argvs = sys.argv[1:]
for action in argvs:
    eval(action)()
```
To execute:

```shell
$ python demo.py pre_task task post_task
running pre_task
running task
running post_task
```
#### Use getattr()

If you put all the functions in the class and define them as static methods, you can use `getattr()` to get and call them.

```python
import sys

class Task:
    @staticmethod
    def pre_task():
        print("running pre_task")
    @staticmethod
    def task():
        print("running task")
    @staticmethod
    def post_task():
        print("running post_task")
argvs = sys.argv[1:]
task = Task()
for action in argvs:
    func = getattr(task, action)
    func()
```

#### Use dict()

We all know that the object has a `__dict__()` magic method, it stores the properties and methods of any object. For example:

![dict](/img/dict.jpg)

You can call class method use` __dict__.get` :

```python
import sys

class Task:
    @staticmethod
    def pre_task():
        print("running pre_task")

func = Task.__dict__.get("pre_task")
func.__func__()# Output
```
```shell
$ python /tmp/demo.py
running pre_task
```

#### Use global()

```python
import sys

def pre_task():
    print("running pre_task")

def task():
    print("running task")

def post_task():
    print("running post_task")

argvs = sys.argv[1:]
for action in argvs:
    globals().get(action)()# Output
```
```shell
$ python /tmp/demo.py pre_task task post_task
running pre_task
running task
running post_task
```

#### Compile and Run From Text

You can define your function in a string, and use the `compile` function to compile it into byte code, and then used `exec` to execute it.

```python
pre_task = """
print("running pre_task")
"""
exec(compile(pre_task, '', 'exec'))# Or from a text file
with open('source.txt') as f:
    source = f.read()
    exec(compile(source, 'source.txt', 'exec'))
```

#### Use attrgetter()

In the built-in library of `operator`, there is a method for obtaining attributes, called `attrgetter`, execute after obtaining the function.


```python
from operator import attrgetter

class People:
    def speak(self, dest):
        print("Hello, %s" %dest)

p = People()
caller = attrgetter("speak")
caller(p)("Tony")
```
```shell
# Output
$ python /tmp/demo.py
Hello, Tony
```

#### Use methodcaller()

There is also a methodcaller method in operator :

```python
from operator import methodcaller

class People:
    def speak(self, dest):
        print("Hello, %s" %dest)

caller = methodcaller("speak", "Tony")
p = People()
caller(p)
```
```shell
# Output
$ python /tmp/demo.py
Hello, Tony
```
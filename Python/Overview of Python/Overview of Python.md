___
___
### Introduction to Python 
Python is a high level, interpreted, general purpose programming language. It can be used to build almost any type of application with the right set of libraries and tools. Additionally, python supports objects, modules, threads, exception-handling, and automatic memory management which help in modelling real-world problems and building applications to solve these problems. Python is a dynamic typed language, which executes code on the fly. For example, if we were to print out "1" + 2, in python, it will result in a type error as the data types of these two values are not the same whereas the same code in JavaScript would simply result in 12.  
#### Comments in Python 
We use comments in Python so that the complier can ignore whatever comes after the keyword. We use the `#` keyword in python to comment out an explanation or a block of code that we do not want the compiler to run.
#### Print in Python
The first thing that any programmer learns while learning any programming language is how to display HELLO WORLD. Python being a high level language uses a keyword called `print()`. The parameters of the print statement should be enclosed in quotes if we are printing a string. 
Here are few ways you can use the print statement. 
```python
print("HELLO WORLD") # prints HELLO WORLD
name = "Abhi" # variable named ABHI
print("Hello" + " My name is " + name) # prints Hello My name is Abhi
print(32+56) # prints 88
print(2**3) #prints the 2^3 = 8
print(29%2) # prints the modulo of 29%2 = 1
print("""This is an example 
of a multiline string 
statement that is used in python """) # multiline statememnt
```
#### Errors in Python
Python refers to mistakes as errors and will point out the location at which an error has occurred, There are two kinds of common errors that we would encounter while programming in python
- **Syntax Error**: If you ever encounter a syntax error here are the things you would need to check. Check if something is wrong with the way your program is written — punctuation that does not belong, a command where it is not expected, or a missing parenthesis can all trigger the same error. 
- **Name Error**: occurs when the Python interpreter sees a word it does not recognize. Code that contains something that looks like a variable but was never defined will throw a Name error.
#### Built-in Data types in Python 
There are different types of data types in Python. Some built-in Python data types are:
- **Numeric data types**: int, float, long, complex
- **String data types**: str
- **Sequence data types**: list, tuple, range
- **Binary data types**: bytes, byterarray, memoryview
- **Mapping data types**: dict
- **Boolean data types**: bool
- **Set data types**: set, frozenset
[Reference](https://www.digitalocean.com/community/tutorials/python-data-types)
### Functions in Python 
___

When we want to re-use a particular code repeatedly we use the user defined functions which will allow us to call the function in one line. 
###### Defining a function
A function consists of many parts, we first will start off by defining a function. 
```python
def function_name():
	pass
```
###### Calling a function:
The process of executing the code inside the body of a function is known as calling the function, we call the function by:
```python
function_name()
function_name(argument1)
function_name(argument1, argument2, argument3)
```

###### Types of Arguments:
 There are three kinds of Arguments that we would use in a function call:
 1. Positional argument: The assignments of the variable depends on the position of the argument. The variables NEED to be assigned in one particular order.
 2. Keyword argument: When we use the keyword for the variables to assign values. The order of the arguments do not matter.
 3. Default argument: These are optional arguments, the code will run even if the argument is not defined and will take the default parameter. 

> Being able to write python functions in one line is called [[Lambda Functions]]
### Loops
___
#### 1. FOR loop:
In a `for` loop, we will know in advance how many times the loop will need to iterate because we will be working on a collection with a predefined length. With `for` loops, on each iteration, we will be able to perform an action on each element of the collection.
```Python
# Syntax
for <var> in <collection>:
	<action>
for <var> in <range>:
	<action>
```

#### 2. While loop:
In a `while` loop, the loop performs the actions as long as the given condition is satisfied. 
```Python
while <condition>:
	<action>
```

#### 3. Loop control: Break 
Sometimes we need to break out of the loop once we are satisfied with the performance or the result of our action. In this case, we do not want to iterate through the entire loop. We use the keyword `break` to stop our loop. 
```Python
for <var> in <collection>:
	if <var> == <result>:
		break
```
This code will break out of the loop once our condition is satisfied. 

#### 4. Loop control: Continue
While the `break` control statement will come in handy, there are other situations where we don’t want to end the loop entirely. What if we only want to skip the current iteration of the loop?
For example, when we want to print out only the positive numbers in a list. 
```Python
for <var> in <collection>:
	if <var> < 0:
		continue
```

### Files in Python 
___
Another important thing to work on in python are the [[Files]] which help us write, read, append files from different forms such as CSV, JSON. 


## Classes in Python 
____
[[Class]] in python is a template for a data type. It describes the kinds of information that the class will hold and how a programmer will interact with that data.


## Modules in Python
___
A module is a collection of Python declarations intended broadly to be used as a tool. Modules are also often referred to as “libraries” or “packages” — a package is really a directory that holds a collection of modules.
Usually to use module in a file, we use the syntax:
```Python
from module_name import object_name
```

==Often, a library will include a lot of code that you don’t need that may slow down your program or conflict with existing code. Because of this, it makes sense to only import what you need.==

#### Random module 
One of the most common modules in Python is the `random` module. We will work with two common functions of random:
- `random.choice()`: Which takes a list as an argument and returns a number from the list. 
- `random.randint()`: Which takes two numbers as arguments and generates a random number between the two numbers you passed.

#### decimal module 
When it comes to calculating the sum of two floating point numbers, the built-in athematic module results in a weirdly formatted number, Ex:
```Python
cost_of_gum = 0.10  
cost_of_gumdrop = 0.35  
  
cost_of_transaction = cost_of_gum + cost_of_gumdrop  
# Returns 0.44999999999999996
```


Instead of this, we use the decimal module taking the same example below
```Python
from decimal import Decimal 

cost_of_gum = 0.10  
cost_of_gumdrop = 0.35  
  
cost_of_transaction = cost_of_gum + cost_of_gumdrop 
# Returns 0.45

```

#### Files as modules 
One file will not have access to the contents of another file even if it is in the same directory. If we want to import a user-defined function from another file to our current file, we import that file to the current file.
```Python
from <file_name> import <function_name>
```

#Python


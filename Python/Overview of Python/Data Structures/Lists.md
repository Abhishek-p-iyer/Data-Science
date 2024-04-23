___

# Introduction to Lists in Python
## One-Dimensional Lists
A collection of sequential data enclosed in square brackets are called a list in python. The values in a list are separated by a comma. A list in Python can contain multiple data types simultaneously. We can store a string and a numerical digit in a list. 
For example:
```Python
list = [23, "Alpha", False, 0.5]
```
#### 1. Accessing elements in a list 
To access a particular element from a list, we use the index of the element. Lists in python are zero indexed which means that the first element will have an index of 0. To access the first element of the above example, we simply `print(list[0])` which will give us a result of 23, we access the last element in the list by simply `print(list[3])` as our first element is indexed at 0. 
	Now to get the last element of the list, we could also use a negative index. -1 is considered to be the end of the list. When we `print(list[-1])`, we get `0.5` as the output. 

#### 2. Modifying List Elements
If we have a list and we want to modify a particular element of the list:
Lets take an example list:
```pYTHON
garden = ["Tomatoes", "Green Beans", "Cauliflower", "Grapes"]
```
If we decide that we no longer want to grow Green Beans and replace them with Raspberries, we simply access the element that we want to modify and assign another variable to it. 
```Python
garden[1] = "Raspberries"
print(garden)
# OUTPUT: ["Tomatoes", Raspberries", "Cauliflower", "Grapes"]
```

We could additionally add another list by simply using the `+` operator. 
```Python
list1 = ["Abhi", "Guy", "Tony"]
list2 = [23, 35, 37]
print(list1 + list2)
# OUTPUT: ["Abhi", "Guy", "Tony", 23, 35, 37]
```

#### 3. Slicing in Python
When we want to extract a portion of a list, we call it slicing. We use the syntax `list[start:end]` to slice a list. The start is the first element we want to slice and the end is one element after the last element. 
- If we want to slice the first `n` number of elements, we follow the syntax `list[:n]`
- If we want to slice the last `n` number of elements in a list, we use the syntax `list[-n:]`
```Python
letters = ["a", "b", "c", "d", "e", "f", "g"]
sliced_list = letters[1:6]  
print(sliced_list)
# OUTPUT: ["b", "c", "d", "e", "f"]

sliced_list2 = letters[:2]
print(sliced_list2)
# OUTPUT: ["a", "b"]

sliced_list3 = letters[-2:]
print(sliced_list3)
# OUTPUT: ["f", "g"]
```

#### 4. List Methods
A method is a built-in functionality that will help us create, manipulate and delete data. For lists, the methods will follow the form of  `list_name.method()`. 
Few of the [[List Methods]] are given below:
1. Append(): Adds an element to the end of the list
2. Remove(): Removes the last element from the list
3. Insert(): Adds an element in a particular index
4. Pop(): Removes a particular element in a specified index
5. Len(): Returns the length
6. Count(): Counts the number of occurrences of an element 
7. Sort(): Sorts the list

___
#### ==List Comprehension==

List comprehension is an elegant way to write code in which we can combine the aspects of the control loops and list into one line. 
We'll understand list comprehension with an example. 
```Python
# Code to double the elemets of a list 
list = [1,2,3,4,5]
new_list = []
# Without using List comprehension 
for ele in list:
	new_list.append(ele*2)
print(new_list)

# Using List comprehension 
new_list2 = [ele*2 for ele in list]
print(new_list2)
```
We can see that using list comprehension, we were able to compress the two lines of code into one. This is easier to read and understand and as well as easier to code. Now that we know what List comprehension is, lets take a deeper dive into the syntax and the different ways in which we write code using list comprehension. 

```Python
# SYNTAX
list_name = [<expression> for <ele> in <collection>]
```

We can now expand this to include conditional logic statements. 
List comprehension is written differently when we have only the `IF` statement and when we have both the `IF` and the `ELSE` statement. 
- **Only the IF statement**:  
Lets take an example of doubling only the positive integers in a list. 
```Python
# Syntax
list_name = [<expression> for <ele> in <collection> if <condition>]
# Code
list1 = [12, 4, -4, -2, -9, 1, 2, -9, -2]
positive_int_doub = [num*2 for num in list1 if num>0]
```
- **Using the IF and ELSE statement**: 
Lets take an example of doubling the positive integers and tripling the negative ones. 
```Python 
# Syntax
list_name = [<expression> if <condition> else <expression> for <ele> in <collection>]

# Code
list1 = [12, 4, -4, -2, -9, 1, 2, -9, -2]
new_list = [num*2 if num> 0 else num*3 for num in list1]
print(new_list)
```

___

___
#### ==Combining Lists: The ZIP Function==
Suppose that we already have a list of names and a list of heights:
```Python
names = ["Jenny", "Alexus", "Sam", "Grace"]  
heights = [61, 70, 67, 64]
```
If we wanted to create a nested list that paired each name with a height, we could use the built-in function `zip()`. 
The `zip()` function takes two (or more) lists as inputs and returns an _object_ that contains a list of pairs. Each pair contains one element from each of the inputs. This is how we would do it for our `names` and `heights` lists:
```Python
names_and_heights = zip(names, heights)
# OUTPUT: <zip object at 0x7f1631e86b48>
```
If we were to then examine this new variable `names_and_heights`, we would find that an address of the list is being printed. This _zip object_ contains the location of this variable in our computer’s memory.
```Python
converted_list = list(names_and_heights)  
print(converted_list)
# OUTPUT: [('Jenny', 61), ('Alexus', 70), ('Sam', 67), ('Grace', 64)]
```

> Our data set has been converted from a zip memory object to an actual list
 >  Our inner lists don’t use square brackets around the values. This is because they have been converted into tuples.

___

### Two-Dimensional Lists
When a list consists of another list, it is called a 2D list. Lets take the example of class heights:
- Nicolle is 61 inches tall
- Ava is 42 inches tall
- Sam is 56 inches tall
- Jack is 77 inches tall
- Mia is 53 inches tall
- Akash is 64 inches tall
This can be represented by
```python
heights = [["Nicolle", 61],["Ava",42],["Sam", 56],["Jack", 77],["Mia", 53],["Akash",64]]
```
#### Accessing elements in a list 
Accessing elements in a two dimensional list is similar to accessing lists of one dimension. We add another square bracket for the extra dimension. Lets say we want to access the height of Ava. 
The name, height pair of Ava lies in the 1st index and the height lies in the 1st index again. Hence the height of ava would be ``heights[1][1]`. 
#### Modifying List Elements
Now lets say we want to modify Sam's height as there was an error in the scale while measuring his height. 
We use the following code:
```Python
heights[2][1] = 58
print(heights)
# OUTPUT: [["Nicolle", 61],["Ava",42],["Sam", 58],["Jack", 77],["Mia", 53],["Akash",64]]
```

#Python
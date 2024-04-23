___
Words and sentences are fundamental to how we communicate, so it follows that we’d want our computers to be able to work with words and sentences as well. A string can be thought of as a list and each character has an index. 
Not only can we select a single character from a string, but we can also select entire chunks of characters from a string. We can do this with the following syntax:
```Python
string[first_index: last_index]
```

- We can find the length of the string using the `len()` function and slice our string accordingly. We can also us negative indices that count back from the last character. 
- Strings are similar to lists, we can iterate through lists using `for` and `while` loops.
- We can check if a character is present in a string with the `in` statement. 
- When we want to use the quotes for a string we use escape characters to make sure that python does not confuse the quotes to be the end of the string. 
```Python
# Without using Escape character:
favorite_fruit_conversation = "He said, "blueberries are my favorite!"" 

# Using escape character:
favorite_fruit_conversation = "He said, \"blueberries are my favorite!\""

# IN statement 
Sytnax: letter in word 
print("e" in "blue")
# OUTPUT: TRUE
```

> Strings are _immutable_. We create a new string every time we perform operations on the string.  

## String Methods
[[String Methods]] are built-in functions in python that makes us easier to manipulate data. They allow us to perform complicated tasks with ease and efficiency. These string methods allow you to change the case of a string, split a string into many smaller strings, join many small strings together into a larger string, and allow you to neatly combine changing variables with string outputs.

>String methods follow the syntax: `string_name.string_method(arguments)`.

We will be looking into the following methods:
1. Splitting a string 
2. Joining strings 
3. Strip
4. Formatting methods such as upper, lower, title 
5. Replace 
6. Find 
7. Format 

#Python
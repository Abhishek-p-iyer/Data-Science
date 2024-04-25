___
A Lambda function is a shorthand way of writing a function, it is a one-line code that gets the work done. 
The syntax for Lambda functions follows this order: `<var> = lambda <input>: <return_statement>`
## Lambda functions using the IN keyword 

The `IN` statement in Python can be used to check if a substring is present in the string. We are going to use this to define our one line function. 
```Python 
contains_a  = lambda word: 'a' in word

print(contains_a("banana"))
print(contains_a("apple"))
print(contains_a("cherry"))

# OUTPT => True, True, False
```


## Checking if the string is long 
Create a lambda function named `long_string` that takes an input `word` and returns `True` if the string has over 12 characters in it. Otherwise, return `False`.
```Python 
long_string = lambda word: True if len(word)>12 else False

print(long_string("short"))
print(long_string("photosynthesis"))

# OUTPUT => False, True
```

## Ends with A
Create a lambda function named `ends_in_a` that takes an input `str` and returns `True` if the last character in the string is an `a`. Otherwise, return `False`.
```Python 
ends_in_a = lambda str: True if str[-1]=='a' else False

print(ends_in_a("data"))
print(ends_in_a("aardvark"))
# OUTPUT => True, False 
```

## Even/Odd
To check if a number if even or odd 
```Python 
even_or_odd = lambda num: 'even' if num%2 == 0 else "odd"
print(even_or_odd(23))
print(even_or_odd(22))

# OUTPUT => odd, even 
```

#Python
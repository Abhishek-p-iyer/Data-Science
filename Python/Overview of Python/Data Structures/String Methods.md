____

### 1. Formatting Methods
There are three string methods that can change the casing of the string and they are `.lower()`, `.upper()`, `.title()`. 
```Python 
song = "the best of times"
song = song.upper()
print(song)
song = song.lower()
print(song)
song = song.title()
print(song)
```

### 2. Splitting a string 
`.split()` is performed on a string, takes one argument, and returns a list of substrings found between the given argument. If we do not provide a delimiter, the function takes the default argument to be a space. 
We can split strings using escape sequences as well, such as newline (`\n`) or a tab (`\t`).  
It follows the syntax given below: 
```Python
string_name.split(delimiter)

# Example
man_its_a_hot_one = "Like seven inches from the midday sun"  
print(man_its_a_hot_one.split())  
# => ['Like', 'seven', 'inches', 'from', 'the', 'midday', 'sun']
```
You can even split a string on a character,  Lets consider the following example. 
```Python
greatest_guitarist = "santana"  
print(greatest_guitarist.split('n'))  
# => ['sa', 'ta', 'a']

print(greatest_guitarist.split('a'))  
# => ['s', 'nt', 'n', '']
```
> Here we notice that when the string ends with the delimiter character, it creates an empty string at the end. 

### 3. Joining Strings
Now that we've split the string, we use the `.join()` method to put them back together. it is essentially the opposite of `.split()`, it _joins_ a list of strings together with a given delimiter. The syntax of `.join()` is:
```Python
'delimiter'.join(list_you_want_to_join)

# Example 
my_munequita = ['My', 'Spanish', 'Harlem', 'Mona', 'Lisa']  
print(' '.join(my_munequita))  
# => 'My Spanish Harlem Mona Lisa'
```

### 4. Stripping a string
When working with strings that come from real data, you will often find that the strings aren’t super clean. You’ll find lots of extra whitespace, unnecessary linebreaks, and rogue tabs.
Python provides a great method for cleaning strings: `.strip()`. Stripping a string removes all whitespace characters from the beginning and end. Consider the following example:
```Python
featuring = "           rob thomas                 "  
print(featuring.strip())  
# => 'rob thomas'
```

All the whitespace on either side of the string has been stripped, but the whitespace in the middle has been preserved.
You can also use `.strip()` with a character argument, which will strip that character from either end of the string.
```Python
featuring = "!!!rob thomas       !!!!!"  
print(featuring.strip('!'))  
# => 'rob thomas       '
```
By including the argument `'!'` we are able to strip all of the `!` characters from either side of the string. Notice that now that we’ve included an argument we are no longer stripping whitespace, we are ONLY stripping the argument.

### 5. Replace
Replace takes two arguments and replaces all instances of the first argument in a string with the second argument. The syntax is as follows
Syntax:
`string_name.replace(substring_being_replaced, new_substring)`
```Python
with_spaces = "You got the kind of loving that can be so smooth"  
with_underscores = with_spaces.replace(' ', '_')  
print(with_underscores)
```

### 6. Find 
Another interesting string method is `.find()`. `.find()` takes a string as an argument and searching the string it was run on for that string. It then returns the first _index value_ where that string is located.
```Python
print('smooth'.find('t'))  
# => '4'
```

### 7. format()
Python also provides a handy string method for including variables in strings. This method is `.format()`. `.format()` takes variables as an argument and includes them in the string that it is run on. You include `{}` marks as placeholders for where those variables will be imported.
```Python
"My favorite song is {} by {}.".format(The best of times, Dream Theater)
# => My favorite song is The best of Times by Dream Theater
```

#Python
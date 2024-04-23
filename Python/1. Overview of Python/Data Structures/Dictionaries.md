___

### Introduction

Dictionaries are unordered key-value pairs. It provides a way where we can map pieces of data to each other so we can quickly find values that are associated with one another. 
Suppose we want to store the prices of various items sold at a cafe:

- Avocado Toast is 6 dollars
- Carrot Juice is 5 dollars
- Blueberry Muffin is 2 dollars
We create a dictionary called menu to store the values of the data.

```Python
menu = {"avocado toast": 6, "carrot juice": 5, "blueberry muffin": 2}
```

Values can be of any type. We can use a string, a number, a list, or even another dictionary as the value associated with a key. 
The values of a dictionary could be a list or another dictionary but, these types of data cannot be keys and will result in a `Type Error`. 

> Dictionaries in Python rely on each key having a _hash value_, a specific identifier for the key. If the key can change, that hash value would not be reliable. So the keys must always be unchangeable, hashable data types, like numbers or strings.


### Adding a Key-Value pair
If we want to add a key-value pair to an already existing dictionary, we can use this syntax:
```Python
dictionary[key] = value
```

Lets take our menu example again, to add another item in our menu. 
```Python
menu = {"avocado toast": 6, "carrot juice": 5, "blueberry muffin": 2}
menu["chesscake"] = 9
print(menu)

# => {"avocado toast": 6, "carrot juice": 5, "blueberry muffin": 2, "cheescake":9}
```

### Adding multiple Key
If we want to add more than one key-value pair into our dictionary, we use the `update()` method. 
```Python
sensors = {"living room": 21, "kitchen": 23, "bedroom": 20}
sensors.update({"pantry":20, "guest room": 25, "patio": 35})
print(sensors)

# => {"living room": 21, "kitchen": 23, "bedroom": 20, "pantry": 22, "guest room": 25, "patio": 34}
```

### Dict Comprehension 
Let’s say we have two lists that we want to combine into a dictionary, like a list of students and a list of their heights, in inches::
```Python 
names = ['Jenny', 'Alexus', 'Sam', 'Grace']  
heights = [61, 70, 67, 64]

# SYNTAX
dict = {key:value for key, value in zip(<list1>, <list2>)}

students = {key:value for key,value in zip(names, heights)}
```

### Get a Key
There are two ways in which we can access a value from a dictionary. The first one being the more direct way, where we know that a particular key DOES infact exist in the dictionary. The syntax for which is simply `print(dictionary[key])`. We use this to determine the value when we know for sure that the key exists in the dictionary. Now if the key does not exist, python will produce an error.
To make sure that we do not end up getting an error, we use the `.get()` method that is built into python. If a key does not exist, the function will return `None`.  
Once you have a dictionary, you can access the values in it by providing the key.
```Python 
senors = {"living room": 21, "kitchen": 23, "bedroom": 20, "pantry": 22, "guest room": 25, "patio": 34}
print(sensors["kitchen"])
# OUTPUT: => 23
print(sensors["bedroom"])
# OUTPUT =>20

print(sensors.get("pantry"))
# => 22
print(sensors.get("living"))
# =>None
```

### Deleting a Key
We can delete a key by using the `.pop()` method. We can also print a default value if the key does not exist.
```Python
senors = {"living room": 21, "kitchen": 23, "bedroom": 20, "pantry": 22, "guest room": 25, "patio": 34}

sensors.pop("living room", "Does not exist")
# OUTPUT => {"kitchen": 23, "bedroom": 20, "pantry": 22, "guest room": 25, "patio": 34}
sensors.pop("living room", "Does not exist")
# OUTPUT: => Does not exist
```

### Get ALL Keys
Sometimes we want to operate on all of the keys in a dictionary. We can use the built-in `list()` function to get a list of all of the keys in a dictionary. For example, if we have a dictionary of students in a math class and their grades:
```Python
test_scores = {"Grace":[80, 72, 90], "Jeffrey":[88, 68, 81], "Sylvia":[80, 82, 84], "Pedro":[98, 96, 95], "Martin":[78, 80, 78], "Dina":[64, 60, 75]}

print(list(test_scores))  
# Prints ["Grace", "Jeffrey", "Sylvia", "Pedro", "Martin", "Dina"]
```

Dictionaries also have a `.keys()` method that returns a `dict_keys` object. A `dict_keys` object is a _view_ object, which provides a look at the current state of the dictionary, without the user being able to modify anything. The `dict_keys` object returned by `.keys()` is a set of the keys in the dictionary. You cannot add or remove elements from a `dict_keys` object, but it can be used in the place of a list for iteration:
```Python
for student in test_scores.keys():  
	print(student)
# PRINTS
# Grace  
# Jeffrey  
# Sylvia  
# Pedro  
# Martin  
# Dina
```

### Get ALL Values
Dictionaries have a `.values()` method that returns a `dict_values` object (just like a `dict_keys` object but for values!) with all of the values in the dictionary. It can be used in the place of a list for iteration:
```Python
test_scores = {"Grace":[80, 72, 90], "Jeffrey":[88, 68, 81], "Sylvia":[80, 82, 84], "Pedro":[98, 96, 95], "Martin":[78, 80, 78], "Dina":[64, 60, 75]}  
  
for score_list in test_scores.values():  
	print(score_list)


'''
[80, 72, 90]  
[88, 68, 81]  
[80, 82, 84]  
[98, 96, 95]  
[78, 80, 78]  
[64, 60, 75]
'''
```

### Get all items 
You can get both the keys and the values with the `.items()` method. Like `.keys()` and `.values()`, it returns a `dict_list` object. Each element of the `dict_list` returned by `.items()` is a tuple consisting of `(key,value)`.

```Python
biggest_brands = {"Apple": 184, "Google": 141.7, "Microsoft": 80, "Coca-Cola": 69.7, "Amazon": 64.8}  
  
for company, value in biggest_brands.items():  
	print(company + " has a value of " + str(value) + " billion dollars. ")

"""
Apple has a value of 184 billion dollars.  
Google has a value of 141.7 billion dollars.  
Microsoft has a value of 80 billion dollars.  
Coca-Cola has a value of 69.7 billion dollars.  
Amazon has a value of 64.8 billion dollars.
"""
```

#Python
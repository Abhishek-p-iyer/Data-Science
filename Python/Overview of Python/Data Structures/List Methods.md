___
##### Append():
The append method is used when we want to add a particular element to the list. The syntax for which is `list_name.append(<element>)`. 
```Python
list1 = ["Abhi", "Guy", "Tony"]
list1.append("Jake")
print(list1)
# OUTPUT: ["Abhi", "Guy", "Tony","Jake"]
```
##### Remove():
The remove method removes a particular element from the list. The syntax for which is `list_name.Remove(<element_name>)`. ==ONLY THE FIRST MATCHING ELEMENT IS REMOVED. ==
```Python
list1 = ["Abhi", "Guy", "Tony"]
list1.remove("Tony")
print(list1)
# OUTPUT: ["Abhi", "Guy"]
```

##### Insert():
 The insert method is used to insert an element into a specified position in a list whereas the append method appends the element to the last position.
```Python
list1 = ["Abhi", "Guy", "Tony"]
list1.insert(2, "Hellen")
print(list1)
# OUTPUT: ["Abhi", "Guy", "Hellen", "Tony"]
```
> It is important to note that the insert method requires two positions, the first one being the numerical one that represents the position in which the element should be inserted in and then comes the element we want to insert. 
> When we use the insert method, all the elements from the specified index is shifted to the right and the element is inserted.

##### Pop():
 The pop method removes the element from the specified position. It requires one argument, the index for the element that needs to be removed. 
```Python
list1 = ["Abhi", "Guy", "Tony"]
list1.pop(1)
print(list1)
# OUTPUT:  ["Abhi", "Tony"]
```

##### Len():
It returns the length of a list. 
```Python
list1 = ["Abhi", "Guy", "Tony"]
val = len(list1)
print(val)
# OUTPUT: 3
```

##### Count():
We use the count method when we want to find the number of repetitions of an element in a list.
```Python
list1 = [123, 867, 895, 895, 123, 895]
val = list1.count(895)
print(val)
```

##### Sort():
The sort method is used to sort a particular list. We can sort the list in ascending or descending order.  
```Python
names = ["Xander", "Buffy", "Angel", "Willow", "Giles"]
names.sort()  
print(names)
# OUTPUT: ['Angel', 'Buffy', 'Giles', 'Willow', 'Xander']

names.sort(reverse=True)  
print(names)
# output: ['Xander', 'Willow', 'Giles', 'Buffy', 'Angel']
```
#Python
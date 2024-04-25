____

## Introduction
- A class is a template for a datatype. It describes the kinds of information that the class will hold and how a programmer interacts with it. We define a class using a `class` keyword. 
- The class doesn't accomplish anything simply by being defined. We must create an instance of the class. This instance of the class is called an object. 
- Class variable: A class variable is the data we want to be available to every instance of a class. The class variable will be the same for every instance of the variable. 
```Python
class Musician: #Defing a class
	title = "Rockstar" #Class variable

drummer = Musician() #Instantiation of a class
```

Above we defined the class `Musician`, then instantiated `drummer` to be an object of type `Musician`We then printed out the drummer’s `.title` attribute, which is a class variable that we defined as the string “Rockstar”.
If we defined another musician, Ex:`guitarist = Musician()` they would have the same `.title` attribute.


## Methods 
_Methods_ are functions that are defined as part of a class. The first argument in a method is always the object that is calling the method. Convention recommends that we name this first argument `self`. Methods always have at least this one argument.
We define methods similarly to functions, except that they are indented to be part of the class.

#### Methods without arguments
```Python
class Dog:  
  dog_time_dilation = 7  
  
  def time_explanation(self):  
    print("Dogs experience {} years for every 1 human year.".format(self.dog_time_dilation))  
  
pipi_pitbull = Dog()  
pipi_pitbull.time_explanation()  
# Prints "Dogs experience 7 years for every 1 human year."
```

Above we created a `Dog` class with a `.time_explanation()` method that takes one argument, `self`, which refers to the object calling the function. We created a `Dog` named `pipi_pitbull` and called the `.time_explanation()` method on our new object for Pipi.

#### Methods with arguments
```Python
class DistanceConverter: 
	kms_in_a_mile = 1.609
	def how_many_kms(self, miles)
		return miles*self.kms_in_a_mile

converter = DistanceConverter()
kms_in_5_miles = converter.kms_in_a_mile(5)
print(kms_in_5_miles)
```
Above we defined a `DistanceConverter` class, instantiated it, and used it to convert 5 miles into kilometers. Notice again that even though `.how_many_kms()` takes two arguments in its definition, we only pass `miles`, because `self` is implicitly passed (and refers to the object `converter`).

## Constructers
There are several methods that we can define in a Python class that have special behavior. These methods are sometimes called “magic,” because they behave differently from regular methods. Another popular term is [_dunder methods_](https://www.codecademy.com/resources/docs/python/dunder-methods?page_ref=catalog), so-named because they have two underscores (double-underscore abbreviated to “dunder”) on either side of them.

The first dunder method we’re going to use is the [`__init__()`](https://www.codecademy.com/resources/docs/python/dunder-methods/init?page_ref=catalog) method (note the two underscores before and after the word “init”). This method is used to _initialize_ a newly created object. It is called every time the class is instantiated.
Methods that are used to prepare an object being instantiated are called _constructors_. The word “constructor” is used to describe similar features in other object-oriented programming languages, but programmers who refer to a constructor in Python are usually talking about the `__init__()` method.

```Python
class Shouter:  
  def __init__(self, phrase):  
    # make sure phrase is a string  
    if type(phrase) == str:  
  
      # then shout it out  
      print(phrase.upper())  
  
shout1 = Shouter("shout")  
# prints "SHOUT"  
  
shout2 = Shouter("shout")  
# prints "SHOUT"  
  
shout3 = Shouter("let it all out")  
# prints "LET IT ALL OUT"
```

## Instance Variable 
We’ve learned so far that a class is a schematic for a data type and an object is an instance of a class, but why is there such a strong need to differentiate the two if each object can only have the methods and class variables the class has? This is because each instance of a class can hold different kinds of data.
The data held by an object is referred to as an _instance variable_. Instance variables aren’t shared by all instances of a class — they are variables that are specific to the object they are attached to.

## Attribute Functions
Instance variables and class variables are both accessed similarly in Python. This is no mistake, they are both considered _attributes_ of an object. If we attempt to access an attribute that is neither a class variable nor an instance variable of the object Python will throw an `AttributeError`.

```Python
class NoCustomAttributes:  
  pass  
  
attributeless = NoCustomAttributes()  
  
try:  
  attributeless.fake_attribute  
except AttributeError:  
  print("This text gets printed!")  
  
# prints "This text gets printed!"
```

What if we aren’t sure if an object has an attribute or not? [`hasattr()`](https://www.codecademy.com/resources/docs/python/built-in-functions/hasattr?page_ref=catalog) will return `True` if an object has a given attribute and `False` otherwise. If we want to get the actual value of the attribute, [`getattr()`](https://www.codecademy.com/resources/docs/python/built-in-functions/getattr?page_ref=catalog) is a Python function that will return the value of a given object and attribute. In this function, we can also supply a third argument that will be the default if the object does not have the given attribute.
The syntax and parameters for these functions look like this:
`hasattr(object, “attribute”)` has two parameters:
- _object_ : the object we are testing to see if it has a certain attribute
- _attribute_ : name of attribute we want to see if it exists
`getattr(object, “attribute”, default)` has three parameters (one of which is optional):
- _object_ : the object whose attribute we want to evaluate
- _attribute_ : name of attribute we want to evaluate
- _default_ : the value that is returned if the attribute does not exist (note: this parameter is **optional**)

```Python
hasattr(attributeless, "fake_attribute")  
# returns False  
  
getattr(attributeless, "other_fake_attribute", 800)  
# returns 800, the default value
```

Above, we checked if the `attributeless` object has the attribute `.fake_attribute`. Since it does not, `hasattr()` returned `False`. After that, we used `getattr` to attempt to retrieve `.other_fake_attribute`. Since `.other_fake_attribute` isn’t a real attribute on `attributeless`, our call to `getattr()` returned the supplied default value `800`, instead of throwing an `AttributeError`.

Creating a safe search engine entry:

```Python
class SearchEngineEntry:  
  secure_prefix = "https://"  
  def __init__(self, url):  
    self.url = url  
  
  def secure(self):  
    return "{prefix}{site}".format(prefix=self.secure_prefix, site=self.url)  
  
codecademy = SearchEngineEntry("www.codecademy.com")  
wikipedia = SearchEngineEntry("www.wikipedia.org")  
  
print(codecademy.secure())  
# prints "https://www.codecademy.com"  
  
print(wikipedia.secure())  
# prints "https://www.wikipedia.org"
```

## String Representation

One of the first things we learn as programmers is how to print out information that we need for debugging. Unfortunately, when we print out an object we get a default representation that seems fairly useless.

```Python
class Employee():  
	def __init__(self, name):    
	self.name = nameargus = Employee("Argus Filch")print(argus)
# prints "<__main__.Employee object at 0x104e88390>"
```

This default string representation gives us some information, like where the class is defined and our computer’s memory address where this object is stored, but is usually not useful information to have when we are trying to debug our code.

We learned about the dunder method `__init__()`. Now, we will learn another dunder method called [`__repr__()`](https://www.codecademy.com/resources/docs/python/dunder-methods/repr?page_ref=catalog). This is a method we can use to tell Python what we want the _string representation_ of the class to be. `__repr__()` can only have one parameter, `self`, and must return a string.

In our `Employee` class above, we have an instance variable called `.name` that should be unique enough to be useful when we’re printing out an instance of the `Employee` class.

```Python
class Employee():  
	def __init__(self, name):    
		self.name = name  
	def __repr__(self):    
		return self.nameargus = Employee("Argus Filch")print(argus)
# prints "Argus Filch"
```

We implemented the `__repr__()` method and had it return the `.name` attribute of the object. When we printed the object out it simply printed the `.name` of the object! Cool!





























#Python 
___
___
Files are opened in Python with the `with` keyword.  The `with` keyword invokes something called a _context manager_ for the file that we’re calling `open()` on. This context manager takes care of opening the file when we call `open()` and then closing the file after we leave the indented block.
The `with` syntax replaces older ways to access files where you need to call [`.close()`](https://www.codecademy.com/resources/docs/python/files/close?page_ref=catalog) on the file object manually.
### 1. Reading a File 
```Python
with open('real_cool_document.txt') as cool_doc:  
  cool_contents = cool_doc.read()  
print(cool_contents)
```
This opens a file object called `cool_doc` and creates a new indented block where you can read the contents of the opened file. We then read the contents of the file `cool_doc` using `cool_doc.read()` and save the resulting string into the variable `cool_contents`.
### 2. Iterating through lines 
When we read a file, we might want to grab the whole document in a single string, like `.read()` would return. But what if we wanted to store each line in a variable? We can use the `.readlines()` function to read a text file line by line instead of having the whole thing. Suppose we have a file:

```Python
with open('<file_name>.txt') as <var>:  
  for line in <var>.readlines():  
    print(line)
```
The above script creates a temporary file object that points to the file filename.  It then iterates over each line in the document and prints the entire file out.
### 3. Reading a line 
Sometimes you don’t want to iterate through a whole file. For that, there’s a different file method, `.readline()`), which will only read a single line at a time.
```Python
with open('just_the_first.txt') as first_line_doc:
  first_line = first_line_doc.readline()
  print(first_line)
```
### 4. Writing a file 
```Python
with open('generated_file.txt', 'w') as gen_file:  
  gen_file.write("What an incredible file!")
```
Here we pass the argument `'w'` to `open()` in order to indicate to open the file in write-mode. The default argument is `'r'` and passing `'r'` to `open()` opens the file in read-mode as we’ve been doing.
### 5. Appending to file 
 Instead of opening the file using the argument `'w'` for write-mode, we open it with `'a'` for append-mode. If we have a generated file with the following contents:
```Python
with open('generated_file.txt', 'a') as gen_file:  
  gen_file.write("\n... <text>")
```
### 6. Reading and Writing a CSV file 
CSV files contain commas and even though we can read these lines as text without a problem, there are ways to access the data in a format better suited for programming purposes.  In Python we can convert that data into a dictionary using the `csv` library’s `DictReader` object. 
The users.csv table looks like
```csv
Name,Username,Email  
Roger Smith,rsmith,wigginsryan@yahoo.com  
Michelle Beck,mlbeck,hcosta@hotmail.com  
Ashley Barker,a_bark_x,a_bark_x@turner.com  
Lynn Gonzales,goodmanjames,lynniegonz@hotmail.com
```

Here’s how we’d create a list of the email addresses of all of the users in the above table:
```Python
import csv  
  
list_of_email_addresses = []  
with open('users.csv', newline='') as users_csv:  
  user_reader = csv.DictReader(users_csv)  
  for row in user_reader:  
    list_of_email_addresses.append(row['Email'])
```
In the above code we first import our `csv` library, which gives us the tools to parse our CSV file. We then create the empty list `list_of_email_addresses` which we’ll later populate with the email addresses from our CSV. Then we open the **users.csv** file with the temporary variable `users_csv`.

We pass the additional keyword argument `newline=''` to the file opening `open()` function so that we don’t accidentally mistake a line break in one of our data fields as a new row in our CSV. After opening our new CSV file we use `csv.DictReader(users_csv)` which converts the lines of our CSV file to Python dictionaries which we can use access methods for. The keys of the dictionary are, by default, the entries in the first line of our CSV file. Since our CSV’s first line calls the third field in our CSV “`Email`“, we can use that as the key in each row of our `DictReader`.
When we iterate through the rows of our `user_reader` object, we access all of the rows in our CSV as dictionaries (except for the first row, which we used to label the keys of our dictionary). By accessing the `'Email'` key of each of these rows we can grab the email address in that row and append it to our `list_of_email_addresses`.

### 7. Writing a CSV File
```Python
big_list = [{'name': 'Fredrick Stein', 'userid': 6712359021, 'is_admin': False}, {'name': 'Wiltmore Denis', 'userid': 2525942, 'is_admin': False}, {'name': 'Greely Plonk', 'userid': 15890235, 'is_admin': False}, {'name': 'Dendris Stulo', 'userid': 572189563, 'is_admin': True}]  
  
import csv  
  
with open('output.csv', 'w') as output_csv:  
  fields = ['name', 'userid', 'is_admin']  
  output_writer = csv.DictWriter(output_csv, fieldnames=fields)  
  
  output_writer.writeheader()  
  for item in big_list:  
    output_writer.writerow(item)
```

In our code above we had a set of dictionaries with the same keys for each, a prime candidate for a CSV. We import the `csv` library, and then open a new CSV file in write-mode by passing the `'w'` argument to the `open()` function.

We then define the fields we’re going to be using into a variable called `fields`. We then instantiate our CSV writer object and pass two arguments. The first is `output_csv`, the file handler object. The second is our list of fields `fields` which we pass to the keyword parameter `fieldnames`
Now that we’ve instantiated our CSV file writer, we can start adding lines to the file itself! First we want the headers, so we call `.writeheader()` on the writer object. This writes all the fields passed to `fieldnames` as the first row in our file. Then we iterate through our `big_list` of data. Each `item` in `big_list` is a dictionary with each field in `fields` as the keys. We call `output_writer.writerow()` with the `item` dictionaries which writes each line to the CSV file.
### 8. Reading a Writing a JSON file 
Lets say the json file looks like 
**purchase_14781239.json**:
```json
{  
  'user': 'ellen_greg',  
  'action': 'purchase',  
  'item_id': '14781239',  
}
```
We would be able to read that in as a Python dictionary with the following code:
```Python
import json  
  
with open('purchase_14781239.json') as purchase_json:  
  purchase_data = json.load(purchase_json)  
  
print(purchase_data['user'])  
# Prints 'ellen_greg'
```
First we import the `json` package. We opened the file using our trusty `open()` command. Since we’re opening it in read-mode we just need to pass the file name. We save the file in the temporary variable `purchase_json`.
We continue by parsing `purchase_json` using `json.load()`, creating a Python dictionary out of the file. Saving the results into `purchase_data` means we can interact with it. We print out one of the values of the JSON file by keying into the `purchase_data` object.\

### 9. Writing a JSON File 
Let’s say we had a Python dictionary we wanted to save as a JSON file:
```json
turn_to_json = {  
  'eventId': 674189,  
  'dateTime': '2015-02-12T09:23:17.511Z',  
  'chocolate': 'Semi-sweet Dark',  
  'isTomatoAFruit': True  
}
```
We’d be able to create a JSON file with that information by doing the following:
```Python
import json  
  
with open('output.json', 'w') as json_file:  
  json.dump(turn_to_json, json_file)
```


#Python
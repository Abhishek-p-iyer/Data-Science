___

## 1. Simple Bar Chart 
The `plt.bar` function allows you to create simple bar charts to compare multiple categories of data. You call `plt.bar` with two arguments:
- the x-values — a list of x-positions for each bar
- the y-values — a list of heights for each bar
```Python
from matplotlib import pyplot as plt
drinks = ["cappuccino", "latte", "chai", "americano", "mocha", "espresso"]
sales =  [91, 76, 56, 66, 52, 27]
plt.bar(range(len(drinks)), sales)
#create your ax object here
ax= plt.subplot()
ax.set_xticks(range(len(drinks)))
ax.set_xticklabels(drinks)
plt.show()
```

![[Pasted image 20240115073058.png]]

## 2. Side-by-Side bar chart
We can use a bar chart to compare two sets of data with the same types of axis values. To do this, we plot two sets of bars next to each other, so that the values of each category can be compared.
For our side by side bar graph, we want two different bars to be touching each other, we know that the default width of a bar from the `matplotlib` library is 0.8. Hence, we can use this to get the x-values for both the graphs. 
```Python
n = 2  # This is our second dataset (out of 2)  
t = 2 # Number of datasets  
d = 7 # Number of sets of bars  
w = 0.8 # Width of each bar  
x_values2 = [t*element + w*n for element in range(d)]
```

Lets look at an example:
```Python 
from matplotlib import pyplot as plt
drinks = ["cappuccino", "latte", "chai", "americano", "mocha", "espresso"]
sales1 =  [91, 76, 56, 66, 52, 27]
sales2 = [65, 82, 36, 68, 38, 40]
#Paste the x_values code here
n=1
t=2
d=6
w=0.8
store1_x = [t*element + w*n for element in range(d)]
plt.bar(store1_x, sales1)
n=2
t=2
d=6
w=0.8
store2_x = [t*element + w*n for element in range(d)]
plt.bar(store2_x, sales2)
plt.show()
```
![[Pasted image 20240115073930.png]]
## 3. Stacked Bar Charts 
If we want to compare two sets of data while preserving knowledge of the total between them, we can also stack the bars instead of putting them side by side. For instance, if someone was plotting the hours they’ve spent on entertaining themselves with video games and books in the past week, and wanted to also get a feel for total hours spent on entertainment, they could create a stacked bar chart:
We do this by using the keyword `bottom`. The top set of bars will have `bottom` set to the heights of the other set of bars. So the first set of bars is plotted normally:

```Python
from matplotlib import pyplot as plt
drinks = ["cappuccino", "latte", "chai", "americano", "mocha", "espresso"]
sales1 =  [91, 76, 56, 66, 52, 27]
sales2 = [65, 82, 36, 68, 38, 40]
plt.bar(range(len(sales1)), sales1)
plt.bar(range(len(sales1)), sales2, bottom = sales1)
plt.legend(['Location 1','Location 2'],loc=0)
plt.show()
```
![[Pasted image 20240115074109.png]]

## 4. Error Bars
 Sometimes, we need to visually communicate some sort of uncertainty in the heights of those bars. To display error visually in a bar chart, we often use error bars to show where each bar _could_ be, taking errors into account. 
 Each of the black lines is called an _error bar_. The taller the bar is, the more uncertain we are about the height of the blue bar. The horizontal lines at the top and bottom are called _caps_. They make it easier to read the error bars.
 
 If we wanted to show an error of +/- 2, we would add the keyword `yerr=2` to our `plt.bar` command. To make the caps wide and easy to read, we would add the keyword `capsize=10`:
```Python
values = [10, 13, 11, 15, 20]  
yerr = 2  
plt.bar(range(len(values)), values, yerr=yerr, capsize=10)  
plt.show()
```

If we want a different amount of error for each bar, we can make `yerr` equal to a list rather than a single number:
```Python
values = [10, 13, 11, 15, 20]  
yerr = [1, 3, 0.5, 2, 4]  
plt.bar(range(len(values)), values, yerr=yerr, capsize=10)  
plt.show()
```

![[Pasted image 20240115074536.png]]

## 5. Fill Between 
We’ve learned how to display errors on bar charts using error bars. Let’s take a look at how we might do this in an aesthetically pleasing way on line graphs. In Matplotlib, we can use `plt.fill_between()` to shade error. This function takes three arguments:
1. `x-values` — this works just like the x-values of `plt.plot()`
2. lower-bound for y-values — sets the bottom of the shaded area
3. upper-bound for y-values — sets the top of the shaded area
Generally, we use `.fill_between()` to create a shaded error region, and then plot the actual line over it. We can set the `alpha` keyword to a value between 0 and 1 in the `.fill_between()` call for transparency so that we can see the line underneath.
```Python
from matplotlib import pyplot as plt
months = range(12)
month_names = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]
revenue = [16000, 14000, 17500, 19500, 21500, 21500, 22000, 23000, 20000, 19500, 18000, 16500]
y_lower = [i-i*0.1 for i in revenue]
y_upper = [i+i*0.1 for i in revenue]

plt.plot(months,revenue)
ax=plt.subplot()
ax.set_xticks(months)
ax.set_xticklabels(month_names)
plt.fill_between(months, y_lower, y_upper, alpha=0.2)
plt.show()```

![[Pasted image 20240115074858.png]]

## 6. Pie Chart 
In Matplotlib, you can make a pie chart with the command `plt.pie`. When we make pie charts in Matplotlib, we almost always want to set the axes to be equal to fix this issue. To do this, we use `plt.axis('equal')`.
#### Labeling in Pie Chart 
We also want to be able to understand what each slice of the pie represents. To do this, we can either:

1. Using a legend to label each color, or
2. Adding labels on the chart itself.

##### Using a legend 
```Python
budget_data = [500, 1000, 750, 300, 100]  
budget_categories = ['marketing', 'payroll', 'engineering', 'design', 'misc']  
  
plt.pie(budget_data)
plt.axis('equal')
plt.legend(budget_categories)
```

![[Pasted image 20240115075311.png]]

#### Adding labels to the chart itself 
```Python
plt.pie(budget_data, labels=budget_categories)
plt.axis('equal')
```
This puts the category names into labels next to each corresponding slice:
![[Pasted image 20240115075400.png]]

One other useful labeling tool for pie charts is adding the percentage of the total that each slice occupies. Matplotlib can add this automatically with the keyword `autopct`. We pass in string formatting instructions to format the labels how we want:
Some common formats are:
- `'%0.2f'` — 2 decimal places, like `4.08`
- `'%0.2f%%'` — 2 decimal places, but with a percent sign at the end, like `4.08%`. You need two consecutive percent signs because the first one acts as an escape character, so that the second one gets displayed on the chart.
- `'%d%%'` — rounded to the nearest `int` and with a percent sign at the end, like `4%`.

So, a full call to `plt.pie` might look like:
```Python
plt.pie(budget_data,  
        labels=budget_categories,  
        autopct='%0.1f%%')
        plt.axis('equal')
```

![[Pasted image 20240115075557.png]]

## 7. Histogram
Sometimes we want to get a feel for a large dataset with many samples beyond knowing just the basic metrics of mean, median, or standard deviation. To get more of an intuitive sense for a dataset, we can use a histogram to display all the values.
All bins in a histogram are always the same size. The _width_ of each bin is the distance between the minimum and maximum values of each bin. 
Each bin is represented by a different rectangle whose height is the number of elements from the dataset that fall within that bin.

To make a histogram in Matplotlib, we use the command `plt.hist`. `plt.hist` finds the minimum and the maximum values in your dataset and creates 10 equally-spaced bins between those values.
```Python
plt.hist(dataset)  
plt.show()
```

![[Pasted image 20240115080002.png]]

> We could also customize the number of bins our histogram should have by adding a keyword `bins` and equating it to the number of bins we want. We could also customize the minimum and maximum value that our histogram takes into consideration by adding a keyword `range` and adding the minimum and maximum value in round brackets.

### Multiple Histograms 
If we want to compare two different distributions, we can put multiple histograms on the same plot. We can do this in two ways:
1. Using the keyword `alpha` and setting it to any number between 0 and 1. This sets the transparency of the histogram. A value of 0 would make the bars entirely transparent. A value of 1 would make the bars completely opaque.
```Python
plt.hist(a, range=(55, 75), bins=20, alpha=0.5)  
plt.hist(b, range=(55, 75), bins=20, alpha=0.5)
```
![[Pasted image 20240115080453.png]]

2. Using the `histtype` keyword with an argument `step` to draw the outline of the graphs. 
```Python
plt.hist(a, range=(55, 75), bins=20, histtype='step')  
plt.hist(b, range=(55, 75), bins=20, histtype='step')
```
![[Pasted image 20240115080617.png]]

Another important problem we face while dealing with multiple histograms are that they might have different number of samples, making one graph much bigger than the other. To solve this, we can normalize our histograms using `density=True`. This command divides the height of each column by a constant such that the total shaded area of the histogram sums to 1.
```Python
a = normal(loc=64, scale=2, size=10000)  
b = normal(loc=70, scale=2, size=100000)  
  
plt.hist(a, range=(55, 75), bins=20, alpha=0.5, density=True)  
plt.hist(b, range=(55, 75), bins=20, alpha=0.5, density=True)  
plt.show()
```

| Without the `density` argument | With the `density` argument |
| ---- | ---- |
| ![[Pasted image 20240115080931.png]] | ![[Pasted image 20240115080946.png]] |

#Python
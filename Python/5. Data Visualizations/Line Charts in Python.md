___

For a better understanding of the concepts below: [Matplotlib Documentation](https://matplotlib.org/)

## Basic line graph
The line graph is usually used to represent data that changes over time. We can use a line chart to visualize the rate of change of a stock price over a specific time period, or visualize the rate of change of petrol and diesel price over a period of time. 
In python we use the `matplotlib` library to plot a line graph. 
```Python 
from matplotlib import pyplot as plt 
x_values = [0, 1, 2, 3, 4]  
y_values = [0, 1, 4, 9, 16]  
plt.plot(x_values, y_values)
plt.show()
```

The graph will look like: 
![[Pasted image 20240114110315.png]]

We can also add multiple lines in the same axes by using the same `x-value` and changing our `y-value`.  By default, the first line will always be blue and the second line will always be orange. We can change this by adding a parameter called `color` and equate it to your desired color with a hex code. We can also use `linestyle` to change our line to either a dashed line or a dotted line. We can additionally add a marker for the points in our graph, the points can be customized to a square, a star or a circle. 
To see all of the possible options, check out the [Matplotlib documentation](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html#matplotlib.pyplot.plot). Here are a couple of those values applied to our plots about lunch spending of two people. 

```Python 
from matplotlib import pyplot as plt 
# Days of the week:  
days = [0, 1, 2, 3, 4, 5, 6]  
# Your Money:  
money_spent = [10, 12, 12, 10, 14, 22, 24]  
# Your Friend's Money:  
money_spent_2 = [11, 14, 15, 15, 22, 21, 12]

plt.plot(days, money_spent, color='green', linestyle=':')
plt.plot(days, money_spent, color='#AAAAAA', marker='o')
```

![[Pasted image 20240114111144.png]]

## Axis and Labels 
Sometimes we would want to zoom into the plot, for this we use `plt.axis()` and we give 4 values as the input, 
- Minimum X value 
- Maximum X value
- Minimum Y value 
- Maximum Y value 
We can label the x- and y- axes by using `plt.xlabel()` and `plt.ylabel()`. The plot title can be set by using `plt.title()`.
```Python 
x = [0, 1, 2, 3, 4]  
y = [0, 1, 4, 9, 16]  
plt.plot(x, y)  
plt.axis([0, 3, 2, 5])
plt.xlabel("X-axis")
plt.ylabel("Y-axis")
plt.title("Zoomed-in")
plt.show()
```
## Subplot
Sometimes, we want to display two lines side-by-side, rather than in the same set of x- and y-axes. When we have multiple axes in the same picture, we call each set of axes a _subplot_. We can have many different subplots in the same figure, and we can lay them out in many different ways. We can think of our layouts as having rows and columns of subplots. 
We use the `.subplot()` command to create subplots, which needs three input arguments:
- The number of rows of the subplot 
- The number of columns of the subplot 
- The index of the subplot 
```Python 
# Data sets  
x = [1, 2, 3, 4]  
y = [1, 2, 3, 4]  
  
# First Subplot  
plt.subplot(1, 2, 1)  
plt.plot(x, y, color='green')  
plt.title('First Subplot')  
  
# Second Subplot  
plt.subplot(1, 2, 2)  
plt.plot(x, y, color='steelblue')  
plt.title('Second Subplot')  
  
# Display both subplots  
plt.show()
```

![[Pasted image 20240115063019.png]]

### Adjusting subplots
Sometimes, when we’re putting multiple subplots together, some elements can overlap and make the figure unreadable
We can customize the spacing between our subplots to make sure that the figure we create is visible and easy to understand. To do this, we use the `plt.subplots_adjust()` command. The `plt.subplots_adjust()` has some keyword arguments that can move your plots within the figure:
- `left` — the left-side margin, with a default of 0.125. You can increase this number to make room for a y-axis label
- `right` — the right-side margin, with a default of 0.9. You can increase this to make more room for the figure, or decrease it to make room for a legend
- `bottom` — the bottom margin, with a default of 0.1. You can increase this to make room for tick mark labels or an x-axis label
- `top` — the top margin, with a default of 0.9
- `wspace` — the horizontal space between adjacent subplots, with a default of 0.2
- `hspace` — the vertical space between adjacent subplots, with a default of 0.2
For example, if we were adding space to the bottom of a graph by changing the bottom margin to 0.2 (instead of the default of 0.1), we would use the command:

```Python
plt.subplots_adjust(bottom=0.2)
```

## Legends 
When we have multiple lines on a single graph we can label them by using the command `plt.legend()`.
The `legend` method takes a list with the labels to display. So, for example, we can call:
```Python
plt.plot([0, 1, 2, 3, 4], [0, 1, 4, 9, 16])  
plt.plot([0, 1, 2, 3, 4], [0, 1, 8, 27, 64])  
plt.legend(['parabola', 'cubic'])  
plt.show()
```
which would display a legend on our graph, labeling each line:
![[Pasted image 20240115063858.png]]

`plt.legend()` can also take a keyword argument `loc`, which will position the legend on the figure.
These are the position values `loc` accepts:
![[Pasted image 20240115064340.png]]

Sometimes, it’s easier to label each line as we create it. If we want, we can use the keyword `label` inside of `plt.plot()`. If we choose to do this, we don’t pass any labels into `plt.legend()`. For example:
```Python
plt.plot([0, 1, 2, 3, 4], [0, 1, 4, 9, 16],  
         label="parabola")  
plt.plot([0, 1, 2, 3, 4], [0, 1, 8, 27, 64],  
         label="cubic")  
plt.legend() # Still need this command!  
plt.show()
```

## Modify Ticks
In all of our previous exercises, our commands have started with `plt.`. In order to modify tick marks, we’ll have to try something a little bit different.
As our plots can have multiple subplots, we have to specify which one we want to modify. In order to do that, we call `plt.subplot()` in a different way.
```Python
ax = plt.subplot(1, 1, 1)
```
`ax` is an axes object, and it lets us modify the axes belonging to a specific subplot. Even if we only have one subplot, when we want to modify the ticks, we will need to start by calling either `ax = plt.subplot(1, 1, 1)` or `ax = plt.subplot()` in order to get our axes object.
Suppose we wanted to set our x-ticks to be at 1, 2, and 4. We would use the following code:
```Python
ax = plt.subplot()  
plt.plot([0, 1, 2, 3, 4], [0, 1, 4, 9, 16])  
plt.plot([0, 1, 2, 3, 4], [0, 1, 8, 27, 64])  
ax.set_xticks([1, 2, 4])
```
Our result would look like this:
![[Pasted image 20240115065021.png]]

We can also modify the y-ticks by using `ax.set_yticks()`.
When we change the x-ticks, their labels automatically change to match. But, if we want special labels (such as strings), we can use the command `ax.set_xticklabels()` or `ax.set_yticklabels()`. For example, we might want to have a y-axis with ticks at 0.1, 0.6, and 0.8, but label them 10%, 60%, and 80%, respectively. To do this, we use the following commands:
```Python
ax = plt.subplot()  
plt.plot([1, 3, 3.5], [0.1, 0.6, 0.8], 'o')  
ax.set_yticks([0.1, 0.6, 0.8])  
ax.set_yticklabels(['10%', '60%', '80%'])
```
![[Pasted image 20240115065113.png]]

## Figures
When we’re making lots of plots, it’s easy to end up with lines that have been plotted and not displayed. If we’re not careful, these “forgotten” lines will show up in your new plots. In order to be sure that you don’t have any stray lines, you can use the command `plt.close('all')` to clear all existing plots before you plot a new one.

Previously, we learned how to put two sets of axes into the same figure. Sometimes, we would rather have two separate figures. We can use the command `plt.figure()` to create new figures and size them how we want. We can add the keyword `figsize=(width, height)` to set the size of the figure, in inches. We use parentheses (`(` and `)`) to pass in the width and height, which are separated by a comma (`,`).
To create a figure with a width of 4 inches, and height of 10 inches, we would use:

```Python
plt.figure(figsize=(4, 10))
```

Once we’ve created a figure, we might want to save it so that we can use it in a presentation or a website. We can use the command `plt.savefig()` to save out to many different file formats, such as `png`, `svg`, or `pdf`. After plotting, we can call `plt.savefig('name_of_graph.png')`


#Python




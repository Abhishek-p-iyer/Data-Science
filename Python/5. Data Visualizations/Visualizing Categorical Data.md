___
## Introduction to Bar Charts
 Bar charts are generally used to visualize the relative frequencies of categories within a variable. Therefore, they are primarily helpful for visually summarizing categorical variables rather than quantitative variables. If you can organize your variable into distinct categories, a bar chart is often a great option! If not, you may need to resort to a different visual.
 Once you have your distinct categories, a bar chart is best used to display the different value counts of each category.

## Bar Charts Vs Histogram
- Bar charts are used for categorical variables, while histograms are used for quantitative data.
- Histograms must always be arranged in a specific order because they represent a range of numerical values. For a bar chart, each bar represents frequencies of category variables within a category. Unless the variable is ordinal, the bars could be arranged in any order.

## Plotting Bar charts using Seaborn

```Python
import pandas as pd
import seaborn as sns 
from matplotlib import pyplot as plt 

df = pd.read_csv('<csv file>')
sns.countplot(df.Results)
plt.show()
```

![[Pasted image 20240115111124.png]]

## Bar Chart Ordering 
### Nominal Data 
Nominal Data does not necessarily have to be represented in an orderly fashion, hence we have a lot of creative freedom when choosing where each bar goes on our chart.
```Python
sns.countplot(df["victory_status"], order=df["victory_status"].value_counts(ascending=True).index)
```

![[Pasted image 20240115114033.png]]
If we want the bars in reverse order, we can take `ascending=True` out of the `.value_counts()` method (descending order is the default). The `index` call specifies the row labels of the DataFrame.
### Ordinal Data 
If we are working with ordinal data, we should plot the data according to our categorical variables. For example, let’s say we want to plot the number of students per grade level at a college. We can order the categorical values as `First Year`, `Second Year`, `Third Year`, and `Fourth Year` since they are ordinal. Using `.countplot()`, we can input these as a list in the `order` parameter.

```Python
sns.countplot(df["Grade Level"], order=["First Year", "Second Year", "Third Year", "Fourth Year"])
```
![[Pasted image 20240115114240.png]]

## Pie Charts

 While bar charts are usually used for visualizing a table of frequencies, pie charts are an alternative when you want to visualize a table of proportions. Pie charts are made up of slices that are combined to make a full circle. Each slice represents a proportion, and the sum of each proportion (slice) adds up to 1 (or 100%).
These slices are meant to give viewers a relative size of the data, similar to how bars in a bar chart act. The arc length (size of the slice’s angle) of each slice is proportional to the quantity it represents. This also means that the area of each slice is proportional to this quantity as well.

> pie charts are the most issue-ridden graphing visual. This is due to the fact that the human eye would find it harder to decipher the area between two similar proportions. 

##### When do we use a Pie Chart?
The main reason one would use a pie chart is to represent a part-to-whole relationship in your data. If you want to compare relative sizes of your data and see how they all are part of a larger sum while worrying less about their precise sizes, a pie chart may be a good option.

## Plotting a Pie Chart
We use the `matplotlib` library to plot a pie chart as the seaborn library does not have any pie chart libraries. 
```Python
from matplotlib import pyplot as plt 
student_grades = pd.read_csv("student_grades.csv")
wedge_sizes = student_grades["Proportions"]  
grade_labels = student_grades["Grades"]
plt.pie(wedge_sizes, labels = grade_labels)
plt.axis('equal')
plt.title(“Student Grade Distribution”)  
plt.show()
```
![[Pasted image 20240115114900.png]]

## Making your Pie Charts better
With too many slices, trying to decipher the visual becomes cumbersome — sometimes even more cumbersome than reading off data from a table. When we have a pie chart with 20 wedges, it becomes harder for the reader to get some meaningful information from the graph. 
When you find yourself in a situation like this, you have a couple of options.
1. You can aggregate your pie slices to create fewer while still portraying an informative story.
2. You can see if a bar chart does a more effective job at portraying the information. Bar charts do not suffer nearly as much as pie charts when it comes to visualizing numerous categories. This is because they are not constrained to one circle — instead, numerous bars.
#Python
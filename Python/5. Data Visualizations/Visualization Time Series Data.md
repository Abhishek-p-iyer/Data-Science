____
Data represented in a single point in time is known as _cross-sectional data_. Time series data shows up in the real world quite often. For example, weather readings, company stock prices, and sales data are all examples of data that can be tracked over time. Therefore, it’s important that you are able to explore and visualize data with a time component.


## Line Plot
A _line plot_ is commonly used for visualizing time series data.

In a line plot, time is usually on the x-axis and the observation values are on the y-axis.

```Python 
# import libraries  
import pandas as pd  
import matplotlib.pyplot as plt  
import seaborn as sns  
  
# load in data  
sales_data = pd.read_csv("sales_data.csv")  
  
# peek at first few rows of data  
sales_data.head()
# convert string to datetime64  
sales_data["date"] = sales_data["date"].apply(pd.to_datetime)  
sales_data.set_index("date", inplace=True)  
  
# create line plot of sales data  
plt.plot(sales_data["date"], sales_data["sales"])  
plt.xlabel("Date")  
plt.ylabel("Sales (USD)")  
plt.show()
```

![[Pasted image 20240115131653.png]]

Notice how we can see the trend of the data over time. Looking at the chart, it seems that:

- Sales are seasonal, peaking at the beginning and end of each year, and slowing down in the middle of each year.
- Sales don’t seem to show signs of growth over time. This appears to be a stagnant business.


## Box plot
When working with time series data, box plots can be useful to see the distribution of values grouped by time interval.

For example, let’s create a box plot for each year of sales and put them side-to-side for comparison:
```Python
# extract year from date column  
sales_data["year"] = sales_data["date"].dt.year  
  
# box plot grouped by year  
sns.boxplot(data=sales_data, x="year", y="sales")  
plt.show()
```

![[Pasted image 20240115131737.png]]
For each year of the sales data, we can easily see useful information such as median sales, the highest and lowest sales, the interquartile range of our data, and any outliers.

Median sales for each year (represented by the horizontal line in each box) are quite stable, suggesting that sales are not growing over time.

## Heatmap
We can also use a heatmap to compare observations between time intervals in time series data.

For example, let’s create a density heatmap with year on the y-axis and month on the x-axis. This can be done by invoking the `heatmap()` function of the `sns` Seaborn object:
```Python
# calculate total sales for each month  
sales = sales_data.groupby(["year", "month"]).sum()  
  
# re-format the data for the heat-map  
sales_month_year = sales.reset_index().pivot(index="year", columns="month", values="sales")  
  
# create heatmap  
sns.heatmap(sales_month_year, cbar_kws={"label": "Total Sales"})  
plt.title("Sales Over Time")  
plt.xlabel("Month")  
plt.ylabel("Year")  
plt.show()
```

![[Pasted image 20240115131828.png]]

## Lag Scatter Plot
We can use a _lag scatter plot_ to explore the relationship between an observation and a lag of that observation.

In a time series, a _lag_ is a previous observation:

- The observation at a previous time step (the smallest time interval for which we have distinct measurements) is called lag 1.
- The observation at two times ago is called lag 2, etc.

In the sales dataset, we have a different sales value for each day. Therefore, the lag 1 value for any particular day is equal to the sales on the previous day. The lag 2 value is the sales two days ago, etc.
The plotting module of pandas library has a built-in `lag_plot` function that plots the observation at time t on the x-axis and the lag 1 observation (t+1) on the y-axis:
```Python
# import lag_plot function  
from pandas.plotting import lag_plot  
  
# lag scatter plot  
lag_plot(sales_data)  
plt.show()
```
![[Pasted image 20240115132007.png]]
How can we interpret a lag scatter plot?

- If the points move from the bottom left to the top right, this indicates a positive correlation between observations and their lag 1 values. For example, high sales on one day are associated with high sales on the previous day.
- If the points move from the top left to the bottom right, this indicates a negative correlation between observations and their lag 1 values. For example, high sales on one day are associated with low sales on the previous day and vice versa.
- If there is no identifiable structure in the lag plot, this indicates the data is random, and there is no association between values at consecutive time points. For example, sales on one day tell you no information about expected sales on the following day.

Exploring the relationship between an observation and a lag of that observation is useful for helping us determine whether a dataset is random.

Since the points in the sales data move along a diagonal line from the bottom left to the top right, this indicates that our data is not random and there is a positive correlation between observations and their lag 1 values.


## Autocorrelation plot
An _autocorrelation plot_ is used to show whether the elements of a time series are positively correlated, negatively correlated, or independent of each other.
This can be plotted with the `autocorrelation_plot()` function of the `pandas.plotting` module:
```Python
# import autocorrelation function  
from pandas.plotting import autocorrelation_plot  
  
# autocorrelation plot  
autocorrelation_plot(sales_data)  
plt.show()
```

![[Pasted image 20240115132148.png]]

In the autocorrelation plot above, lag is on the x-axis and the value of the autocorrelation, which ranges from -1 to 1, is on the y-axis. A value near 0 indicates a weak correlation while values closer to -1 and 1 indicate a strong correlation.

Notice how the autocorrelation plot for the sales data forms waves, oscillating between strong negative and positive correlation. These waves suggest that our dataset exhibits seasonality.

Also, notice how the autocorrelation decreases over time. This indicates that sales tend to be similar on consecutive days, but sales from three years ago are less associated with today’s sales than sales from one year ago.

#Python
___
When we have one categorical variable and one quantitative variable, we can compare the association between these two variables statistically by comparing there mean, median and their mean and median difference. We will also need to visually represent these variables using a boxplot and a histogram for aiding us in understanding the distribution better. 

Let's take an example of exploring the relationship between two variables of two schools. We are exploring the relationship between the student scores in these two schools are we are trying to find out if one is better than the other. 
We will first start by comparing the mean and median differences of the student's scores in these two schools. 
```Python 
# Mean and Median difference 

scores_GP = students.G3[students.school = 'GP']
scores_MS = students.G3[students.school = "MS"]

mean_GP = np.mean(scores_GP)
mean_MS = np.mean(scores_MS)

median_GP = np.median(scores_GP)
median_MS = np.median(scores_MS)

mean_diff = mean_GP - mean_MS
median_diff = median_GP - median_MS

print(mean_diff) # Output: 0.64
print(median_diff) # Output: 1.0
```

Now the difference between these values are not very large, but large can also be very subjective based on a particular scenario, hence we visualize and compare the data with the help of boxplots and histograms. 

### Side-by-Side boxplot 
We look at the spread of the data in these boxplots, if there is more than 50% overlap in the data, we can say with a certain degree of confidence that the both these values are similar. 
```Python 
import seaborn as sns 

sns.boxplot(x='school', y='G3', data='students')
plt.show()
```

![[Pasted image 20240105131843.png]]
We can see that the spread of these graphs look similar, there is more than 50% overlap between the two graphs. 
### Overlapping Histogram
Another way to look at the spread of the data is by plotting overlapping histograms. 
```Python 
import matplotlib.pyplot as plt 

plt.hist(x=scores_GP, color= "blue", label = 'GP', normed = True, alpha=0.5)
plt.hist(x=scores_MS, color="red", label='GP', normed=True, alpha=0..5)
plt.legend()
plt.show()
```

![[Pasted image 20240105132452.png]]

While overlapping histograms and side by side boxplots can convey similar information, histograms give us more detail and can be useful in spotting patterns that were not visible in a box plot




#Python
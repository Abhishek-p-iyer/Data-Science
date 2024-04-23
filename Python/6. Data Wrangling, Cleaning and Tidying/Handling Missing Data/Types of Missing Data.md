___
While every variable in a dataset can provide some information that we need, not all variables are created equal. Depending on what we are trying to learn, some fields have more importance than others

There are four types of missing data that we would find in our dataset:
- Structurally Missing Data: There is a logical reason why the data is missing
- Missing Completely at Random: The probability of the data to be missing is equal for all data points. 
- Missing at Random: The probability that the data would be missing is different for different groups but is equal within the same group.
- Missing not at random: There is an inherent reason why the data is missing which is not related to the survey.

## Structurally Missing Data 
There is a logical reason why the data is missing. For ex: When we're surveying the frequency with which the user uses the asthma inhaler is conducted, there is going to be a population that does not have asthma, hence not needing an inhaler. This datapoint will be left blank and these datapoints are called structurally missing data. 


## Missing Completely at Random (MCAR)
Sometimes data can just be missing. It can happen for any reason, but the important thing to note is that it could have happened to any observation. For ex: Lets take an example of a pedometer sensor that has a bug issue which causes 20% of the sample to not track their steps. Now these datapoints are missing at completely random as the bug can affect any individual, hence any datapoint can be missing.

## Missing at Random (MAR)
The probability that a datapoint would be missing is different for different groups but equal within the group. Lets take an example of a activity survey which asks people to enter their height and weight. Now the probability that people would not enter their weight or height would differ from different people. Usually people who have a BMI of above normal would refrain from entering in their details. The probability that an individual from different categories of the BMI index not entering the data would differ from different categories but it would be similar within each category. 


## Missing Not At Random (MNAR)
The reason for this data to be missing is not random but the data is missing for some inherent reason. For ex: Lets take an example of a survey of weights across different age groups, now if the weight scale ran out of batteries, it would cause the datapoints for certain individuals to be missing. There are reasons why the data is missing and they're just not missing due to random events.

# Salary-Determination-Using-Data-Mining-in-Python
Programming language and package
I used python to complete this project and Tableau for some deeper visualization. The main packages that I have used were imported in the beginning of the source code (pandas, Numpy, matplotlib,etc). 

The whole python program was running on Jupyter notebook. Before I read the adult.data and adult.test files, I changed them to csv files, and used excel to replace all the “?” with “NaN”. I used pandas to read the files which converted them to pandas data frames.

Take a Big View: 
Data Types:
I would like to know what which columns contain continuous data type and which ones contain categorical data type.

Brief Data Description for Continuous Data:
I would like to know some basic statistics of this dataset, including minimum, maximum, and mean, standard deviation and so on. I can obtain some general sense of which column has a more dispersed data. For example, the capital-gain and capital-loss both have very large standard deviation, which means their data is very dispersed.

Histogram for Continuous Data:
I also used histogram to visualize each continuous data column. Looks like none of them are bell-shaped but skewed. For example, age is right skewed, which means many data came from younger people; education-num is left skewed, which means majority of people obtained a moderate to higher education. 

Heat Map:
I used heat map to see if there is any significant correlation among each column, but these correlations are not strong.
 
Gaussian Naïve Bayes Classifier
I defined two main Naïve Bayes Classifier functions:
1.	For combination of continuous data and categorical data:
For continuous data, I assumed that they follow the normal distribution. I created two functions that can calculate the probability density function by given mean and variance for labels “>50K” or “<=50K”. 
For categorical data, since I could not fit them into the above normal distribution functions, I created another function that could calculate Bayesian probabilities for categorical data given salary labels. It was called p_var_given_salary().
Next, I defined a function called GaussianNB(), which would call the above three functions and identify if the given data should be assigned to “>50K” or “<=50K”.

2.	For categorical data only.
This was applied to discretized continuous data.
In this case, I defined another Naïve Bayes classifier function called GaussianNB_disc() that only would call p_var_given_salary().

Data Discretization with Equal Bins Method
For equal bins method, I used Tableau to visualize the histogram of the continuous data since it provided me with bigger and better quality of graphs. I tried different bins for each variable and each bin label was considered as a category group. For example, if I had 20 bins, then I would get age1 group through age20 group. I noticed that smaller bins gave me more details. However, if these bins were too small, some categories might contain zero data. Therefore, I chose a moderate bin width for each variable.

Then I created a function cont_to_cat() that would help me sort different data into their corresponding categorical groups.

10-Fold Cross Validation
I created a set of randomized indexes to make sure the train and test datasets were randomly picked. Next, I split the randomized data into 10 folds. Each time, the program would take nine of them to train and one of them to test, and the output would be two lists of salary labels. 
Next I used the function that I defined, get_score(), to get accuracy, recall, precision and f1 scores by comparing my two lists of salary labels. 
Finally, I created a dataframe that show accuracy, recall, precision and f1 score for each fold and the overall performance of the model. 

Train and Test Data:
Since we are supposed to use 2 methods for missing values and 2 methods for continuous data, I used 4 combinations to fulfill this requirement.

Combination 1: Drop Missing Values and Use Gaussian Naïve Bayes Classifier
The original data contained missing values: 1836 in workclass, 1843 in occupation and 583 in native_country, which were total of 4262. In first combination, I chose to drop all of them. Then I used the none-null dataset and GaussianNB() and 10 folds cross validation to evaluate my first combination. 
 
The overall accuracy for combination 1 was 82.61%, which was a good performance.

Combination 2: Drop Missing Values and Use Discretized Continuous Data
In this combination, I also used the non-null dataset, but instead of using Gaussian distribution to fit my continuous data, I discretized them and used GaussianNB_disc() instead of GaussianNB(). 
 
The overall accuracy for combination 2 was 75.8%, which was so good.

Combination 3: Assign Missing Values to New Values and Use Gaussian Naïve Bayes Classifier
Since all the missing values were categorical, the most common data in their columns might be the central tendency of the attribute, so I assigned 4262 missing values according to this method, which were Private for workclass, “Other-service” for occupation, and “United-States” for native_country. I then created a new dataset that included these changes.
I fitted this new dataset with Gaussian Native Bayes Classifier and got result of accuracy of 82.61% for my combination 3.
The accuracy of combination 1 and combination 3 were the same, which meant that if missing values were dropped, the discretization would not matter too much in this case. 

 

Combination 4: Assign Missing Values to New Values and Use Discretized Continuous Data
In this combination, I used the dataset with replaced missing values and GaussianNB_disc() to evaluate the result. 
 
It turned out that combination 4 has the best scores overall(featured with an overall accuracy rate of 82.629%,  therefore, I used combination 4 for my test dataset. 

Testing the New Dataset 
In this step, I firstly imported and preprocessed the adult.test dataset as I did for combination ¬ assigned missing values with new values and discretized continuous data.
Then I removed k-fold method and used dataset that I used for combination 4(adult_WthNull_Disc) as my train data, and the adult.test data as my test data. 
I also added a step that would evaluate adult_WthNull_Disc as both my train and test data for comparison.
The result showed that I obtained 83% accuracy rate from this model for adult.test dataset, which was a good performance. 
Using adult_WthNull_Disc as both train and test data a sightly higher rate, which was what I have expected.

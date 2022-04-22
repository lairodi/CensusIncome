# Introduction #  
  [The Adult Census Income dataset](https://archive-beta.ics.uci.edu/ml/datasets/adult) data was extracted from the 1994 Census bureau database by Ronny Kohavi and Barry Becker (Data Mining and Visualization, Silicon Graphics). The main prediction task with this data set is to determine whether a person makes over $50K a year. The data has 48842 rows, mix of continuous and discrete and is split into training (32561 rows) and test (16281 rows).   
  
  In this assignment, we explore this dataset and build classification decision tree models (random forest and boosting) to predict whether a person makes over \$50,000 or not. Both the models will be tuned to improve model performance and then their performance will be compared against a baseline logistic regression model. Finally, we will analyze the pros and cons of each model and recommend a model that can be used with this dataset.
    
# Exploratory Data Analysis #  
  
## Dataset description ##

The dataset has 14 columns that are a mixture of categorical, ordinal, and numerical data types. The dataset contains missing values that are marked with a question mark character (?). There are a total of 48,842 rows of data, and 3,620 with missing values, leaving 45,222 complete rows.  
*Refer to Appendix for full description of all the variables.*  
  
## General observations ##  
  
- **Dependent variable - income** - a binary variable with two classes (salary >50K and salary <=50K). The classes are imbalanced, with a skew toward the rows with salary <=50K (Salary >50K - minority class, approximately 25%, Salary <=50K - majority class, approximately 75%).
- **educationnum** and **education** - Majority of the people have 8 to 16 years of education and most of them have completed high school and some college. We will need to check if both of these variables are needed in the model as they appear to be related to each other. 
- **hoursperweek** - The mean number of working hours per week is 41, and at least 50% of the people taking part in the survey, work between 40 and 45 hours per week. The data also has a lot of outliers which might affect algorithm performance, so we will group the working hours in 5 categories - less than 40 hours per week, between 40 and 45 hours per week, between 45 and 60 hours per week, between 60 and 80 hours per week, and more than 80 hours per week)
- **nativecountry** - has 41 levels and a predictive model with this variable will have 41 additional degrees of freedom. This will complicate the analysis and might lead to over-fitting. Hence, we will group the native countries into several global regions - Asia_East, Asia_Central, Central_America, South_America, Europe_West, Europe_East.
- **capital_gain** and **capital_loss** - There are too many zeros in the variables “capital_gain” and “capital_loss” and this can seriously disrupt the analysis. We will convert these two variables into categorical variables and group them as High, Medium and Low. 

## Relationship between variables ##   
  
- There is very little correlation (-0.008957423) between income and the variable fnlwgt, so this variable will be omitted.
- The correlation between income and the variable age is 0.2419981, and between income and number of years of education is 0.3352862. Even though the correlation is low, We will need to do additional analysis to see if these can be excluded.
- There seems to be a relationship between the categorical variables and the response. However, of these variables, native_region seems to have a weak association with income while also being heavily skewed.  Other weak relationships seem to arise from race and sex. 
- Education and years of education (education.num) reveal the same information, so educationnum will be omitted.

# Model Fitting #  
  
## Logistic regression ##  
The first model will be a logistic regression model. The R **step** function will be used to select the best logistic regression model. The AIC for the model chosen is 19776.97, and the variable educationnum was excluded. The training error for this model was 15.1% (0.1510841) and the test error was 15.24% (0.1524568). A probability threshold of 0.5 is used to classify the model predictions (this was the value that yielded the highest prediction accuracy)

## Random Forest ##   
The next model is a random forest model. The randomForest function from R package randomForest is used to create the model. From the error plot, it appears that the errors do not decrease after the number of trees reaches a certain threshold, which is around 100 trees. Hence, a RF model with 200 trees was built and this achieved a similar accuracy with a computationally less expensive model. The overall “out-of-bag” error, or OOB, for the first model is 14.76% and for the model with fewer (200) trees it is 14.9%. The prediction error for the training data is 9.2% (0.09200318) and for the test data it is 15.12% (0.1512616).  
  
Checking the variable importance table, we see that race, sex and native_region are the least important variables. So, we created a model without those variables and calculated the prediction accuracy. The training prediction error was 9.1% (0.09110802) and the test prediction error was 15.46% (0.1546481). Since the test prediction error increased, we will not exclude these variables from the model.

## Boosting ##  
The last model created is a gradient boost model. Using a probability threshold of 0.13 to classify the predictions (if probability > 0.1379 then classify that row as >50K, else <=50K), the training prediction error is 13.92% (0.1392481) and the test prediction error is 14.56% (0.1456839). This model has the lowest prediction accuracy compared to the other two models. 
  
Here also we see that sex, race and native_region are the least important variables. So, we created a model without those variables and calculated the prediction accuracy. The training prediction error was 14.12% (0.1412042) and the test prediction error was 15.56% (0.1456839). Again, since the test prediction error increased, we will not exclude these variables from the model. 

# Conclusion #
  
An interesting outcome of these models is that, despite issues such as gender wage gap, the models generated showed that gender and race were not very significant. However, it is important to note that the model is predicting whether someone earns more than \$50,000 or not and this doesn't extend to actual income comparisons between those of different sex and race. However, as expected, random forest and boosting performed the best. The highest training accuracy of resulted from the random forest model excluding race, sex and native_region, however, gradient boost performed the best with the test data. 


  

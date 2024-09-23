# Regression Analysis for Predicting Success on College Exams

![students-taking-exam-classroom-education-260nw-1289569291](https://github.com/user-attachments/assets/4592f182-6d88-4ec1-9ed1-3b7297ce6c2a)


## Business Understanding
The transition from high school to college can be daunting, marked by increased academic rigor, newfound independence, and the pressure to succeed. The academic workload often surpasses that of high school, requiring efficient time management and effective study habits. Moreover, the social environment is drastically different, with a larger and more diverse student body. This can lead to feelings of isolation or anxiety for some freshmen. Understanding the key variables that contribute to high exam scores can significantly alleviate stress and improve academic performance. By identifying effective study techniques, time management strategies, and seeking support from professors and peers, freshmen can better navigate the challenges of college and achieve their academic goals.

I was a high school educator for eight years and I want to use my data science understanding to help high school students with this transition. High school seniors, parents of high school seniors, high school educators, and colleges/universities are directly impacted by this topic. Understanding the variables that go into successful college exam scores will help alleviate stress on the students. This will give parents more confidence that their student will be successful in college while also reducing the risk of losing money they are investing in their students' education. High school educators will be able to use this knowledge to be better at preparing their students for college and universities will see a financial benefit as the number of dropouts will be reduced.

## Data Understanding

The data I’m using for this analysis comes from the data science community website, Kaggle. There are 20 features and 6,607 records that are described clearly. The features include: hours studied, attendance, parental involvement, participation in extracurricular activities, sleep, previous scores, motivation, internet access, tutoring, family income, teacher quality, school type, peer influence, physical activity, learning disabilities, parental education level, distance from home, and gender. The data is stored in a csv file with integer and object variables. Descriptive statistics and value counts are available for all of the numerical and categorical data in the set.

These data are suitable for this project because the features are all relavent towards things that a college student has to balance when they go to school. There are also some features that it would be interesting to know if they are significant or not towards a student doing well on an exam. There are some features that I would think would be interesting to add in the future. For example, there are features on physical activity but I'd like to see if diet affects exam scores as well. The 'freshman 15' is a well-known phenomenon where a freshman gains fifteen pounds his/her first year at college and I wonder if that impacts exam scores as well.

To understand the data I looked at the data types of all of the feautures and checked to see if there were any missing values. There were some missing values but it was a small percentage of the total data so I dealt with this by dropping rows with missing values. Next, I looked at the descriptive statistics of the numerical features (hours studied, attendance, sleep hours, previous scores, tutoring sessions, physical activity, and exam score) and the value counts of the categorical data (parent involvement, access to resources, extracurriculars, motivation, internet access, family income, teacher quality, school type, peer influence, learning disabilities, parent education, distance from home, and gender). For this analysis I'm using exam score as my target variable and I'm using the other features as predictors of the exam score.

The next thing I did was look at the relationship amongst the predictor variables and the relationship between the predictor variables and the target variable. Attendance (.58) and Hours studied (.45) were moderately coorelated to exam score and all other features were not correlated. I used scatter plots to check the coorelation and the linearity of the data. I used a correlation matrix to check the correlation between the numerical data and found no multicollinearity. I found a way to check the correlation of the categorical variables by using the Cramers V statstic method and made a correlation matrix with that as well and found no multicollinarity.

![Screenshot 2024-09-23 135611](https://github.com/user-attachments/assets/30709647-3669-4a60-85cb-89c01b45f4db)

## Data Preparation

To prepare the data for this analysis I assigned the predictor features to the X variable and the target score to the y variable. Then I split the data into a training set (80%), a test set (10%), and a validation set (10%) to avoid data leakage. I used the training data to train and pick the best performing model. I used the validation set to evaluate the best model and tune the hyperparamters. I used the test set as an unseen data set to provide an unbiased final evaluation of the best model with the best hyperparameters.

To prepare the data for analysis I also created dummy variables for each of the categorical features. I did this so that each of the categorical features will be represented by a 0 or a 1 for the model to analyze. I dropped the first column from each category in order to avoid multicollinearity. I then needed to change the datatype to float so the modeling will work.

I dropped insignificant features from the data to see if that improved the modeling metrics. I also scaled the data to better understand how changes in the coefficients would impact the target variable.

## Modeling

I chose to do a linear regression analysis because I am using variables to predict a continuous target varaibel. I chose mean absolute error (MAE) as the target metric. Mean absolute error is expressed at the same scale as the target variable so it is easy to interpret. All errors are treated equally, so the metric is robus to outliers. Also, it disregards the direction of erros, so underforecasting is the same as overforecasting. I chose the metric that is robost to outliers because the data does contain outliers in the numberical categories. I also wanted to put an emphasis on underforecasting because I'd rather the model predict a student get a grade and then the student get a higher grade than was expected than have the model predict a grade and the student get a lower grade than expected. I also used r-square score as a comparison metric to show the explanation of variance between the models and improvement from hyperparameter tuning.

I chose a single linear regression model using the highest correlated feature to the target variable as my baseline model. The model and variable were statistically significant with a R-squared score of .331 and a MAE score of 2.073.

I chose five other linear regression models to compare to my baseline model. These were a multiple linear regression model using all of the features, random forest regressor, k-nearest neighbors regressor, ransac regressor, and support vector machine regression. The highest performing model of these was the ransac regressor. This model had a MAE score of .421 and an R-squared score of .733. This was a 1.652 improvement on MAE score and a .402 improvement on R-squared score. This means the ransac model can explain 40% more of the variance than the baseline model and has 1.652 less error on a 100 point scale than the baseline model. This is important because that is 2 points on an exam score which means I'm reducing the error by potentially a whole exam question depending on how many questions there are on the exam.

Next, I looked for ways to improve the ransac regressor model using the validation data. First, I tried dropping the features that weren't significant to the model. These features were: sleep hours, school type (public vs. private), and gender. This very slightly improved the MAE (.0003) and R-squared (.0002). However, there was improvement so I left these features out as I continued modeling.

Next, I scaled the data using a standard scalar. This didn't impact the metrics as this process is only done to make the coefficients more interpretable by keeping them on the same scale.

Finally, I ran a gridsearch on the most important hyperparamters of a ransac regression model and applied them to the model. This made the model's MAE worse by .03. Therefore, I made the final model the ransac model with default hyperparamaters.

![Screenshot 2024-09-23 135933](https://github.com/user-attachments/assets/d92ce87a-bd4d-445d-a104-d922c1db9227)


## Evaluation 

I chose mean absolute error (MAE) as the target metric. Mean absolute error is expressed at the same scale as the target variable so it is easy to interpret. All errors are treated equally, so the metric is robus to outliers. Also, it disregards the direction of erros, so underforecasting is the same as overforecasting. I chose the metric that is robost to outliers because the data does contain outliers in the numberical categories. I also wanted to put an emphasis on underforecasting because I'd rather the model predict a student get a grade and then the student get a higher grade than was expected than have the model predict a grade and the student get a lower grade than expected. I also used r-square score as a comparison metric to show the explanation of variance between the models and improvement from hyperparameter tuning.

The final model I chose was the ransac regressor model using only the scaled significant features from the baseline model. This model had the lowest MAE and highest R-squared score of the models I evaluated.

When I evaluated the performance of the model using the holdout test data the final metrics were MAE - .448 and R-squared - .729. I also plotted the residuals and found that there was heteroskedasticity and normality of the residuals.

I plotted the predicted results from the model with the actual test scores from the holdout data and the implications were good for the real-world problem. The predicted exam scores were closely clustered along a line showing that the predictions were within a 1 or 2 percentage points of the actual test scores. In the five exceptions where the actual test score wasn't close to the predicted score the actual test scores were higher than the predicted score which was my goal when choosing an evaluation metric.

![predicted vs actual exam scores](https://github.com/user-attachments/assets/3826acb6-fa19-4099-b6de-3b414ffe8dc2)

## Conclusions

*   **Conclusion 1:**  College exam scores are predictable. 
*   **Conclusion 2:**  Sleep, Public vs. Private College, and Gender are insignificant in predicting exam scores.
*   **Conclusion 3:** The follwing table shows which features most positively 

<img width="236" alt="coefficient" src="https://github.com/user-attachments/assets/785af1c6-91ee-4798-97e3-759325e4e3d4">

## Next Steps

*   **New Features:** The dataset has a wide range of features, but if it includes features such as physical activity and sleep I'd like to have invormation on a feature about diet to see if that's significant to exam scores.  Does the "freshman 15" impact exam scores? 

*   **New Populations:** This data analyzes college exams but could these features also apply to high school exams as well? 

*  **Application:**  What is the best way to get this information to the target stakeholders? 

* **Explore:**  Why is there a dropoff in exam scores after more than 6 tutoring sessions per month? 

## For More Information

See the full analysis in the Jupyter Notebook or review this presentation. 

For additional info, contact Josh Grimes at josh.g.grimes@gmail.com

## Project Structure

* [.gitignore](https://github.com/josh-g-grimes/Regression-Analysis---College-Exam-Performance/blob/main/.gitignore)
* README.md
* [data](https://github.com/josh-g-grimes/Regression-Analysis---College-Exam-Performance/blob/main/StudentPerformanceFactors.csv)
* [notebook.ipynb](https://github.com/josh-g-grimes/Regression-Analysis---College-Exam-Performance/blob/main/notebook.ipynb)
* presentation

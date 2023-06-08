# Prediction Model: How Good are these Recipes?
by Tauhid Noor (tnoor@ucsd.edu)

## Framing the Problem
In this report, I will create a prediction model that tries to predict the rating - response variable - of recipes based on other given information in the dataset. I will be building a classification model to predict the ratings since the ratings are discrete values (1-5). I chose ratings as my response variable over other metrics because it's reasonable to see ratings as the dependent variable, and features such as calories, no. of ingredients, macronutrients, etc as independent variables that determine the quality/enjoyability of a recipe. Moreover, since I'm building a classification model that deals with discrete values, ratings is the only usable metric that suits the description of a discrete metric. The other variables would be more appropriate for a regression model since they are continuous variables. 

For my classification model, with `rating` column as my response variable, I will be using `calories`,`n_ingredients`, `minutes`, `total fats`, `carbohydrates`, `protein`, `sugar` columns as my features (tentative). The transformations of the features will be decided on as we look to see what improves the model's performance level. I chose these metrics because I think these metrics would assess the healthiness, taste, convenience, and nourishment of the recipe which would determine if the user enjoyed the food and rated it high. 

I will be evaluating the model by using the mean accuracy, that is, finding the proportion of labels that the model got right. For a classfication model, I think determining the proportion of correctly predicted values is a more appropriate method than the common RMSE and R^2 methods because the latter two are suited to regression models.

## Baseline Model
For the initial model, I will use Decision Tree Classification Model since it predicts discrete values like the `rating` column. I will choose `sugar`, `calories`, and `n_ingredients` as the features. All 3 columns are quantitative. I will leave the `sugar` column as itself. I transformed the `calories` column into **natural log** values so that the unusually large values and outliers don't distort the results of the prediction model. I transformed the `n_ingredients` to a **binary** format (1 and 0) to indicate high number of ingredients **(1)** and low number of ingredients **(0)**.

The performance of our baseline model can be considered sufficient. The accuracy of our model is approximately 77.405% which leaves room for improvement, but since it's our baseline model, to be able to accurately label more often than not in our initial model, it can be considered promising.

## Final Model
In our final model, I will engineer two new additional features with several columns: `total fat`, `protein`, and `carbohydrate` columns for one feature, and `minutes` and `n_steps` for the other. I will also test out the best hyperparameters: I will choose the best performing classification model between Decision Tree Classifier and Random Forest Classifier. I will decide by comparing the training accuracy and testing accuracy of the two different models. The error values will be the mean accuracy of our predictions for each max depth for the respective model. After picking out the highest average of the training and test error from each model, I will then cross-compare between the two models to see which is the highest between the two. 

It's important to pick the best sub-hyperparameter (max_depth) to avoid overfitting or underfitting, and then the best hyperparameter (decision tree/random forest) to get the best overall performance. 

##### Decision Tree Result:
After going through an iterative process to find the mean accuracies of the different max depths of our Decision Tree, we found the best max_depth to be 13, and the average of the training and testing accuracies to be approximately 77.428%. We will now do the same with the Random Forest Classifier algorithm and compare the two different models. 

##### Random Forest Result:
After going through an iterative process to find the mean accuracies of the different max depths of our Random Forest, we found the best max depth to be 15, and the average of the training and testing accuracies to be approximately 77.482%. 

##### Comparison:
If we compare the two models, the Random Forest model has marginally better accuracy across the the training and testing accuracies than that of the Decision Tree model. The Random Forest model is "better" by 0.54%. While the difference may be small, objectively the Random Forest model should be the final model for its performance. 

The final model is an improvement over the baseline model. Its mean accuracy is better by 0.77%. The improvement is marginal but after adding two new reasonable features, and finding the best hyperparameters through an iterative process, it is the best we can do.

The improvement that we observed can be attributed to the new features. The first new feature will add all the values of the macronutrients (fat, protein, and carbohydrates) of a recipe because it is reasonable to assume that the higher your total PDV of fat, protein and carbohydrate, the more nourishing the recipe is, which should cause users to rate the recipe higher. The second feature divides the number of minutes to make the recipe by the number of steps. This transformation tells us the average time spent on one step of the recipe, which shows the amount of detail and care is given to prepare the recipe. The more care the recipe is prepared with, the better it will come out. Hence, we should expect users to rate it higher. 

## Fairness Analysis
After coming up with the final model, it's important to check if the model is biased in some way. I will check to see if there is a substantial difference in performance level of the model when feeding it data separated into two groups. I will use a **permutation test** to find the significance of our findings.

**Group 1:** The dataset with high number of ingredients (greater than 8)

**Group 2:** The dataset with low number of ingredients (less than or equal to 8)

**Null hypothesis:** There is no difference in mean accuracy between data with low no. of ingredients and high no. of ingredients.

**Alternative hypothesis:** There is a difference in mean accuracy betwene data with low no. of ingredients and high no. of ingredients. 

**Test statistic:** Absolute difference in mean accuracy

**Significance level:** 1%

#### Conclusion
The p-value is 0.8% which falls below the 1% threshold. Hence, we reject the null hypothesis which claimed there was no significant difference. Now while we may have rejected the null hypothesis, it doesn't mean we accept the alternative hypothesis. Based on our permutation test, we can only say that there is an association between the performance of the model and the group.

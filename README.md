# Titanic Competition
* Rank: 297/12.885 - top 2.3%
* Leaderboard Accuracy: 82.535%
* [Kaggle Notebook](https://www.kaggle.com/competitions/titanic/data) - Please "Upvote" if you find helpful - this helps other Kaggler to find it more easily!

# Original Features: 
| Variable | Definition | Key |
|----------|-----------|-----|
| survival | Survival | 0 = No, 1 = Yes |
| pclass | Ticket class | 1 = 1st, 2 = 2nd, 3 = 3rd |
| sex | Sex | |
| Age | Age in years | |
| sibsp | number of siblings / spouses aboard the Titanic | |
| parch | number of parents / children aboard the Titanic | |
| ticket | Ticket number | |
| fare | Passenger fare | |
| cabin | Cabin number | |
| embarked | Port of Embarkation | C = Cherbourg, Q = Queenstown, S = Southampton |

# Engineered features: 
| Variable | Method of creating | Purpose | 
|----------|--------------------|---------|
| Accompany | SibSp + Parch + 1 | Document passengers went in groups| 
| Title | Grouping title with similar charactersitics | Reduce number of categories | 
| Married | One-hot encoding on Mrs Title | Differenciating Mrs and Ms | 
| Family Name | Extract the family name in 'name' feature | Grouping people from same family | 
| Survival Rate | Target encoding using average survival rate of people with important similarities | Reduce categories + provide subtle target encoded information | 

# Model Selection
* Problem: Supervised Binary Classification with small dataset (891 instances)
* Suitable models: 
    * Bootstrap Aggregation Tree-based models: Random Forest, Extra Trees
    * Gradient Boosting models: Catboost (suitable for small dataset)
    * Logistic Regression
    * Supported Vector Machines
    * K Nearest Neighbors 
    * Gaussian Naive Bayes
* KNeighborsClassifier and GaussianNB perform with poor accuracy on train-validation set

# Hyper-parameter Tuning: Optuna Framework
## *model's objective* function
* Calculate accuracy on cross validation: **val_score**
* Calculate accuracy on train-validation set: **test_score**
* Compute gap = test_score - val_score
* Return value: val_score, gap
* Purpose: Evaluate the accuracy and generalizing ability of models 

## *fine_tune* function
* Input: objective function ({val_score, gap})
* Generate combinations of hyperparamters in directions that maximize val_score and gap
* Floor *gap* at 0 - meaning models with good generalizing ability won't be penalized
* Compute  Manhattan distance from ideal hyperparameters (100% accuracy and 0 gap)
* Filtering Models whose accuracy >= 80%
* Select one with smallest Manhattan Distance

# Leaderboard Result: 
| Model | Accuracy | 
|-------|----------|
|RandomForestClassifier|82.5%|
|LogisticRegression|80.3%|
|SVC|78%|
|ExtraTreesClassifier|81.3%|
|CatBoostClassifier|77%|
|Voting Classifier (random forest + logistic regression) | 82.5%| 

# Reference

This notebook couldn't been completed without referencing the wonderful works of the following authors:
* https://www.kaggle.com/code/thomascharuel/titanic-getting-above-80-accuracy
* https://www.kaggle.com/code/krishnamishras/titanic-prediction-model-84-accuracy
* https://www.kaggle.com/code/abhishek0032/titanic-survival-prediction-feature-engineering
* https://www.kaggle.com/code/vinothan/titanic-model-with-90-accurac

# Thank you for Reading!
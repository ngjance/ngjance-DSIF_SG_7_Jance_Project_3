## Overview

We are a nutritional meal plan provider company that provides different types of diet meal plans.

Example: Low-carb, Mediterranean, Flexitarian, Vegan, Keto.

We want to introduce a new keto meal plan, and would like to gather the experiences of people who have gone through / are going through keto diet.

Example, their likes, dislikes, struggles and success factors.

---

## The Problem

As we are also gathering data for intermittent fasting diet, the database is mixed up with both intermittent fasting and keto data.

Therefore, the goal is to correctly identify the posts relating to keto.

We want to reduce the number of posts that we predicted as keto but is actually intermittent fasting, that is, Type 1 error.<br>
This is because we want to gather the experiences unique to keto dieting so that we can provide the best keto diet meal plan experience to help our customers achieve their goals.

---

## Acceptance Criteria

- Minimun 5000 unique posts
- Number of words per post > 20
- Accuracy score >= 0.75
- Number of Type 1 error < 100
- F1 score >= 0.75

---

## Data Collection

- 15,000 title posts were collected from each subreddit, "Intermittentfasting" and "Keto".
- It was made sure that there are no duplicates in these 15,000 posts.
- It is to note that some of the posts contains selftext while others don't.

---

## Data Cleaning

- We first check that there are no empty values under both the selftext and title columns.
- We then noticed that there are some selftexts that contain "[removed]", which is not meaningful to our analysis and have to been removed.
- We then melt the "selftext" and "title" columns into one column.
- As one of our acceptance criteria is to have posts with more than 20 words, we dropped post with 20 or less words.
- This resulted to 7,010 observations for "Intermittentfasting" subreddit, and 9,011 observations for "Keto" subreddit.
- Lastly, we concat both the "Intermittentfasting" and "Keto" tables into one table.
- The total number of observations in the combined table is 16,021.

---

## EDA

- Most of the posts ranges from between 0 and around 1800 characters.
- Most of the posts ranges from between 0 and around 250 words.
- Most of the words are between 3 and 9 characters long.
- The common stopword is "and".
- The top three words are "I", "I'm" and "keto".
- The third fivegrams has the phrase "doesn't seem to".<br>
It seems that the post creators are probably trying something that doesn't seem to work.<br>
It is important to take note of what is probably not working for the users so that we can enhance our meal plan offerings.<br>
- From the fifth fivegrams onward, it seems that many users are new to keto and need some advices and info.<br>
This shows that many people who are new to keto diet are turning to reddit for advices.<br>
We need to deep dive into what are the newcomers' questions and concerns, and address them through our products.<br>
We should also target this group of people, and place targeted ads about our new keto diet meal plans.
- Most of the posts have neutral sentiments.

---

## Preprocessing

- Converted 'intermittentfasting' subreddit to '0', and 'keto' subreddit to '1'.
- Numbers, numeric values, punctuations, stop words and extra white spaces are removed from the posts.
- The words are then lemmatized and vectorized into vectors of real numbers.
- The dataset is split into train and test sets to prevent overspill.
- There is a slight imbalance in the train set.
- Three samplers are tested using base model "Random Forest"
    - RandomUnderSampler
    - SMOTE
    - SMOTETomek
All three samplers produce very similar outcomes.<br>
However, SMOTETomek gives the best accuracy and F1 score with lowest RMSE. Hence, we use SMOTETomek to balance the data.

---

## Base Model - Random Forest Classifer

- We use the default random forest model to compare with other classification models:
    - Logistic Regression
    - Bagging
    - XGBoost
- XGBoost has the lowest Type 1 error although the accuracy score is the second lowest.
- All four models were overfitted.
- The type 1 error is also more than our acceptance criteria.
- Hence we need to tune the models to reduce overfitting.

---

## Models - after Tuning

- The accuracy score is about 10 to 30% lower after tuning but the overfit situation is much better.
- This can be seen from the confusion matrix where the type 1 error is also reduced.
- The F1 score is also lower than before tuning.
- The only model that meets our acceptance criteria is Bagging.

---

## Conclusion

- The final Bagging model is the only model that meets all of our acceptance criteria.
- Although the accuracy score and the F1 score is the second lowest, it has the smallest overfit value of 1 basis point which can be overlooked.

---

## Recommendations

- We recommend to use Bagging with the below parameters to sort the data between "Intermittentfasting" and "Keto".
    - base_estimator = DecisionTreeClassifier(max_depth=7, ccp_alpha=0.05)
- From the earlier data analysis, it is found that there are many posts by new keto dieters seeking for advices.
- These posts provide valuable insights to us when marketing and producing our new keto meal plan.
- From these posts, we would be able to learn about the common challenges that the new keto dieters and introduce solutions to these challenges, thereby increase takeup rates for our new keto meal plan.
- We could also explore placing ads targeted at these new keto dieters, to help them have an easier dieting journey.
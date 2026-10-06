# Week 2.1 - EDA, Feature Engineering and XGBoost

## EDA

### Q1. What is EDA (Exploratory Data Analysis)? Why is it the first step before building any ML model?

**Answer:**

EDA means understanding our data before building the ML model.

First, we check what kind of data we have, how much data is there, whether there are missing values, and whether there are any patterns or problems.

It helps us understand the data properly before giving it to the model.

### Q2. What is class imbalance? Which class has the least tickets and why does it create a problem?

**Answer:**

Class imbalance means that some classes have many more tickets than other classes.
In our dataset, the **Critical class has the least number of tickets**.
This is a problem because the model gets fewer examples of Critical tickets. So, it may not learn how to identify Critical tickets properly and may predict them as High or another class.

### Q3. What does `df.describe()` show you?

**Answer:**

`df.describe()` gives us a quick summary of the numerical data.

From it, we can see:

- Number of values
- Average value
- Minimum value
- Maximum value
- Standard deviation
- 25%, 50%, and 75% values

### Q4. What is the difference between `value_counts()` and `value_counts(normalize=True)`?

**Answer:**

`value_counts()` tells us the actual number of records in each category.

`value_counts(normalize=True)` tells us the percentage of records in each category.

I use `value_counts()` when I want the actual count, and `normalize=True` when I want to understand the percentage distribution.

### Q5. The "Date of Purchase" column shows `object` instead of `datetime`. What does this mean and how do you fix it?

**Answer:**

It means Python is treating the date as text instead of an actual date.

We can convert it using:

```python
df["Date of Purchase"] = pd.to_datetime(df["Date of Purchase"])
```

After converting it, we can easily perform date-related operations.

# Feature Engineering

### Q6. What is Feature Engineering? Why can't you directly feed raw ticket text into an XGBoost model?

**Answer:**

Feature Engineering means converting raw data into useful features that the ML model can understand.

For example, our ticket contains:

```text
VPN is not working
```

This is text, and XGBoost cannot directly understand this text.

So, we convert the text into numbers using TF-IDF. We can also create features like `word_count`.

### Q7. What is TF-IDF? What do TF and IDF mean?

**Answer:**

TF-IDF is a method used to convert text into numbers and find important words.

**TF (Term Frequency)** means how often a word appears in a ticket.

**IDF (Inverse Document Frequency)** means how common or rare a word is
across all tickets.

For example, words like "the", "is", and "please" are common, so they are less important. Words like "VPN", "server", and "outage" can be more useful.

### Q8. What does `max_features=100` mean? What happens if we increase it to 500?

**Answer:**

`max_features=100` means TF-IDF will use a maximum of 100 words or features.

If we increase it to 500, the model can use up to 500 words or features.

This gives the model more information, but it can also make the model more complex and take more time to process.

### Q9. How does `word_count` help the model predict priority?

**Answer:**

`word_count` tells us how many words are present in a ticket.

A short ticket may have very little information, while a longer ticket may contain more details about the problem.

The model can use word count along with TF-IDF to find patterns related to ticket priority.

### Q10. What is the difference between `fit_transform()` and `transform()`?

**Answer:**

`fit_transform()` learns information from the training data and then converts the data into numbers.

We use it on the training data.

`transform()` uses the information already learned and converts new data into the same format.

So, we use `fit_transform()` for training data and `transform()` for test or new data.

# ML Model - XGBoost

### Q11. What is XGBoost? What does "boosting" mean?

**Answer:**

XGBoost is a machine learning algorithm that we use to make predictions.
Boosting means the model creates many small decision trees one after another. Each new tree tries to improve the mistakes made by the previous trees.Together, these trees make the final prediction.

### Q12. Why did we split the data into 80% train and 20% test?

**Answer:**

We use 80% of the data to train the model and 20% to test it.The training data teaches the model.The test data helps us check how well the model works on data that it has not seen before.If we trained on all the data, we would not have separate unseen data to properly test the model.

### Q13. What does `stratify=y` do and why is it important?

**Answer:**

`stratify=y` keeps the class distribution similar in both the training and testing data.This is important because our dataset has class imbalance.It helps make sure that classes with fewer tickets are also represented in both the training and testing data.

### Q14. What is a Label Encoder? Why did we use it?

**Answer:**

A Label Encoder converts text labels into numbers.

For example:

```text
Critical -> 0
High -> 1
Low -> 2
Medium -> 3
```

We use it because the ML model works with numerical values instead of text labels.

### Q15. What does `n_estimators=100` mean?

**Answer:**

`n_estimators=100` means XGBoost will create 100 decision trees.

If the number is too low, the model may not learn enough.

If it is too high, the model can take longer to train and may overfit the training data.

# Scenario-Based Questions - Week 2

### Q1. The XGBoost model got 90% accuracy. Why might this accuracy be misleading?

**Answer:**

It could be because of class imbalance.

For example, if 90% of the tickets are Medium, the model could predict Medium for almost every ticket and still get 90% accuracy.
But it may not actually learn how to identify Critical or High tickets.That's why we also check F1-score, precision, recall, and the confusion matrix.

### Q2. The model almost never predicts Critical correctly. What could be the reasons and how would you fix it?

**Answer:**

One reason could be that there are very few Critical tickets in the training data.Another reason could be that Critical and High tickets have similar descriptions, so the model finds them difficult to separate.We can improve this by adding more Critical examples, using oversampling techniques like SMOTE, or using appropriate class weights.

### Q3. A ticket says "urgent pls fix asap" but the model predicts Low. Is the model wrong?

**Answer:**

The model may not necessarily be completely wrong.The ticket has only four words, so the model has very little information to work with. The TF-IDF features may also not have enough useful information.We can improve the model by adding the subject line, creating urgency-related features such as `urgent`, `critical`, and `asap`, and adding more similar examples to the training data.

### Q4. The saved model gives an "XGBoost version mismatch" error. What happened?

**Answer:**

The model was probably trained using one version of XGBoost, while the teammate is using a different version.Different versions can sometimes cause compatibility problems.To prevent this, we should save the exact package versions in a `requirements.txt` file so everyone uses the same versions.

### Q5. Why did we use TF-IDF instead of just counting words?

**Answer:**

Simple word counting treats words more equally.
For example, words like "the", "is", and "please" can appear in many tickets.

TF-IDF gives less importance to common words and more importance to useful words such as "VPN", "database", and "outage".

So, TF-IDF can give the model more useful information.

### Q6. Can we use the same TF-IDF vectorizer for a new batch of tickets?

**Answer:**

Yes. We should use the same vectorizer that was used during training.

For new tickets, we use:

```python
tfidf.transform(new_data)
```

We should not use `fit_transform()` again because that can change the vocabulary and feature positions.

The model expects the same features that it saw during training.

### Q7. The model has 75% F1-score on test data but gets 4 out of 10 real tickets wrong. Why?

**Answer:**

The test data may be similar to the training data, but real tickets can be different.For example, people may use different words, new types of problems may appear, or tickets may come from different departments.This is called **data drift**.So, a good test score does not always guarantee the same performance on new real-world data.

### Q8. Can we use the model in production to automatically close tickets?

**Answer:**

I would say no, not directly.The model can make mistakes, and automatically closing a ticket could cause a serious problem if an important ticket is classified incorrectly.I would recommend using the model to suggest the priority first. Then a human can review the prediction and make the final decision.This is called a **human-in-the-loop approach**.

### Q9. What happens if we forget to save the TF-IDF vectorizer?

**Answer:**

The model cannot directly understand new ticket text without the TF-IDF vectorizer.

The vectorizer converts the text into the numerical features that the model expects.

If the original training data is available, we can fit the vectorizer again using the same training data and recreate the required vocabulary and feature mapping.

The important lesson is to save all the required files together:

```text
model.pkl
tfidf_vectorizer.pkl
label_encoder.pkl
```

### Q10. Someone gets 95% accuracy but never predicts Critical or High correctly. Is their model better?

**Answer:**

No.

Their accuracy may be high because there are many more Medium or Low tickets.

But if the model cannot identify Critical or High tickets, then it is not useful for our actual purpose.

So, we should not compare models using accuracy alone.

We should also check:

- Precision
- Recall
- F1-score
- Confusion matrix
- Performance for each class

A model with lower accuracy can actually be better if it identifies the important classes more effectively.

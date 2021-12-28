# Drugs Reviews Sentiment Analysis 
Text Mining project realized as a part of the 	*Data Mining, Text Mining and Big Data Analytics* 
exam  of the [Master's degree in Artificial Intelligence @ University of Bologna](https://corsi.unibo.it/2cycle/artificial-intelligence). 

The aim of this project is to **compare different text representations** and **learning models** in the context of a classification task.

The dataset used is the [Drug Reviews Dataset](https://archive.ics.uci.edu/ml/datasets/Drug+Review+Dataset+%28Drugs.com%29).

## Overview 
I understood and cleaned data by performing some Exploratory Data Analysis (EDA) and Text Preprocessing. 

Since the dataset does not provide a sentiment target variable, I labelled the reviews using two different strategies: 
- Ratings
- VADER

I used three different text representations: 
- CountVectorizer (BoW)
- TF-IDF 
- Dense Word Embedding (Glove)

For each of them I compared three different algorithms for classification: 
- Naive Bayes 
- Logistic Regression
- Random Forest  

For some of these configurations I ran a GridSearch to find the best hyper parameters.

For each model, I computed the 95%  **confidence interval on accuracy**.

Finally, I compared the models in pairs through the **significance test** to determine if there is a model which 
can be considered better than the others; in particular, I compared the performance of the best models found 
through the various grid searches by statistically compared the accuracy on the test set.

## Usage
The [code](https://github.com/helemanc/drugs-reviews-sentiment-analysis/notebook/Drugs_Reviews_Sentiment_Analysis.ipynb) 
can be directly ran on Google Colaboratory or through any 
jupyter server. 

**NOTES**

Some sections may take a long time to run.

Therefore, in [this folder](https://github.com/helemanc/drugs-reviews-sentiment-analysis/pickle_files)  you can find 
the files to load during the execution; in particular:
- ***df.p*** to skip the "Data Understanding", "Text Preprocessing" and "Labeling" sections, and go directly to "Models 
 Load Clean Dataset".
- ***X_train.p, X_test.p, Y_train.p, Y_test.p*** to skip the preparation part of the embedding matrix and data in the 
 "Dense Word Embedding" section and go directly to "Dense Word Embedding -> Load Input Data".

If you have problems downloading the files, you can also find them in [this](https://drive.google.com/drive/folders/1w2Dw7jpOlvdV4gUDqPO63VXQBp_bhM-0?usp=sharing)
Google Drive folder.

## Results
In light of the work done I can conclude the following:
- Thanks to the analysis carried out in the "**Data Understanding**" section I was able to note that the dataset is very unbalanced since almost 70% of the votes have a value greater than 7. 

    Indeed, if we analyze the classification reports we can see that the support value between the two classes is really 
    different (we will comment later on the classification report obtained from our best model).
- Furthermore, again thanks to the work carried out in the "**Data Understanding**" section, I was able to eliminate the records that contained *null values* and which would have led to partially inconsistent results. In particular, most of the null values were present in the *condition* field. Thanks to this cleaning job I was only able to get the completely correct reviews.
- It was also interesting to investigate two methodologies for the attribution of the *sentiment* variable, although in the end  I decided to keep only the labels built through the **rating based** strategy.
- The exploration of hyperparameters could be much more extensive, but it was necessary to find a tradeoff between the time needed to search for them and the time available for experimentation.
- Models based on the Random Forest classification algorithm were the worst in terms of performance. However, this must not mislead us; in particular, I cannot conclude that Random Forest is the absolute worst classifier for the purpose of solving our task, because I have not performed an exhaustive analysis of its hyper parameters. For issues related to the execution time of the classifier, I set parameters in a fairly random way that allowed me not to have a very long execution time. 

    Actually, in order to perform a more consistent comparison you would have to go and rerun the tests by testing different hyper parameters for this classifier, but this eluded the objectives of this work.
- In light of the **comparison between models** carried out on the basis of the significance test I can conclude that, with a 95% of confidence, there is a statistically significant difference between most of our models (i.e. hardly due to chance). 

    This means that, in relation to the size of the test set on which the models were tested (the greater the sample size, the greater the accuracy of the test), I am quite certain (95%) that the probability that the observed difference in accuracy may be ascribed to the case is low. Therefore, in ordering my tests based on the accuracy obtained, a probability of failing is less than 5% (alpha = 0.05) (except for models 6 and 7).
- I can therefore conclude, with 95% of confidence that among the models tested **there is a better model than all** (`Model 2: Count Vectorizer + Logistic Regression`). Therefore, for what I believe to be the best model based on scientific evidence, I report the *classification report* below:

    ```
                   precision    recall  f1-score   support

           0         0.91      0.86      0.88     21056
           1         0.94      0.96      0.95     48939

  accuracy                             0.93     69995
  macro avg        0.92      0.91      0.92     69995
  weighted avg     0.93      0.93      0.93     69995
    ```

  - Analysis of the classification report:
      - As I announced earlier, the `support` value between the two classes (0 = negative, 1 = positive) is very different, so the dataset is very unbalanced.
      - The `recall` means "how many of this class you find over the whole number of element of this class". 
      - The `precision` means "how many are correctly classified among that class." The result obtained in terms of precision makes us understand that almost all of the reviews have been correctly classified in those classes; so, my classifier works well.

- The best model **confusion matrix** is the following:
   ```
      [[18119  2937]
      [ 1830 47109]]
    ```
  As can be seen, most of the instances have been classified correctly (elements on the diagonal).
- An unexpected result: CountVectorizer, the simplest strategy for encoding text, turned out to be the best. There is no reason why TF-IDF would give more information for a classification task. It performs well for search and ranking, but classification needs to gather similarity, not singularities.



## Further Works
  - A very unbalanced dataset can greatly influence the classification results, therefore a possible future development could be to adopt a strategy to rebalance the dataset; possible strategies could be data oversampling or term weighting.
  - It would be interesting to repeat the work by also considering the 'neutral' class as well as' positive 'and' negative.

- It would be interesting to observe if, by training Dense Word Embedding on our corpus, we can obtain better results. In particular, both in order to improve spell checking and to improve the quality of the embedding matrix, it could be useful to train both tools on texts that deal with topics similar to those of our dataset.

- It would be interesting to broaden the analysis, for example by going to see what happened on March 1st 2016 and why there are so many reviews for that day (like seeing if any bot or pharmaceutical company has self-rated and reviewed to get it 'false approval' and boost its popularity in the market).

- It would be interesting to list the names of the drugs and remove them from the reviews since they are all names that are certainly not stemmed and that are certainly OOV in dense embeddings. 

## License 
This project is licensed under the Apache License 2.0 - see the [LICENSE](https://github.com/helemanc/drugs-reviews-sentiment-analysis/blob/main/LICENSE) file for details.
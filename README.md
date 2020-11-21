# Final Project

- Danesh Badlani
- Sam Bluestone
- Leslie Le
- Joe Salerno

## Dataset and Study

A study on statistics taken from Portuguese school children. Given the children's performance in two core classes, Mathematics and Portuguese, the machine attempted to predict the children's performance in secondary school, which will help improve the quality of education and enhance school resource management.

The dataset contains educational statistics of 395 students with 33 features, 16 of these being non-numerical data. Although of these 16, depending on how you transform the data, at least 11 features can be defined as binary features. Also, there are no missing features within this dataset.

Consult raw-data/student.md for all of the descriptions and names of the features within the dataset and how these features are being defined - binary, nominal, or numeric.

## Feature Transformation

We used the last three columns to create a scores column, which is the target variable. These last three columns - G1, G2, and G3 - represent the student's first-period score, second-period score, and final score, respectively. We decided to compile these scores together by averaging the three scores and assigning those values according to the letter grades mentioned in the paper. We ended up mapping these ordinal values into integers.

We also decided to binarize the following features:

- school
- sex
- address
- famsize
- Pstatus
- schoolsup
- famsup
- activities
- paid
- internet
- nursery
- higher
- romantic

The reason we decided to binarize these features was to allow for easy processing since these features contained two values each. For example, 'male' was assigned 1 while 'female' was replaced by 0 in the sex feature. Moreover, all the features that contained 'yes' and 'no' were replaced by 1 and 0 respectively.

We decided against using a one-hot-encoding vector for these features because doing so may increase the processing time and the complexity of finding the correlations between features (due to all the new columns that will be created).

## Feature Selection

We used a **RandomForest feature** selector to determine which features were the most important towards determining the value of the target feature. Because we did not explicitly define the threshold parameter for the feature selection process, it automatically set the threshold at the average importance of all of the features. This yielded a total of 14 features that were greater than the mean of all of the importances. We chose to stick with the mean because the importances were clustered relatively close together, and using the mean as the threshold yielded a reasonable number of features. If these features turn out to not yield good results when we start implementing the model, we will increase or decrease the number of features appropriately. According to the graph generated by the random forest, the top five features that were the most important to determining the target feature the most were 'absences', 'freetime', 'Fedu', 'goout', and 'health'.

The **heatmap correlation graph** is a large graph that shows the correlations between the 14 features selected by the Random Forest feature selection. Rather than comparing the number of features that can be found on the dataset, the heatmap compares a larger number of features due to the one-hot-encoding vector of the categorical features: Mjob, Fjob, reason, and guardian. According to the heatmap, the features that are most correlated to the target feature are the mother's job type ('Mjob'): 'other' and 'services', 'Medu', 'romantic', 'Fedu', and 'health'.

According to the two feature selection algorithms, the agreed-on features that correlate most to the target features are 'Fedu' and 'health'. We believe that we should not use many of the binary features or categorical features due to the features' low variances (the features do not assist in predicting a correct model or the feature does not provide enough information).

## Feature Extraction

For the **Principal Component Analysis (PCA)**, it is recommended that one-hot encoded binary features should not be used in this algorithm. Due to this, we used only the continuous features in the PCA. We tested two separate classifiers, one being regular PCA and the other being KPCA (Kernel). The result we got from them differed drastically because it appears that our dataset is not linear. For PCA, our output consisted of a graph displaying the separation of the classes. In this graph, it is evident that the classes are laid on top of one another, demonstrating that these classes are not linearly separable. For KPCA, we got more promising results because KPCA excels on non-linear dimensionality reduction. The graph produced shows the classes are well separated with the use of kernels, revealing that we have a suitable dataset for non-linear classifiers. Thus, the use of both methods of PCA has given us a lead on what classifier might perform best on the given dataset because it is not linear. So, we are inclined to believe that classifiers such as SVM might work best for our model of the dataset although, this will not stop us from exploring many potential models.

The goal of performing **Linear Discriminant Analysis (LDA)** is to both increase the computational efficiency and reduce the chances of overfitting the model to the testing set. Additionally, LDA assumes that the data is normally distributed, so we decided to scale the data using the Normalizer from the sklearn API before performing LDA on the dataset. For this LDA, we chose to do it with the first two components so that it could be visualized, even though we could hypothetically use up to 3 (the sklearn API says that n_components should be strictly less than the number of classes - 1, which for us would be 4). When we start applying models to our data, we will try to use LDA with more than 3 components. The decision region plot that LDA produces does not separate the data effectively, but this is most likely because we are only using 2 components. Again, the performance will most likely improve when we use more components. It is also possible that there are different covariances between our classes, which is another assumption that LDA makes that might not necessarily be true.


## Random Forest: 

Primarily, we did a grid search on all our feature extraction methods to determine which one performed the best, which ended up being no feature extraction by a significant margin. Then, we did a couple more grid searches to determine the best parameters for this model, eventually landing on 90 estimators, gini criterion, max depth of 13 and a random_state of 6. Now, upon testing this model on varying numbers of features (as seen by the graphs provided), we figured out that RF performed better than SBS in terms of metrics. Lastly, we were able to see that the model achieved a best accuracy of roughly 0.83 with 38 features and at the same time achieved a CV validation set accuracy of 0.75 (+- 0.3), thus showing us that this model is not overfit on the dataset. It must be noted that there is still a possibility for further tuning that may be able to increase this accuracy score and the other metrics (all currently above 0.8 in this listed best case), but these would be very minor adjustments. 

## Decision Tree:  

We tested Decision Tree thoroughly but were not able to achieve accuracy above 0.6, thus we decided not to pursue this classifier. 


# Times
SVM: ~9:23.55
Logreg: ~8:07.08
KNN: ~14:03.58
DT: ~0:53.35
RF: ~3:35.84

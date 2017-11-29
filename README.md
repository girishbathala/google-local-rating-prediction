# google-local-rating-prediction

### 1. Objective
The goal of this task is to predict a user’s star rating of a particular business. The following pipeline was
followed to implement the task :

### 2. Data-set preprocessing and preparation

The Google Local dataset has 200,000 reviews of businesses’ rated by users. The dataset contains many 
attributes but for the purpose of this task, only the following were used:
* businessID: The ID of the business. This is a hashed product identifier from Google.
* userID: The ID of the reviewer. This is a hashed user identifier from Google.
* rating: The star rating of the user’s review.

The dataset is divided into train, validation using k-fold ( k = 3) split. The entire 200,000 data points were used for building
the final model once all the parameters were estimated after validation

### 3. Recommender System Models Used

The approach used was to build an ensemble of 6 different recommender system algorithms to model the
relation between the users and businesses. For each algorithm the 200,000 X 3 train-set was converted into a 
pandas dataframe and then was represented as a surprise trainset object before passing it into surprise’s algorithm libraries

### 3.1 BaselineOnly
Algorithm predicting the baseline rating estimate $\hat{r}_{ui}$ for given user $u$ and item $i$.
$$\hat{r}_{ui}=b_{ui}= \mu +b_u+ b_i $$

If user $u$ is unknown, then the bias $b_u$ is assumed to be zero. The same applies for item $i$ with $b_i.$

This model tries to minimize the following regularized  square error, where $\lambda$ is the regularization parameter.
$$\sum_{r_{ui} \in R_{train}} \left(r_{ui} - (\mu + b_u + b_i)\right)^2 +
\lambda \left(b_u^2 + b_i^2 \right)$$


Two separate models are built using the "baselineOnly" idea from surprise :

Using Stochastic Gradient Descent (SGD): The parameters for SGD are :
– ’reg’ or lambda: The regularization parameter of the cost function that is optimized
– ’learning rate’: The learning rate of SGD
– ’nepochs’: The number of iteration of the SGD procedure

UsingAlternating Least Squares(ALS): The update equation forμis the simple average of allthe all ratings centered after subtractingbu+bi. The update equations forbuandbiare in fig : ,where K is theRtrainset,λ2=regi,andλ3=regu,Parameters for the ALS algorithm are :

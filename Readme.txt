The most basic recommendation systems make recommendations based on the average ratings of its users.  
The model is trained by calculating an average rating for each item using the training set.  At prediction time, the average ratings are returned as the predicted ratings for the target user.

 

Data Description

Recommendation data are often represented as matrices to make various numerical calculations more efficient. 
 Users are indexed along rows, and items are index along columns.  The entry at row i and column j gives user's i numerical rating of item j.  
 Explicit ratings might be represented by the numbers 1 to 5, while implicit ratings might be represented as 0 (no interaction) and 1 (interaction).

The ratings are often very sparse – most users only rate a handful of items.  In an ideal world, we would use null values to indicate unrated items.  
Unfortunately, numerical algorithms don't aren't aware of null values.  Instead, values of 0 are often used to indicate an item hasn't been rated. 
 This is consistent with the representation of implicit ratings but requires special handling for explicit ratings.

Rating matrices are often too large to represent using 2D Numpy arrays.  Instead, special sparse matrix representations are used. 
 Scipy provides several implementations, including compressed sparse-row (CSR) matrices which are optimized for accessing data by row and compressed sparse-column (CSC) matrices which are optimized for accessing data by column.

 

Interface

Your recommendation algorithm implementations will implement a modified Scikit-Learn-like interface.  There will be three methods:

The constructor will take any parameters needed for the algorithm.  The parameters will vary between algorithms.
void fit(training_ratings). Trains the model on ratings for N users and M items.  The ratings are expected to be represented by a N x M CSR sparse matrix.
predicted_ratings predict(user_ratings). Predicts ratings for users based on their known ratings. 

The method takes a N x M CSR sparse matrix as input and returns a N x M CSR sparse matrix of predicted ratings.
  The input matrix will be relatively sparse (few ratings), while the output matrix will be much denser (many ratings).
 

The Average Ratings Algorithm

The average ratings algorithm makes predictions exactly as the name suggests.  Average will be computed for each item from the ratings in the training set. 
 At prediction time, the output matrix will consist of the average ratings repeated along the rows – once for each user.
   There are two ways to compute the ratings: including or excluding the zeros when calculating the averages.  If zeros are not included, then the average ratings represent the average ratings of users who rated the items.
     In this version, we assume that an unrated item has no meaning.

If zeros are included, we assume the act of rating or not rating an item is a signal.  The user chose not to rate some of the items.  
This might be because a user isn't interested in a particular movie or musical artist.  Since they avoided watching the movie or listening to the artist's music, they had no basis to rate the item. 
 It should be noted that this assumption may not hold true when the number of items is very large – a user obviously can't make a decision about every or even most of the items that Amazon sells.

In either case, if all of the ratings for an item are zero, the average should be set to zero to avoid NaNs from dividing by zero.

Your implementation should take a parameter in the constructor that indicates whether to include the zeros in the averages.

You should write unit tests for your implementation using Python's unittest library.

 

Data Preparation

You will evaluate the algorithms on the 25 million rating MovieLens data setLinks to an external site..  You will need to read the data and construct a sparse matrix of the ratings.  
The data have already been cleaned – only users with 20 or more rated items are included in the data set.  Note that the item (movie) ids are not contiguous.  You will need to remap the ids using code like the following:

 

original_movie_ids = set(df["movieId"])
movie_id_map = {original : new for new, original \

in enumerate(original_movie_ids) }
df["movieId"] = df["movieId"].map(movie_id_map)

 

Experimental Setup

For the purposes of this class, we are going to use a two-level experimental design.  The algorithms train models on the training data. 
Divide users into training and testing groups.  Use 80% of the users for training and 20% of the users for testing.  The ratings for the training users can be used to train the models.

The models use the target users' existing ratings to generate predicted ratings for unseen movies.  
For each test user, randomly assign 80% of their ratings to a "seen" group and 20% to an "unseen" group.
Do not assign movies to one group or another for all users; for each user, select random subsets of their ratings for the groups.  
Ensure that no (user, movie) combination is present in both the seen and unseen groups.  Use the seen ratings as input into the model. 

 

Evaluation

Evaluate the predicted ratings from the model against the unseen ratings that were held out for each user.  You will need to reshape the ratings across all users into two lists.  One list will contain the true ratings, while the second list will contain the predicted ratings.  Exclude ratings generated for a user-movie pair but the user hasn't rated the movie.

You can think of this as iterating over two matrices of user-provided and predicted ratings in parallel.  The metrics can only be calculated using the pairs for which user-provided ratings are available.  Exclude all pairs that have a value of 0 (not rated) in the user-provided ratings.

You will use four ways of evaluating the predicted ratings:

Root mean-squared error (RMSE)
Boxplot of true vs predicted ratings
Pearson's Correlation Coefficient (R2)
Fraction of user-movie pairs with non-zero predicted ratings
Fraction of user-movie ratings with a predicted values (recall)
RMSE is appropriate if we want to exactly the predict the ratings of the users.  R2 is useful if we don't care about the ranges of the ratings, just that the predicted ratings rank items in a manner consistent with the user-provided ratings.  The MovieLens ratings are in increments of 0.5.  This makes it easy to visualize the relationships between the user-provided and predicted ratings using a boxplot (user-provided ratings on the X axis, predicted ratings on the Y axis).

Evaluate the average ratings algorithm in both modes (zeros included and zeros not included).

 

Submission

You should submit a Jupyter notebook (HTML format) with your code and analyses.  Include your name and a summary of the work, results, and what you learned in bulleted form at the top or bottom of your notebook.
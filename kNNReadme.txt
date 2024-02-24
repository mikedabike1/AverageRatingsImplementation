K-nearest neighbors (kNN) has largely fallen out of use in most areas of machine learning, but it's still used in recommendation systems to generate personalized recommendations.

 

The kNN Algorithm

kNN calculates ratings for each user by finding the k most similar users in the training set and calculating the average ratings from those training users.

 

There are two stages to the kNN algorithm:

Use the NearestNeighbors class from Scikit-Learn to find the k nearest neighbors in the training set for each user. The class takes two critical parameters: k and the similarity metric (Euclidean and Cosine similarity are common. The kneighbors(user_ratings) method takes a N x M sparse matrix of user-provided ratings and returns a N x k matrix giving the indices in the training set of the k closest neighbors for each user.
 

Calculate the average ratings. This is a direct application of the algorithm from the previous assignment. For each user, the averages should be calculated using the ratings from the k neighbors.
 

You should write unit tests for your implementation using Python's unittest library.

 

Evaluation

Evaluate the kNN algorithm with the two similarity metrics (Euclidean and Cosine), whether or not the ratings are weighted by inverse document frequency, and normalized (none and L2) using Scikit Learn's TfIdfTransformerLinks to an external site., and a range of values for k (e.g., 5, 10, 25, 50, 100).  Which combination of parameters gave the best results?

 

Submission

You should submit a Jupyter notebook (HTML format) with your code and analyses.  Include your name and a summary of the work, results, and what you learned in bulleted form at the top or bottom of your notebook.
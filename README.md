# Book Recommendation Algorithm using K-Nearest Neighbors

## Overview

This project develops a book recommendation system using the K-Nearest Neighbors (KNN) algorithm. It utilizes the Book-Crossings dataset, a rich collection of book ratings, to find books that are similar to a user-specified title based on rating patterns from other users.

## Dataset

The project uses the following files from the Book-Crossings dataset:

* `BX-Books.csv`: Contains book metadata such as ISBN, title, author, etc.
* `BX-Users.csv`: Contains user information like user ID, location, and age.
* `BX-Book-Ratings.csv`: Contains book ratings (1-10) by users for specific books.

The dataset is automatically downloaded and unzipped as part of the notebook execution.

## Implementation

The core of the recommendation system is implemented in Python using the following libraries:

* **pandas:** For data manipulation and analysis.
* **numpy:** For numerical computations.
* **scipy.sparse:** For creating sparse matrices, which are efficient for handling large datasets with many zero values (users not rating most books).
* **sklearn.neighbors:** Specifically, the `NearestNeighbors` class is used to implement the KNN algorithm for finding similar books.

The process involves the following steps:

1.  **Data Loading and Initial Exploration:** The necessary CSV files are loaded into pandas DataFrames.
2.  **Data Cleaning and Preprocessing:**
    * Users with less than 200 ratings are filtered out to ensure more reliable rating patterns.
    * Books with less than 100 ratings are also filtered out for better statistical significance.
    * The ratings data is merged with the book titles.
    * Duplicate ratings and rows with missing book titles are handled.
3.  **Creating the User-Book Rating Matrix:** A pivot table is created with book titles as index, user IDs as columns, and ratings as values. Missing ratings are filled with 0.
4.  **Converting to Sparse Matrix:** The user-book rating matrix is converted into a sparse matrix using `csr_matrix` for efficient computation of distances.
5.  **Training the KNN Model:** A `NearestNeighbors` model is initialized with the cosine distance metric (to measure the similarity of rating vectors), the 'brute' force algorithm, and set to find the 6 nearest neighbors (including the book itself).
6.  **Recommendation Function (`get_recommends`):** This function takes a book title as input and performs the following:
    * Finds the index of the given book title in the user-book matrix.
    * Uses the trained KNN model to find the 5 nearest neighboring books based on cosine distance.
    * Returns a list where the first element is the input book title, and the second element is a list of the 5 recommended books along with their cosine distances from the input book.
    * Handles cases where the input book title is not found in the dataset.

## Usage

The primary function for getting recommendations is `get_recommends(book_title)`. You can use it as follows:

```python
recommendations = get_recommends("The Queen of the Damned (Vampire Chronicles (Paperback))")
print(recommendations)

# Infocamp2020 Data Analysis
Algorithm to analyze student questions from Piazza for Infocamp 2020

# Data Source
I pulled our sample of ~200 questions from the Piazza discussion board from [CS10 the beauty and joy of computing](https://cs10.org/su20/). The data contained three data structures with student, contribution, and post information.

# Data Preparation
I used a modified version of the [Bloom and Garrison et al 2001](https://pdfs.semanticscholar.org/edfa/79787c645169ec36344ad3a8946956e09ff7.pdf) methods to label the data between 4 categories.

0. Other/Logistics
1. Recall/Remember
2. Exploration/Analysis
3. Create/Expand

Here is the raw student data and content data structures.
![student data](https://github.com/graceyraspberry/Infocamp2020data/blob/main/images/student.png)

![content data](https://github.com/graceyraspberry/Infocamp2020data/blob/main/images/content.png)

The `all_data` DataFrame contains the labeled data that we used to train our model. It contains 21 columns, but the ones we care about are:

1. `id`: An identifier for the training example
1. `bloom`: The modified Bloom-garrison score based on the 4 categories above. 
1. `question`: The text of the question.
1. `folder`: The type of discussion post, we chose the categories `exams`, `problemset` and `projects`.
1. `subject`: The original discussion post title.
1. `module`: The subject module that came from another layer we labeled between `subject` and `folder`.

The original data was hierarchical with `thread_id` connected to the parent question and many layers of tags and authors. The post-level title was too low level, and the folder level was too broad, so we categorized the subjects further into modules (which you can view in the labeled/subject.csv) file. 


## Training the Model
I split the data into 80-20 train val split using sci-kitlearn.

# Feature Engineering

We would like to take the text of a question and predict what category under the bloom-garrison score. This is a classification problem, so we can use logistic regression to train a classifier. Recall that to train an logistic regression model we need a numeric feature matrix $X$  and a vector of corresponding binary labels $Y$ . Unfortunately, our data are text, not numbers. To address this, we can create numeric features derived from the question text and use those features for logistic regression.

Each row of $X$ is an email. Each column of $X$ contains one feature for all the emails. 

# EDA
To identify some features that allow us to distinguish the question categories. We compared the distribution of a single feature in each category to distinguish between them. 

Here is the histogram plot of each folders' bloom scores:
![distrib plot](https://github.com/graceyraspberry/Infocamp2020data/blob/main/images/countplot.png)

And a correlation plot across some of the other features.
![corr plot](https://github.com/graceyraspberry/Infocamp2020data/blob/main/images/corrplot.png)

# Aggregating Data for Backend

I aggregated the data with total question counts and predicted bloom proportions at the "class" level, "subject" level and "student" level. This made it easy for visualization on the dashboard.

Here is a plot of the student data:
![student plot](https://github.com/graceyraspberry/Infocamp2020data/blob/main/images/studentplot.png)

# Discussion of methods
Our sample size was very small, so our model has high variability and may lead to bias. We used 5-fold cross-validation to resample and retrain our model.

# Dashboard Visualization

Our final dashboard is a full stack web app built with Django and HTML/Javascript. The github repo is [here](https://github.com/maxsonyang/Infocamp2020).

![dashboard preview](https://github.com/graceyraspberry/Infocamp2020data/blob/main/images/ezgif.com-video-to-gif%20(1).gif)

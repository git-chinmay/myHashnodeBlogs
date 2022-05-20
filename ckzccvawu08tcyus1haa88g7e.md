## Question Classification using SVM

Question classification is very important for question answering and today in this blog we will try to summarize a whitepaper Titled same as the name of this blog. The paper was originally published by Dell Zhang and Wee Sun Lee from National University of Singapore around year 2003. Although its an old paper but still its holds relevance today for understanding the core concept.

## Introduction

If we go back to a decade that time the search engines generally performs document retrieval, means you put some keywords and the engine will show you stack of documents that contains those keywords. However, what a user really wants is often a precise answer to a question. For instance, given the question “Who was the first American in space?”, what a user really wants is the answer “Alan Shepard”, but not to read through lots of documents that contain the words “first”, “American” and “space” etc.

In order to correctly answer a question, usually one needs to understand what the question asks for. Question Classification, i.e., putting the questions into several semantic categories, can not only impose some constraints on the possible answers but also suggest different processing strategies. For instance, if the system understands that the question “Who was the first American in space?” asks for a person name, the search space of answers will be significantly reduced.

Although it is possible to write some rules for question classification but it is a tedious job and the paper shows how we can automate the question classification using Machine Learning. 

## Question Classification

The paper uses question datasets which has  6 coarse(rough) grained categories and 50 fine grained categories, as shown in below table. Most question answering systems use a coarse grained category definition.

![pic1.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644216253564/0QcqjxNtG.jpeg)

To simplify the following experiments, we assume that one question resides in only one category.
Paper used the publicly available dataset from USC , UIUC  and TREC. All these datasets have been manually labelled according to the coarse and fine grained categories as per above table. 5500 questions used  for training and 500 for test.

## Features

Authors considered the surface text features of questions. For each question, they extracted two kinds of features: bag-of-words and bag-of-ngrams (all continuous word sequences in the question). Every question is represented as binary feature vectors, because the term frequency (tf) of each word or ngram in a question usually is 0 or 1.


## Comparing SVM and Other machine learning algorithms

Authors tried to train 5 models with same data and compare individual results based on the accuracy evaluation metrics. They have experimented with  Nearest Neighbors (NN), Naïve Bayes (NB), Decision Tree (DT), Sparse Network of Winnows (SNoW), and Support Vector Machines (SVM).
Everybody acquainted with the above algorithms may be except SNoW. For those who don’t know about it SNoW or Sparse Network of Winnows  algorithm  is specifically tailored for learning in the presence of a very large number of features and can be used as a general purpose multiclass classifier. The learned classifier is a sparse network of linear functions. The SNoW software developed by the Cognitive Computation Group at UIUC.

### Experiment Results

The question classification accuracy using different machine learning algorithms, with the **bag-of-words** features, under the** coarse grained category** definition.

![pic2.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644216414982/S7NS8Hkmz.jpeg)

The question classification accuracy using different machine learning algorithms, with the **bag-of-ngrams** features, under the **coarse grained category** definition.

![pic3.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644216459365/X-YlgVGwS.jpeg)

The question classification accuracy using different machine learning algorithms, with the **bag-of-words** features, under the **fine grained category** definition.

![pic4.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644216499465/zG3s3WV3a.jpeg)

The question classification accuracy using different machine learning algorithms, with the **bag-of-ngrams** features, under the **fine grained category** definition.

![pic5.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644216540196/BiEg43zh24.jpeg)

Not surprisingly, classifiers trained on larger training dataset usually get better performance. For the SVM algorithm, we observed that the bag-of-ngrams features are not much better than the bag-of-words features and also to add on the liner kernel produced almost same result as RBF and Polynomial hence for above comparison authors stick to the Liner kernel.
In summary, the experiment results show that with only surface text features the SVM outperforms the other four methods for this question classification task.

## A Tree Kernel

The feature representation technique we are using here are not capturing the syntactical structure of the sentence bcz they are just bag of words which is a limitation. For example, the two questions “Which university did the president graduate from?” and “Which president is a graduate of the Harvard University?” could be discriminated by their different syntactic structures but here it’s the same due to the feature limitations. So the authors proposed to use a special kernel function called tree kernel to enable the SVM to take advantage of the syntactic structures of questions.
The core concept of tree kernel is “Given a question, we first parse(MEI parser) it into a syntactic tree using a parser , and then we represent it as a vector in a high dimensional space which is defined over tree fragments. The tree fragments of a syntactic tree are all its sub-trees which include at least one terminal symbol (word) or one production rule, with the restriction that no production rules can be broken into incomplete parts.” For details, please refer  [here](https://en.wikipedia.org/wiki/Tree_kernel).

There are two parameters for the tree kernel: λ and µ . The default value of λ is set to 0, and the default value of µ is set to 1, so that the tree kernel with default parameter values is identical to the word linear kernel.

The question classification accuracy using SVM based on word linear kernel, ngram linear kernel, and tree kernel ( λ = 0.1 , µ = 0.9 ), under the coarse grained category definition.

The statistical sign test can be applied for comparing two methods A and B . Below table shows the significance test results, where method A is the SVM based on tree kernel ( λ = 0.1 , µ = 0.9 ) and method B is the SVM based on word or ngram linear kernel, both are trained on 5,500 examples and tested on 500 examples, under the coarse grained category definition.


![pic6.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644216652142/GvoTwrEFV.jpeg)

The small P-value means that method A is significantly better than method B

## Conclusion

This paper presents the research work on automatic question classification through machine learning approaches. The main contributions of this paper are as follows. 

1. To show that with only surface text features SVM outperforms four other machine learning methods (NN, NB, DT, SNoW) for question classification.
 
2. The syntactic structures of questions are really helpful to question classification. 

3. The use of a special kernel function called tree kernel to enable the SVM to take advantage of the syntactic structures of questions.

You can read the original paper [here](https://www.comp.nus.edu.sg/~leews/publications/p31189-zhang.pdf).

Hope you have liked the blog. Happy Learning!

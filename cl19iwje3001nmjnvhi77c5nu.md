## Topic Modeling

As name suggest it is a NLP techniques where we can extract the topic of the discussion from a large text set. It is one of the most useful machine learning technique used widely in various fields like news houses, Political campaign and corporate houses. For example an organization trying to understand their customer complaints from emails and various other input methods like social medias or from complain sites.

### What is a Topic?

A topic is nothing but a collection of dominant keywords that are typical representatives. Just by looking at the keywords, we can identify what the topic is all about.

The following are key factors to obtaining good segregation topics:

1.	The quality of text processing.
2.	The variety of topics the text talks about.
3.	The choice of topic modeling algorithm.
4.	The number of topics fed to the algorithm.
5.	The algorithms tuning parameters.

In this blog we will take a real example of the ‘20 Newsgroups’ dataset and use LDA to extract the naturally discussed topics. Latent Dirichlet Allocation (LDA) from Gensim package along with the Mallet’s implementation (via Gensim). Mallet has an efficient implementation of the LDA. It is known to run faster and gives better topics segregation.

### Prerequisites – Download nltk and spacy model

For text processing we will need stopwords from NLTK’s stopwordand specy’s  en model. We also use Spacy’s lemmatization implementation.

Lemmatization is nothing but converting a word to its root word. For example: the lemma of the word ‘machines’ is ‘machine’. Likewise, ‘walking’ –> ‘walk’, ‘mice’ –> ‘mouse’ and so on.

Most people faces issues while installing spacy. You can refer this [link](https://stackoverflow.com/questions/47295316/importerror-no-module-named-spacy-en) for guidance.

### What does LDA do?

LDA basically take each document and represent it as combination of multiple topics with different proportion and also treat each topic as combination of multiple keywords again with different proportions. When we provide a group of topics to it, LDA rearrange the topic distribution within the document and again rearrange the keys within the topics to obtain a good composition of topic-keywords distribution.

### Implementation Steps

You can find the complete Jupiter Notebook implementation [here](https://github.com/git-chinmay/myML/tree/master/NLP/Topic_Modelling_Using_LDA)

To list out what are the steps it involved to implement a basic LDA based Topic modelling
1.	Prerequisite setup
2.	Dataset import
3.	Text Clean-ups (Data Pre-processing)
4.	Word Tokenization
5.	Perform Stopword removal, Bigram and Lemmatization
6.	Prepare a Text corpus and dictionary with word id and its term frequency
7.	Build the model
8.	Some visualization
9.	Finding the optimal no of Topics

### Importing the Dataset

We will be using the 20-Newsgroups dataset for this exercise. This version of the dataset contains about 11k newsgroups posts from 20 different topics. This is available as [newsgroups.json](https://raw.githubusercontent.com/selva86/datasets/master/newsgroups.json).

![1-Dataset.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648395433492/37G_0Yku_.png)

### Viewing the topics and the keywords form it

The above LDA model is built with 20 different topics where each topic is a combination of keywords and each keyword contributes a certain weightage to the topic.

You can see the keywords for each topic and the weightage(importance) of each keyword using lda_model.print_topics()

![2-topics.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1648395487819/z8gCl17MH.jpg)

**How to interpret this?**

Topic 0 is a represented as 

```
(0, '0.705*"ax" + 0.026*"graphic" + 0.013*"peace" + 0.013*"bomb" + ' '0.012*"convert" + 0.009*"homosexual" + 0.007*"violence" + 0.006*"capture" + '  '0.006*"birth" + 0.005*"gif"')

``` 

It means the top 10 keywords that contribute to this topic are: ‘ax’, ‘graphic’, ‘peace.. and so on and the weight of ‘ax on topic 0 is 0.705.

The weights reflect how important a keyword is to that topic.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648395575850/RGJJdF7Lm.png)

### Visualising Topics & its keywords

Each bubble on the left-hand side plot represents a topic. The larger the bubble, the more prevalent is that topic.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648395617500/SdZNg_W1F.png)

A good topic model will have fairly big, non-overlapping bubbles scattered throughout the chart instead of being clustered in one quadrant.

A model with too many topics, will typically have many overlaps, small sized bubbles clustered in one region of the chart.

We have successfully built a good looking topic model.

Given our prior knowledge of the number of natural topics in the document, finding the best model will be a straightforward task.

### Improvements

So far you have seen Gensim’s inbuilt version of the LDA algorithm. Mallet’s version, however, often gives a better quality of topics. Gensim provides a wrapper to implement Mallet’s LDA from within Gensim itself. You only need to download the zip file, unzip it and provide the path to mallet in the unzipped directory to gensim.models.wrappers.LdaMallet.

### Finding the optimal number of topics for LDA

The simplest way to find the optimal number of topics is to build many LDA models with different values of number of topics (k) and pick the one that gives the highest coherence value. If you see the same keywords being repeated in multiple topics, it’s probably a sign that the ‘k’ is too large.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648395759623/iHMRHzKZz.png)

If the coherence score seems to keep increasing, it may make better sense to pick the model that gave the highest CV before flattening out. 

Here we can see at topic count 20 we are getting highest CV. 

We can further analyze by finding the dominant topic in each sentence and use it for our business decisions.

### Conclusion

We started with understanding what topic modeling can do. We built a basic topic model using Gensim’s LDA and visualize the topics using pyLDAvis. We saw how to find the optimal number of topics using coherence scores and how you can come to a logical understanding of how to choose the optimal model.

You can get the complete code [here](https://github.com/git-chinmay/myML/tree/master/NLP/Topic_Modelling_Using_LDA).
Hope you have enjoyed the blog. Happy learning!!



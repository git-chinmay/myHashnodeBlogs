## Deep NLP for LinkedIn Search Systems

This blog post trying to summarizes the original whitepaper named Deep Natural Language Processing for LinkedIn Search Systems submitted by wguo, xwli, sidwang, mikazi, zfu, hgao, jjia, lizhang, blong at LinkedIn. You can read the whitepaper  [here](https://arxiv.org/pdf/2108.08252.pdf) . All the pictures were taken from the paper.

Many search systems work with large amounts of natural language data, e.g., search queries, user profiles and documents, where deep learning based natural language processing techniques (deep NLP) can be of great help. Search engines are typically complicated ecosystems that contain many components (Figure 1), e.g., named entity recognition, query-document matching, etc.


![Fig1.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1632549606193/AZCKkz_00.png)

Deep learning has shown great success in NLP tasks, indicating its potential in search systems. However, developing and deploying deep NLP models for industry search engines requires considering three challenges: Latency, Robustness & effectiveness. This paper focuses on developing practical solutions to tackle these three challenges. It picked five representative search tasks that cover the classic NLP problems. Fig 2. The representative search tasks reflect five common NLP tasks: classification, ranking, sequence labeling, sequence completion and generation.


![Fig2.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1632549917487/hrqFI6cMT.png)

### Search systems at LinkedIn

LinkedIn provides 7 vertical searches, each corresponding to a document type. people, job, feed, company, group, school, event but in the paper the experiments are conducted on 3 vertical searches (people search, job search, help center search), and federated search. Federated search retrieves the documents from all vertical searches and blends them into the same search result page.

The figure-1 above shows a typical search system and its three typical components.

1. Language Understanding : Constructs important features for other two components.

2. Language generation : Suggest new queries that will more likely lead to desirable search results.

3. Document retrieval & Ranking : Produce final results of the search system

*NOTE: It is worth noting that search datasets are different from classic NLP task datasets, mainly in two aspects: 
(1) Data genre. The data unit of classic NLP is complete sentences with dozens of words, while queries only have several keywords without grammar. 
(2) Training data size. Classic NLP datasets are human annotated, therefore the size is usually around tens of thousands of sentences, while search tasks have millions of training examples derived from click-through search logs.*

### Search Tasks

**Query Intent Prediction**

It predicts the probability of the user’s intent towards the several verticals. The predicted intent is an important feature leveraged by downstream tasks such as search result blending  to rank higher the documents from relevant vertical searches. CNN is used for the query intent as its performance gain is very high in terms of text classifications. The team at LinkedIn combines the extracted text embedding with handcrafted features, and uses a hidden layer to enable feature nonlinearity.

Paper uses 24M queries for training, and 50K for dev and test each. The CNN model is also compared with a bidirectional LSTM where CNN has 128 filters of window size 3 (tri-grams) & LSTM has hidden state size of 128.


![Table1.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1632550354841/gLv2idYMR.png)


Table-1 shows the offline relevance & latency performance where both CNN and LSTM outperforms the baseline model(Already used Production Model). For online experiments the paper chooses CNN over LSTM as it is faster and gives almost the same performance. (Table-2)


![Table2.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1632550396752/xIoMfh2lG.png)


**Query Tagging**

Its goal is to identify the named entities in the query. Here it's more interested in 7 types of entities: first name, last name, company name, school name, geolocation, title, and skill. This will help in constructing many useful features for query intent prediction or search rankings. The production model uses three categories of features: character feature, word feature and lexicon features. As the team was able to extract powerful lexicon features they used semi markov conditional random field (SCRF) to train the production model. 

Manually annotated Queries from LinkedIn federated search are used for training(100K),  5K for dev and test each. The experiment result shows the SCRF with lexicon achieved the best results as shown in Table-3.


![Table3.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1632550499263/qNMYzezfL.png)

As there are no significant offline experiment gain models were not deployed online.

**Query Auto Completion**

It is a language generation task. The input is a prefix typed by the user and output is a series of user queries that best describes the user intent. The auto completion process has two steps 1) candidate generation 2) Candidate Ranking. The LinkedIn production system uses heuristic model for generation and XGBoost model for rankings. The paper uses an unnormalized language model because although the normalized language model was giving good result it was computationally heavy with high latency.


![Table4-5.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1632550575219/Ra6jiVYGd.png)


Table- 4 & 5 shows the offline and online results of the model. We can see that the latency is reduced drastically in case of an unnormalized language model.

**Query Suggestion**

This is similar to Google’s “Search related to”. LinkedIn shows “People also search for..”.  Production model suggests based on the two things, for each input query 

1. The input and suggest query must be in same sessions (they are not more than 10mins apart)
2. Both the queries must contain a common word.

Paper uses a small (2 layer LSTM with 100 hidden size) Seq-to-Seq model for query suggestion.


![Table6-7.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1632550706213/PCCHai5-Q.png)

Offline experiments in Table 6 demonstrate that the seq2seq can find better candidates, measured by MRR@10 and the online experiments (Table 7) display evidence of an enhanced user experience, particularly on finding jobs where seq2seq can generate in-domain terminology for novice users.


**Document Ranking**

Given a query, a searcher profile and a set of retrieved documents, the goal of document ranking is to assign a relevance score to each document and generate the ranking. The production model uses XGBoost for training. Paper uses Neural Network (CNN) for higher performance by combining the text embeddings with handcrafted features along with a hidden layer to add non linearity similar to  Query Intent task.


![Table9.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1632550880329/sWD6k7Apa.png)


![Table10.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1632550916752/NVBUmogiU.png)

Table-9 and 10 shows the offline and online model performance. In summary, the offline and online performance are consistent. The new model CNN-ranking significantly outperforms the baseline by a large margin.

### When is Deep NLP helpful ?

The paper also highlights that,  the deep NLP models achieve better relevance performance than traditional methods in search tasks, and are particularly powerful in the following scenarios:

1. Language generation tasks
2. Data with rich paraphrasing

### When is Deep NLP not helpful ?

No gain is observed when applying LSTM-SCRF for query tagging tasks. It reveals an important difference between industry and academic data – huge amounts of user generated data. Hence, the SCRF approach can do well with a lexicon built.

### Latency is the biggest challenge

It has been observed that latency is the biggest problem applying NLP to production search system so the paper also highlights few ways to reduce it

- Algorithm design
- Parallel computing
- Embedding precomputing
- Two pass ranking

### Ensuring Robustness

Robustness is an essential part of the search system as it directly affects the user experience. As deep learning models very easily overfit the paper tries to overcome it by manipulating training data(removing generalization data) & reusing handcrafted features.


This paper focuses on how to apply deep NLP models to five representative search productions, illuminating the challenges along the way. All resulting models except query tagging are deployed in LinkedIn’s search engines. So today if you go and search something on LinkedIn keep in mind that these models are working behind it. Hope you have liked the summarization of the paper. You can read the original paper  [here](https://arxiv.org/pdf/2108.08252.pdf) 

Happy learning !!!



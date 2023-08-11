# Retrieval and Rerank
SentenceTransformers are used to generate embeddings for queries, sentences, and paragraphs, in the context of semantic search.

In scenarios involving intricate search requirements, such as question-answering retrieval, enhanced search performance can be achieved through the implementation of the Retrieve & Re-Rank strategy.

## Retrieve & Re-Rank Pipeline

![image](https://github.com/ArunVigneshFAI/Retrieval_and_Rerank/assets/141916176/c4c249ad-2848-49dd-b831-5afda1c75359)

In the given approach, a search query initiates a two-stage process. Initially, a retrieval system is employed, capable of procuring a substantial list of potential matches (e.g. 100 potential matches) deemed relevant to the query. The retrieval process can encompass lexical search methods like ElasticSearch, or it can integrate dense retrieval strategies utilizing a bi-encoder architecture.

However, it's acknowledged that the retrieved documents might encompass items of varying relevance to the search query. Consequently, the second phase involves the application of a re-ranker built upon a cross-encoder framework. This re-ranker computes relevance scores for all candidates in relation to the provided search query.

The ultimate output comprises a meticulously ranked list of hits, poised to be presented to the user, thus enhancing the efficiency and precision of the search process.

## Retrieval: Bi-Encoder
For the retrieval of the candidate set, we can use a bi-encoder which is implemented in this repository.

![image](https://github.com/ArunVigneshFAI/Retrieval_and_Rerank/assets/141916176/36fe8b17-fdf8-494f-8970-d8a546db2ddc)

Semantic search, also known as dense retrieval, involves the conversion of a search query into a vector space representation, enabling the retrieval of document embeddings situated in proximity within the vector space. This method effectively addresses the limitations of lexical search, allowing for the recognition of synonyms, acronyms, and more nuanced relationships between terms.

![image](https://github.com/ArunVigneshFAI/Retrieval_and_Rerank/assets/141916176/1e1dc0ad-2a95-487a-9487-06d8a0985545)

Within the realm of natural language processing tasks, especially those related to text matching, retrieval, and ranking, the Bi-Encoder architecture is frequently employed. 

Bi-Encoders are neural network architectures designed to generate sentence embeddings, often utilized in various NLP applications. In this setup, two sentences, denoted as A and B, are independently processed through a BERT model, yielding the respective sentence embeddings, u and v. The cosine similarity is subsequently employed to gauge the similarity between these resulting sentence embeddings.

## Re-Ranker: Cross-Encoder
The retriever component must ensure efficiency when dealing with vast document collections, often consisting of millions of entries. However, it might inadvertently return candidates that are not directly relevant to the search query.

![image](https://github.com/ArunVigneshFAI/Retrieval_and_Rerank/assets/141916176/f22a3262-5c5c-43ed-89df-65e56472fa9b)

The retriever component must ensure efficiency when dealing with vast document collections, often consisting of millions of entries. However, it might inadvertently return candidates that are not directly relevant to the search query.

A considerable enhancement to the end results for users can be achieved by incorporating a re-ranker based on a Cross-Encoder.

Cross-Encoder operates by simultaneously inputting both sentences—the query and the document—into a Transformer network. The outcome is a numerical value ranging between 0 and 1, denoting the document's relevance to the given query.

Notably, a Cross-Encoder doesn't generate sentence embeddings, and it doesn't permit the independent input of individual sentences. The prime advantage of Cross-Encoders is their superior performance, attributed to their capability to attentively process both the query and the document.

Calculating scores for numerous (query, document) pairs, which can extend into the thousands or even millions, might be considerably time-consuming. To mitigate this, the retriever function is employed to initially generate a smaller set, perhaps around 100 potential candidates. This subset is then subject to re-ranking by the Cross-Encoder, effectively combining efficiency and performance in the search process.

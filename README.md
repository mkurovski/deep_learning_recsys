# deep_learning_recsys
Deep Learning for Recommender Systems - Master Thesis, Current Results and Architecture

For details see also [my blogpost on Medium](https://ebaytech.berlin/deep-learning-for-recommender-systems-48c786a20e1a)

# Deep Learning for Recommender Systems
This repository contains the proof-of-concept for a Recommender System that learns
user and item (vehicle) representations in a nonlinear fashion using Deep Learning.
It was proven that it beats traditional matrix factorization for collaborative filtering
and collaborative-content-based (hybrid) filtering by a large margin. The evaluation was
conducted offline on an experimental subset of 100'000 subscribed users and roughly 1.7 million
items.

## Proof-of-Concept
The PoC was established in the master thesis "Deep Learning for Recommender Systems:
Joint Learning of Similarity and Preference" which is linked here.
The diagram below summarizes the newest results which beat the preliminary results shown in the thesis. It shows the mean average precision (MAP) across different k (number of provided recommendations). There are different techniques compared with each other: collaborative filtering (CF) and hybrid collaborative-content-based filtering (CF-CBF) as well as a Deep Learning solution that trains for two objectives:
* Predict the probability of a user to prefer a specific item (vehicle)
* Create dense user/item representations that can be used for an efficient candidate generation using a novel regularization technique
![Results of the Study on a Deep Learning based Recommender System](img/results.png?raw=true "DL Recommender PoC Results")

## Big Picture of the DLRS prototype
The prototype is based on a trained deep neural network that is split into three components:
* UserNet: Network to generate a dense user representation from sparse user features
* ItemNet: Network to generate a dense item representation from sparse item features
* RankNet: Network to estimate a probability based on dense user and item representations

The Big Picture also illustrates the two-step process for generating recommendations:
1. **Candidate Generation**: Reduce item corpus (all items) to a few potentially relevant items (candidates) using simpler, but faster models (focus on recall).
For the first step approximate nearest neighbor search on embeddings turns out to be efficient and provide good results:
* [Approximate Nearest Neighbors Oh Yeah (ANNOY)](https://github.com/spotify/annoy)
* [Locally Optimized Product Quantization (LOPQ)](https://github.com/yahoo/lopq)
* KMeans Clustering
* Minimal Euclidean Distances

2. **Ranking**: (Re-)Rank the candidates using a more accurate, but computationally more expensive model (focus on precision).
This second step uses the RankNet only based on a given user representation and the provided candidate representations.

![](img/networks.png?raw=true "DL Network Composition")

## Architecture for Production

![](img/architecture.png?raw=true "DL Recommender Architecture")

## Outlook
* Apply work to RecSys Challenge 2017
* Apply work to RecSys Challenge 2018
* For more information, visit my Medium blogpost in the ebay Tech Berlin

> We can only see a short distance ahead, but we can see plenty there that needs to be done. (Alan Turing)

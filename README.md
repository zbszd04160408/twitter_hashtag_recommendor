# Twitter Hashtag Recommendor

## Overview

### Backgroud
On Twitter, adding a '#' to the beging of a word or a phrases creats a hashtag. When people click on the hastags, you can jump into the a topic with a list of tweets related to this topic. Hashtags on Twitter helps people easily follow topics that they are interested. 

### Problem
When people sending out a tweet and we want to add a hashtag to the tweets, it pumped hashtag list is neither related to the context of tweet very much, nor sorted from trending list. 

### Solution
To solve this problem, I trained a BERT model and utilize the fill-mask task to solve this problem.
Some special modifications I made to the model are as follows:
1. Hashtags are usually consists of multiple words without space. When tokenizing these words, it will be splitted into different words and therefore cannot form up an existed hashtags. Therefore, I added the top 1000 trending hashtags to the token dictioanry and provide special token_ids for each hastags. 
2. When masking the tweets during training, I intentionally mask the tokens that is a hastag, so that the model will learn to predict the place of hashtags specifically.
3. After training the model, we limit the potential candidates of [MASK] with the topics existed on Twitter, to get the relavent hashtags the user can add on. 

### Result
**Twitter current approach:**

<!-- ![f7b41c675776eacc0a638f1517c8fb3](https://user-images.githubusercontent.com/56851668/163923465-a0ac8c4b-a6f1-4553-bc70-aff6c8365ef5.jpg) -->
<img src="https://user-images.githubusercontent.com/56851668/163923465-a0ac8c4b-a6f1-4553-bc70-aff6c8365ef5.jpg" width="500">


**Our approach:**

![image](https://user-images.githubusercontent.com/56851668/163923371-97311812-d6e3-4b47-a277-d5112842198b.png)

**Original tweet:**

![image](https://user-images.githubusercontent.com/56851668/163923602-f7710650-69fd-4313-a653-94f45fae8d81.png)


## Huggingface Space
Huggingface space is [here](https://huggingface.co/spaces/vivianhuang88/hashtag_rec).

## Huggingface Model Card
Huggingface model card is [here](https://huggingface.co/vivianhuang88/bert_twitter_hashtag/tree/main).

## Critical Analysis
1. For efficiency consideration, we only added the top 1000 hashtag topics to our vocab dictionary. However, the actual number of potential hashtags should be millions. If adding all the hashtags to the dictionary, the efficiency of this approach will grow expoenentially. But not including these hashtags might also decrease toe candidate hashtags for users to choose. 
2. Training data is small as well. The size of training data is about 30k. 
3. Future modifications on this model might be add weights on different topics. For example, more recent topics will be weighted higher than older topics.


## Resource Links

[Huggingface tutorial](https://huggingface.co/course/chapter7/3?fw=pt#using-our-finetuned-model)

[Huggingface twitter-roberta-base](https://huggingface.co/cardiffnlp/twitter-roberta-base)

[Huggingface bert-base-uncased](https://huggingface.co/bert-base-uncased)

## Code Demo

## Repo
In this repo

## Video Recording

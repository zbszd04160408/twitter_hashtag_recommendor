# Twitter Hashtag Recommender

## Overview

### Backgroud
On Twitter, adding a '#' to the beging of a word or a phrases creats a hashtag. When people click on the hastags, you can jump into the a topic with a list of tweets related to this topic. Hashtags on Twitter helps people easily follow topics that they are interested. 

The benefits of adding a hashtag to a tweet includes the follows:
1. Have a higher chance to show your post to other people, especially when you added a trending hashtag.
2. Get to know more friends that shares the same interest with you.
3. Make your the topic you are interested hotter and more people get to know it. 

### Problem
When people sending out a tweet and we want to add a hashtag to the tweets, Twitter does not give a good suggestions on the potentail hashtags they would like to add.

On webpage, Twitter do not provide any potential suggestions when you type "#".

On your phone, it will pump a list of hashtags. But the relevance between the list of hashtags and the tweets is not very high, and the hashtags are not sorted from trending hashtag. 


### Solution
To gain the trianing data, I scraped from the trending topics from Twitter, filtered out the tweets in different languages and left with only tweets in English. There are more than 30k tweets in our training dataset.


Then, a BERT model is trained to do the fill-mask task to solve this problem.
Some special modifications I made to the model are as follows:

1. Hashtags are usually consists of multiple words without space. When tokenizing these hashtags, they will be splitted into different words and therefore cannot form up an hashtag when we decode these tokens. For example, "#TheFirstLady" passed in regular tokenizer will be splitted into "#", "The", "First", "#Lady". Therefore, I added the top 1000 trending hashtags to the token dictioanry and provide special token_ids for each hastag. 

2. When masking the tweets during training, I intentionally mask the tokens that is a hastag, so that the model will learn to predict the place of hashtags specifically.

3. After training the model, we limit the potential candidates of [MASK] with the topics existed on Twitter, to get the relavent hashtags the user can add on. 

### Result
**Twitter current approach:**

<!-- ![f7b41c675776eacc0a638f1517c8fb3](https://user-images.githubusercontent.com/56851668/163923465-a0ac8c4b-a6f1-4553-bc70-aff6c8365ef5.jpg) -->
<img src="https://user-images.githubusercontent.com/56851668/163923465-a0ac8c4b-a6f1-4553-bc70-aff6c8365ef5.jpg" width="500">


**Our approach:**

![image](https://user-images.githubusercontent.com/56851668/163928564-029eff54-e89a-433d-9993-6ed6f60f4950.png)

**Original tweet:**

![image](https://user-images.githubusercontent.com/56851668/163923602-f7710650-69fd-4313-a653-94f45fae8d81.png)


## Huggingface Space
Huggingface space is [here](https://huggingface.co/spaces/vivianhuang88/hashtag_rec).

## Huggingface Model Card
Huggingface model card is [here](https://huggingface.co/vivianhuang88/bert_twitter_hashtag/tree/main).

## Critical Analysis
1. For efficiency consideration, we only added the top 1000 hashtag topics to our vocab dictionary. However, the actual number of potential hashtags should be millions. If adding all the hashtags to the dictionary, the efficiency of this approach will drop expoenentially. But not including these hashtags might also decrease the candidate hashtags for users to choose. In addition to this, topics might be unavailable to be predicted if it is not trained in our model. 
2. Training data is small as well. The size of training data is about 30k. 
3. Predicting tags only depend on context of tweet might not be enough, as people usually provide some information as well in the attached images. Getting information from images could also be an idea. 
4. Future modifications on this model might be add weights on different topics. For example, more recent topics will be weighted higher than older topics.


## Resource Links

[Huggingface tutorial](https://huggingface.co/course/chapter7/3?fw=pt#using-our-finetuned-model)

[Huggingface twitter-roberta-base](https://huggingface.co/cardiffnlp/twitter-roberta-base)

[Huggingface bert-base-uncased](https://huggingface.co/bert-base-uncased)

## Code Demo

[Code Demo](https://github.com/zbszd04160408/twitter_hashtag_recommendor/blob/main/30-ModelWalkthrough.ipynb) is inside this repo

## Repo
In this repo

## Video Recording
https://youtu.be/EC18mZBy1Jo

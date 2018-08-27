---
published: true
layout: post
title: Twitter Auto-completer for Members of Congress
---
This project [(Link to Github Repo)](https://github.com/aakashpydi/tweet_completer_congress) is a tweet auto-completer for members of Congress. I used the Twitter API & the DocNow hydrator to create a custom dataset. I then used the GenSim library to generate a custom word2vec representation and finally used a Keras LSTM model to auto-complete tweets.

I used subsets of the following datasets from George Washington University stored on Harvard Dataverse:
- [115th US Congress Tweet Ids](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/UIVHQR)
- [U.S. Government Tweet Ids](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/2N3HHD)
-  [2016 United States Presidential Election Tweet Ids](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/PDI7IN)

### Word2Vec Embeddings

Sample output of top ten closest words to the word 'simple' in the generated embedded space.
![Simple Top Ten]({{site.baseurl}}/images/tweet_autocomplete_images/simple_top_ten_matches.JPG)

Two dimensional representation of the embedded space (dataset of ~50,000 tweets).
![Word2Vec Embeddings Small]({{site.baseurl}}/images/tweet_autocomplete_images/embeddings_small.JPG)

Two dimensional representation of the embedded space (dataset of ~1.5 million tweets)
![Word2Vec Embeddings Large]({{site.baseurl}}/images/tweet_autocomplete_images/embeddings_large.JPG)

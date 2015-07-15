Insight Data Engineering - Coding Challenge
===========================================================  

## Challenge Summary

This challenge is to implement two features:

1. Calculate the total number of times each word has been tweeted.
2. Calculate the median number of *unique* words per tweet, and update this median as tweets come in. 

1. For this feature I split the input into several shards based on alphabetical order. This allows for parallel processes to work on different sections of the initial large input.
The number of processes to spawn can depend on the size the input file. The splitting of several outputs in intermediate stages helps fault tolerance. If one of the processes (before the 
combine stage) fails it can simply be executed once more and the output would still be the same. 
Additionally since the input is random, I chose to go with python's default sorting implementation tim sort, which has a O(nlog(n)) complexity. By sharding data, n is reduced by a factor equal to the number of shards present and each shard is executed in parallel. Additionally this also improves in-memory processing as we now require smaller chunks of data to be loaded into memory as opposed to the entire input.

2. The second feature has streaming input. The traditional way of computing median is to sort the input and then count the value that splits the data equally. However considering the characteristics of our input, the maximum number of unique words in a tweet is 140 due to twitter's limit on character size. We can use this to our advantage and only use the frequencies of incoming data and store them in a hashtable instead of using a large list. 

 


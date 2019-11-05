+++
title = "Search Engine with Python"
+++

**<h2>Query processing and calculating similarity</h2>**

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
Finally we use the input box we created in our application's search page to retrive user search query and make it undergo the same 
pre-processing as our reviews, converting to lower case, stop word removal and lemmatization.
We calculate the weights of words in the query using only term frequency. Now we retrieve the **top 10** reviews from the posting lists of each word in the search query. 
If a review appears in the top-10 elements of every query word, calculate cosine similarity score. 

$$ {sim(q,r)} = \\vec{q} \\cdot \\vec{d} = \\sum_{t\ \\text{in both q and r}} w\_{t,q} \\times w\_{t,r}.$$

Where **'q'** is the search query and **'r'** is a review vector. If  a review doesn't appear in the top-k elements of some query words, use the weight in the 10th element as the upper-bound on weight in vector. Hence, we can calculate the upper-bound score for using the query word's actual and upper-bound weights with respect to vector, as follows. 

$$ \\overline{sim(q,r)} = \\sum_{t\\in T_1} w\_{t,q} \\times w\_{t,r}.$$

$$  + \sum_{t\\in T_2} w\_{t,q} \\times \\overline{w\_{t,r}}.$$


In the above equation, first part has query words whose top-k elements contain review. Second part includes query words whose top-k elements do not contain the review. The weight in the 10-th element of word's postings list is used here. 

$$ \\overline{sim(q,r)} = \\sum_{t\\in q} w\_{t,q} \\times \\overline{w\_{t,r}}.$$

* We choose k as 20, this acts as a hyperparameter forthe search. Set k to be 20 as trial and error shows that this is optimal value.

* Let query be "My stomach hurts", After preprocessing the the vector representation will be ['stomach','hurt']

* Below we show how to algorithm calculate similarity of query.
<pre>
  word-weight-query = number of times word appears in the query / number of words in query


  1. retrive top 20 of posting list for each word in query
  2. find the list of all reviews. 
  3. for every review:
  4.   for every word:
  5.     if word exsists in the review:
  6.        score += word-weight-review * word-weight-query 
  7.     else:
  8.        score += word-weight-review20 * word-weight-query

</pre>

<h2> Challenges faced </h2>

1.  Implementing the count of words in each word. Used the concept of positional indexing.
2.  Getting the posting list for each word. It was computationally expensive to sort every posting list and keep them stored prior to the query similarity calculation. Sort only the posting list being retrived while calculating the similarity.

<h3>Inverted Index vs Term Document Matrix</h3>
The term document matrix was generated with gensim library
* The retrive time for a search result was abot 10 seconds.
The custom Inverted Index yields results dractically faster
* The query retrive time was about 0.05 seconds

<h4> Contributions: </h4>

1. Implemented my own Inverted index from scratch using numpy and pandas.

2. Created A way to quickly retrive the posting lists for similarity calculation.

3. Improving efficiency by dropping words that appear too many times from the vocabulary.

4. Used **nltk's** **_WordNetLemmatizer_** for feature selection.


<h5> Referrences</h5>

*  (https://nlp.stanford.edu/IR-book/html/htmledition/a-first-take-at-building-an-inverted-index-1.html)

* [Sample code given](https://colab.research.google.com/drive/1n1hUx-mO4EqhKyFmN--9pK5MhKR68MpB#scrollTo=8ILVjili5Xmu)

</b>
Continue reading to implement Multinomial Naive Bayes classifier.

[Part 2](https://sananara-aryabhata.netlify.com/post/naive-bayes-classifier/)

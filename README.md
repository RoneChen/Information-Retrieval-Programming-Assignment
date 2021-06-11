# README

## Introduction

"small.py" successfully runs on the small corpus, and the "large.py" runs on the large corpus with some modifications.

Both of them supports manual query and evaluation.

## Directory Structure

```
├─COMP3009J-2020-corpus-large
│  ├─large.py
│  ├─README.md
│  ├─documents
│  ├─files
│  |   ├─porter.py
│  |   ├─stopwords.txt
│  |   ├─quries.txt
|  |   ├─qrels.xtx
│  |   └─__pycache__
|  └─__pycache__
└─COMP3009J-2020-corpus-small
	├─small.py
	├─README.md
    ├─documents
    ├─files
    │  ├─porter.py
    │  ├─stopwords.txt
    │  ├─quries.txt
    |  ├─qrels.xtx
    │  └─__pycache__
    └─__pycache__
```

## Operations

1. Copy the small.py and large.py to the directory presented above.

2. On Windows,

   - For manual query

     When you firstly run the program, it needs to creating the index. Please wait.

   ```
   python small.py -m manual
   ```

   ```
   python large.py -m manual
   ```

   - For evaluation

     When you firstly run the program, it needs to creating the index. Please wait.

   ```
   python small.py -m evaluation
   ```

   ```
   python large.py -m evaluation
   ```

## V 1.1

![v1](C:\Users\m1355\Desktop\Final\readme\v1.png)

​    V 1.1 just finishes the functions of the BM25 calculation of each query without any optimization. Firstly, the program reads in all documents, in each cycle, it removes stop words and stems each token. Then, create a file called index.txt for future use, which follows {document_id : [token1, token2, token3…]}. After that, calculate all terms frequencies and save them in freq.txt. When a query arrives, the system should get ni for each token in the query and calculate the BM25 similarity for all documents.

​	Indexing costs around **142s**, each query costs around **7s** (i7-8550U).

## V 1.2

![v2](C:\Users\m1355\Desktop\Final\readme\v2.png)

​	This version optimizes the algorithm efficiency. I calculated term frequency and ni while reading documents. In other words, when the program creates the index, it calculates the term frequency and ni at the same time and stores them in freq.txt and ni.txt. When a query arrives, the system should follow the BM25 formula and get similarities for all documents by reading the term frequency and the number of documents where the term exists of each token that exists both in the query and the current document from files. Thus, when the system calculates the formula, it only needs to perform multiplications and adding. This significantly increases the speed.

​	Indexing cost around **2.32s**, each query costs around **0.13s** (i7-8550U).

## V 2.0

This version tried to optimized the precision and recall.

There are two different strategies,

- Static return

  In this case, the number of return documents is static. In other words, it is predefined and is a fix value.

- Dynamic Return 

  The number of return documents is dynamic, it changes with the number of  returned documents in "qrels.txt".

### On Small Corpus

#### Static Strategy [1,20)

In this experiment, the return documents number is from 1 to 19, and the result is presented in the graph. It is easy to find that when the search engine returns 7 documents, it can reach a relatively better performance on all metrics, while except P@10 and recall, the other 4 metrics keep increasing.

![image-20210611162535879](C:\Users\m1355\AppData\Roaming\Typora\typora-user-images\image-20210611162535879.png)

#### Dynamic Strategy [-1, 20)

In this experiment, the return documents number is from 1 less than the results provided by "qrels.txt" to the query to 19 more. The result shows that when the search engine returns the same number of documents as "qrels.txt" or one more, it can achieve relatively better performance (around 0.32). 

Compared with the static strategy, the dynamic version provides a better result. As a return of the same number of documents might leads to precision and recall be the same, the system should return **one more documents**.

![image-20210611161933691](C:\Users\m1355\AppData\Roaming\Typora\typora-user-images\image-20210611161933691.png)

### On Large Corpus

#### Static Strategy [1,20)

Having finished the same experiment as in the small corpus, when we return 10 documents both precision and P@10 present a good result. However, other metrics below 0.15.  

![image-20210611130112899](C:\Users\m1355\AppData\Roaming\Typora\typora-user-images\image-20210611130112899.png)

#### Dynamic Strategy [-5,20)

In the dynamic strategy, the result is quite different. It is easy to find that when the system returns 1 fewer documents, recall drops a lot. Compared with the static version, the average score of all 6 metrics is higher; thus, in the large corpus, the dynamic strategy is used, too. Having analyzed the result, the search engine should return **2 fewer documents** for a query than that in the standard results.

![image-20210611130124794](C:\Users\m1355\AppData\Roaming\Typora\typora-user-images\image-20210611130124794.png)

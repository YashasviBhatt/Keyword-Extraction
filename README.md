# Keyword Extraction

**Keywords** plays very important role for an Organization to run their business smoothly and with good revenue generation. They tell us the **intent** behind the mail sent by the customer regarding the product/service we are offering. Not only in emails, it can be used in several fields as well. But finding keywords is not an easy task, it takes a lot of time especially when you are working with a large firm and they receive thousands of emails daily. So, how to do it, obviously we can't just simply ignore these important mail keywords since they are very important for an Organization, but then processing them is a hard task as well, for a human it will take a complete month to process such amount of mails and we don't have that much time and even if we have, we can't waste that much amount of time.

So, what to do then? Well, the answer is **Technology**. Technologies like **Machine Learning** and **Natural Language Processing (NLP)** helps us do this as well as similar kinds of tasks with minimum human effort, minimum time and with great ease. Natural Language Processing is a powerful technology that can process huge amount of data in comparitively very less time. In just an hour or so, it can process datasets ranging from 500MBs to 1.5GBs in size (even more, completely depends on the system used).

## Working

Keyword Extraction, as the name suggests is a project built to extract keywords from _Enron Emails Dataset_ provided by _Cognitive Assistant that Learns and Organizes (CALO Project)_. This dataset contains data from about 150 users, mostly senior management of Enron. The corpus contains a total of about **0.5M (~517K)** messages in a dataset of around 1.3GBs in size.

To be honest, my system wasn't able to handle this much amount of data, not even the limited free version of Google Colaboratory was able to handle it, so I had to divide the dataset into 50 different parts such that each sub-dataset contains approximately 10340 records. Therefore, the analysis you are seeing in this project is done on a sub-dataset of exactly 10348 records. One can do processing on whole dataset but for that he/she needs to apply _Batch Processing_ which will easily take 2-3 Days to process this much amount of data and as mentioned "Time is Money" so I am not going to do that.

The Dataset have 2 columns, first column is the _Files_ column that tells us the extra information regarding the mail like file-name in which the mail was originally kept, the folder name in which that respective file was stored and etc. The second column _Mail_ is actually the column of our concern or need which actually tells us about Mail, Mail Body, Sender Details, Recepient Details, Time and Date Constraints etc.

Since, everything in the Mail Column was stored as string so in the First Step I had to apply some advance **RegEx to extract actual Mail Content** from the string which in itselves was a 3-step process. Now, in the Second Step I had to apply some **Natural Language Pre-processing** to clean extracted, but still raw, data. For that I have generated **Word Tokens** for each mail in Dataset and applied **Lemmatization** on them and checked whether they contains **stop words** or not. If they do then these stop words will be removed and rest will be taken for further processing. At last, a corpus was built containing the words that actually had their significance in our objective.

Now, the data is clean, so, what's next. As we know that we are doing Natural Language Processing which uses Machine Learning in behind and since, Machine Learning can't work on String Datasets, it only works on Numerical Datasets, thus, we need to convert these mails to Numbers or more specifically **Word Vectors**. For that, I used the concept of **Bag of Words Model**. The Bag of Words mechanism converts the sentences into word vectors based on the appearance of a word in that respective sentence. The Bag of Words is nothing but a _Numpy N-Dimensional Array_.

---
## Example of Bag of Words Mechanism

```
For following 6 Sentences:
S1 -> A CAT SAT ON THE MAT.
S2 -> THE DOG CHASED THE CAT.
S3 -> CAT CHASED THE MOUSE.
S4 -> CAT ATE THE MOUSE.
S5 -> DOG WORE A HAT.
S6 -> MOUSE ATE HAT AND MAT.
```

The Bag of Words will look like this

| S | CAT | SAT | MAT | DOG | CHASED | ATE | MOUSE | WORE | HAT
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| S1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| S2 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | 0 | 0 |
| S3 | 1 | 0 | 0 | 0 | 1 | 0 | 1 | 0 | 0 |
| S4 | 1 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 0 |
| S5 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 1 |
| S6 | 0 | 0 | 1 | 0 | 0 | 1 | 1 | 0 | 1 |

From this table, we can see that we are reaching nearer to the goal after each step, so the next step would be counting the frequency of each word with the help of word vectors, How? Summing the Column Values ofcourse and we are done.

### Final Result will be

| CAT | MOUSE | MAT | DOG | CHASED | ATE | HAT | SAT | WORE
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 4 | 3 | 2 | 2 | 2 | 2 | 2 | 1 | 1 |

Thus for our example, number of Keywords are 9 with CAT being the highest occurred Keyword and WORE being the least occurred Keyword.

---

In the project, you can see the Bar Plot which I have plotted to display top 20 majorly occurred **Unigrams**, **Bigrams** and **Trigrams** in the Dataset.

## PS

Now several people might still be having doubts, that what to do with the keywords generated.

We can find the nature of recently received emails. Let's say the analysis on recent emails says that the majorly occurred keywords were like this, _Interest_, _Happy_, _Good Product_, _Enjoy_, _Fun_ etc therefore we can say that for current sales the product/service is putting good impact on the customers and thus we can record the constraints that act as base behind those responses and kept using them and even modify them such that our sales go even higher. Also we can forecast the sales based on those constraints as well. 

It could be used in reverse as well, if negative keywords are observed that means there're some faults in our product/service so we'll try to rectify them or change them so as to make it better such that the upcoming sales does not fail us.

Use the `requirements.txt` file to see all the Python Modules and Libraries used. To install these libraries on your local system use `pip install -r requirements.txt`. You can download dataset and get insights on dataset from following links:
- [Enron Email Dataset Download](https://www.kaggle.com/wcukierski/enron-email-dataset/download)
- [Enron Email Dataset](https://www.cs.cmu.edu/~enron/)
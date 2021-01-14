# FOMCMinutesAnalysis
Uses FOMC minutes and leading economic indicators to predict federal funds rate changes.

# 1.	Introduction

The Federal Open Market Committee (FOMC) is the rate setting body of the Federal Reserve. Its main responsibility is the determination of the target federal funds rate, which in large part determines interest rates throughout the financial system. Throughout its history, detailed minutes have been taken, and, in recent years, these minutes have been designed for and released to public consumption. My goal for this project is to use the information contained within these texts, in conjunction with leading economic indicators, in order to predict changes in the federal funds rate in both the time periods immediately following and *at least 30 days after the rate setting meeting.

# 2.	The Problem
Predict interest rate changes at periods t+1 and t+2 using FOMC minutes released at period t and prior.

# 3.	Methodology
## 3.1.	Collecting the data

Fortunately, the FOMC minutes are relatively accessible, collected in the website of the Federal Reserve of St. Louis. Unfortunately, there is no easy way to download them in bulk, so I had to design a program that would traverse the website and scrape the relevant minutes in pdf format using Beautiful Soup. This ended up with around 1000 documents, going back to the 1920s. 
The federal funds rate data is readily available for download back to 1954 from St. Louis Federal Reserve’s FRED website. 

## 3.2.	Cleaning the texts

To prepare the texts for analysis, I removed the stop words and lemmatized the text. This method of preparation was primarily appropriate for analysis using TF-IDF preprocessing, rather than a pre-trained tool such as BERT. 

## 3.3.	NLP Modeling Approach

In addition to a simple random forest classification model, I wanted to attempt a long short term memory model in order to capture the effect of changes in sentiment over time. For instance, saying that the economy is running hot has a very different effect on interest depending on whether this implies a rebound from low growth or a continuation of a long-running expansion. In addition, this method should capture more information resulting from the policy known as “forward guidance”, which was adopted by the Fed in the wake of the financial crisis in order to influence markets further into the future. Therefore, I approached the problem with four different methods:

1.	One period TF-IDF model: I transformed the cleaned texts using the TF-IDF  methodology and trained a random forest classification model based on federal funds rate movements immediately following the meeting. 
2.	Two period TF-IDF model: Using the same specification as the TF-IDF model, I trained the random forest classification model to predict interest rate changes after the following FOMC meeting
3.	One period LSTM model: Using a recurrent neural network with 2 hidden layers, I trained a classification model to predict interest rate changes in the next period
4.	Two period LSTM model: Using the same architecture as above, I trained the model to predict interest rate changes following the next FOMC meeting. 

Finally, because the intended audience for the federal reserve minutes was changed in 1993, I decided to conduct the post-1993 analysis on its own, in addition to using all post 1954 documents.

# 4.	Results
After finding the accuracy ratings of each of my models on the training and test set, I noted the following observations: 
•	Using all the data, going back to 1954, I was only able to achieve a test accuracy of .72, predicting interest changes one period out using the random forest classifier with the TF-IDF model. 
•	However, this was vastly outperformed by the model using minutes from only after 1993, which had an accuracy of .92, clearly showing that the increased efforts at transparency in the 90s had a quantifiable impact. 
•	Predicting further out in the future was tougher, as you might expect. I was only able to achieve an accuracy of .65 using the random forest TF-IDF model for a distance of one meeting period away. This is in comparison to an implied accuracy of around .72 from futures markets.  


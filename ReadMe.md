# Window-based Stream Data Analytics with SPARK and Kafka

A system for real-time news stream classification, trend analysis and visualization. For such implementation, I worked with Apache Spark and Apache Kafka, implemented some of the state-of-the-art classifiers like SVM by using MLlib, and utilized Spark Streaming. I extracted features and built a classifier over a stream of news articles. For the task of gathering real-time news articles, I used stream tools provided by Guardian API.
https://open-platform.theguardian.com/access/

Stream_producer.py generates the Kafka streaming data from Guardian API every 1 second. The topic created has been named 'guardian2'. When running this from your command prompt window, you should be able to see the news getting printed on the screen in the following format:  
```
Label index (news category) || headline + bodyText
```
Each news article will be tagged with one category, such as Australia news, US news, Football, World news, Sport, Television & radio, Environment, Science, Media, News, Opinion, Politics, Business, UK news, Society, Life and style, Inequality, Art and design, Books, Stage, Film, Music, Global, Food, Culture, Community, Money, Technology, Travel, From the Observer, Fashion, Crosswords, Law, etc. Stream_producer.py script can capture a category for each news article and assign a label index for that category.
Example of Stream_producer.py usage:
```
python3 stream_producer.py API-key fromDate toDate  
```
For instance:
``` 
python3 stream_producer.py API-key 2018-11-3 2018-12-24
```

To simulate real-time streaming and processing, I have collected data by streamming through kafka (by using ‘Stream_producer.py’). The classification model is built offline with articles from such collected data.

Following, in Spark context, I have created a Pipeline model with Tokenizer, Stopword remover, Labelizer, TF-IDF vectorizer, and Classifier. I have tested on many different classification techniques and later presented performance results (i.e., Accuracy, Recall, etc.) for the chosen classification technique.

Finally, I have visualized the results over sliding Windows using Elastic Search & Kibana. I have created a Tag Cloud showing trending new article topics in a given period.

The following figure depicts the framework structure.
![alt text](https://github.com/prit2596/Windows-based-Spark-Streaming/blob/master/FrameworkStructure.PNG)
# Files Description

* **stream_producer.py** : generated offline data for training

* **training.scala** : Loading of data and then doing the pre-processing tasks - tokenizer, stop words removal, tf-idf. Training the dataset on Decision Tree Classifier and Naive Bayes Classifier

* **streaming.scala** : Fetching the news from kafka server in intervals of 20 seconds and using trained model does the classification process and displays the performance results(accuracy,precision,recall)

# How to Run the Code

1)In Project folder, run 'sbt assembly' to generate fat jar

2)Run the training of data and creating ml models:
```
spark-submit --class training <Path to the jar file>IdeaProjects/HW3/target/scala-2.11/HW3-assembly-0.1.jar
```

3)Run the live data on the trained model:
```
spark-submit --class streaming IdeaProjects/HW3/target/scala-2.11/HW3-assembly-0.1.jar
```

Also simultaneously run the stream_producer.py file to fetch the data
```
python3 stream_producer.py ecc8e743-5481-40f5-aed3-ca01b6d8e12d 2019-04-03 2019-04-04
```

# Results

## Performance Screenshots:

![alt text](https://github.com/prit2596/Windows-based-Spark-Streaming/blob/master/Screenshot1.png)
![alt text](https://github.com/prit2596/Windows-based-Spark-Streaming/blob/master/Screenshot2.png)
![alt text](https://github.com/prit2596/Windows-based-Spark-Streaming/blob/master/Screenshot3.png)
![alt text](https://github.com/prit2596/Windows-based-Spark-Streaming/blob/master/Screenshot4.png)


## Kibana Visualizer

![alt text](https://github.com/prit2596/Windows-based-Spark-Streaming/blob/master/kibana1.PNG)
![alt text](https://github.com/prit2596/Windows-based-Spark-Streaming/blob/master/kibana2.PNG)
![alt text](https://github.com/prit2596/Windows-based-Spark-Streaming/blob/master/kibana3.PNG)
---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Hadoop Hands-On (Process Mining with Hadoop)"
subtitle: ""
summary: ""
authors: [Gyunam Park]
tags: []
categories: []
date: 2020-01-31T07:55:14+01:00
lastmod: 2020-01-31T07:55:14+01:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

# Hadoop Hands-on

*(Last updated: 31. January. 2020)*

This blog post is a supplement for Hadoop instruction at *Introduction to Data Science, RWTH-Aachen*. This post covers:

- **What is Hadoop Distributed File System (HDFS)? How can we use it?**
- **What is Hadoop MapReduce? How can we use it?**
- **How can we apply process mining techniques to an event log with billions of events with Hadoop?**

We are living in the world of **big data**. Data is being generated at all the places we can imagine. If you look outside the window, people waiting for the bus generate huge amount of logs while surfing websites and watching Youtube clips. The houses also produce lots of data from sensors attached to electrical machines, even including bulbs. This enables companies to develop services (e.g., personalized recommendation) which benifits their customers (like us) a lot.

If you think of how the companies give those advantages to us, it is not coming for free. Companies are struggling to manage the data as efficient as possible. Building big data infrastructure is one of those efforts. How do they exploit big data infrastructure to manage the big data efficiently? There are three most important trends in constructing big data infrastructure

- *Distribution*
- *More data in memory*
- *Streaming*

In this post, I am going to deal with ***distribution*** part.

If we have a super computer which is capable of handling big data with fast computation and reliable fault tolerance, we don't really have to worry much about distributing the storage and computation into different machines. However, most of the time, it is not the case. Then, what can we do? The answer is distribution.

The motivation is that we store the massive volume of data into multiple cheap commodities and parallelize computation across CPUs of the commodities. With this simple idea, we are able to store and analyze big data. Then, how can we achieve it? The answer is Hadoop.

*([Wikipedia](https://en.wikipedia.org/wiki/Apache_Hadoop)) Hadoop is a collection of open-source software utilities that facilitate using a network of many computers to solve problems involving massive amounts of data and computation. It provides a software framework for distributed storage and processing of big data using the MapReduce programming model.*

The first phase of hadoop was composed of Hadoop Distributed File System (HDFS), which is used for storing data, and MapReduce programming model, which is used for distributing computation. The second phase of hadoop was more elaborated by separating the resource management functionality of previous MapReduce into Yarn and introducing more specialized applications like Hive, which is used for making queries. In the third phase, it becomes much more elaborated, and there are hundreds of applications available in the context of Hadoop framework, which are used for machine learning, streaming data analysis, cloud environment, etc.

{{< figure src="hadoop-history.png" lightbox="true" >}}

The best way to understand how Hadoop works is to learn about **HDFS and MapReduce**, which are basic building blocks for various other applications.

### 1. HDFS

If you upload a file into DFS, it is split into data blocks, and each of them is stored into different nodes. For the purpose of fault tolerance, you can make copies of those blocks and store them into different nodes. Let's say you have two files, *file1.txt and file2.txt*. They are divided into three and two blocks, respectively. Each block is copied three times, and stored into data nodes. For example, *file1.txt* is splited into three blocks, and the three copies of block *A* are stored at data node #1, #2, and #4. It gurantees fault tolerance, i.e., even though data node #1 fails, there are data node #2 and #4, which are still running.

{{< figure src="hdfs-example.png" lightbox="true" >}}

So, how can we upload data into HDFS? Let's have a look at some basic commands for HDFS.

#### 1.1. Preparation

For windows, open your CMD (or Anaconda prompt) with administrator role, and type below:

```
$ hadoop dfsadmin -safemode leave
$ start-all
```

For Mac/Linux,

```
$ start-all.sh
```

#### **1.2. Commands**

1. Cat: Displaying the contents of the filename on console or stdout

   ```
   $ hadoop fs -cat /file1
   ```

2. CopyFromLocal/CopyToLocal: uploading/downloding file from/to local

   ```
   $ hadoop fs –copyFromLocal file:///file1  /folder1
   $ hadoop fs –copyFromLocal file:///folder1  /folder2
   $ hadoop fs –copyFromLocal file:///file1  /folder1/file2
   ```

3. cp: relocating files in HDFS

   ```
   $ hadoop fs –cp /file1  /folder1
   $ hadoop fs –cp /file1  /file2  /folder1
   $ hadoop fs –cp /folder1  /folder2
   ```

4. ls: listing the files in the current directory

   ```
   $ hadoop fs –ls /folder1
   $ hadoop fs –ls /file1
   ```

5. mkdir: making directory

   ```
   $ hadoop fs –mkdir /folder1
   $ hadoop fs –mkdir –p /folder1/folder2/folder3
   ```

6. rm: removing file

   ```
   $ hadoop fs –rm -r /folder1
   $ hadoop fs –rm /file1
   $ hadoop fs –rm  –r /folder1
   ```

#### **1.3. Excercises**

- Build the folders /test/input in your HDFS
- Build the folders /test/output in your HDFS
- Copy the local file PriceSum1.txt into folder /test/input
- Show the contents of PriceSum1.txt in your terminal
- Delete the file /test/input/PriceSum1.txt



### 2. MapReduce

We uploaded our files into HDFS, and they are splited into some blocks, copied, and stored into data nodes.  So, what's next? It is time to do some actual computations using MapReduce Programming model. Let's do word counting with Hadoop.

#### 2.1. Concept

Suppose we have a file, *WordCount1.txt*, which contains the following sentences:

- the quick brown fox
- the fox ate the mouse
- how now brown cow

Assume that this file is split into three blocks, each of which contains one sentence. How can we count the frequency of words in this file? Now, it's time for MapReduce (MR).

MR consists of three functions, *map*, *suffle*, and *reduce*. Map function $map \in K_1 \times V_1 \to (K_2 \times V_2)^*$ maps tuples into sets of tuples.

{{< figure src="map-example.png" lightbox="true" >}}

For example, the block 1 (i.e., sentence 1), $(block_1, the \; quick \; brown \; fox)$ is mapped into $\{ (the,1),(brown,1),(fox,1),(quick,1) \}$. The block 2 (i.e., sentence 2), $(block_2,the \; fox \; the \; ate \; mouse)$, is mapped into $\{ (the,1),(fox,1),(the,1),(ate,1),(mouse,1) \}$.



Suffle function $suffle \in (K_2 \times V_2)^* \to K_2 \times (V_2)^*$ maps sets of tuples into tuples of a key and a set.

{{< figure src="shuffle-example.png" lightbox="true" >}}

For example, $(brown,1)$ from block 1 and $(brown,1) $ from block 2 are mapped into $(brown,[1,1])$.

Reduce function $reduce \in K_2 \times (V_2)^* \to (K_3 \times V_3)^*$ maps tuples of a key and a set to sets of tuples.

{{< figure src="reduce-example.png" lightbox="true" >}}

#### 2.2. Excercise

- For the input document, calculate the total price for each invoice ID. Presume you use MapReduce to do this, please write down the output of each Map function, the output after shuffle, and the output of Reduce function.

  {{< figure src="pricesum-input.png" lightbox="true" style="zoom:50%;" >}}



### 3. MapReduce Programming with Python

So far, we have learned what MapReduce is and how it works. Now, let our Hadoop framework do what we did by hand. Let's first recap what's done by hand. Given an input, we applied map function, shuffle function, and reduce function. Then, what do we need to do for Hadoop to do it instead of us?

1. First, upload file into HDFS (Give input)

2. Write Map function (in Python)

   *(Shuffling is done by Hadoop)*

3. Write Reduce function (in Python)

4. Write Command

#### 3.1. Uploading file into HDFS.

Also, see 1.2.

```
$ hadoop fs -copyFromLocal ./WordCount1.txt /test/input
```

#### 3.2. Write Map function

```python
#!/usr/bin/env python

import sys

# input comes from STDIN
for sentence in sys.stdin:
    # remove whitespace
    sentence = sentence.strip()
    # split the sentence into words
    words = sentence.split()
    # increase counters
    for word in words:
        # write the results to STDOUT;
        # key: word, value: 1 (count of the word)
        print('%s\t%s' % (word, 1))
```

#### 3.3. Write Reduce function

```python
#!/usr/bin/env python

from operator import itemgetter
import sys

current_word = None
current_count = 0
word = None

# input comes from STDIN
for kv_pair in sys.stdin:
    # remove whitespace
    kv_pair = kv_pair.strip()
    # parse the input (word,count) we got from mapper.py
    word, count = kv_pair.split('\t', 1)
    # convert count (currently a string) to int
    count = int(count)
    # shuflling is done by Hadoop
    if current_word!=word:
        if current_word:
            # write result to STDOUT
            print('%s\t%s' % (current_word, current_count))
        current_word = word
        current_count = count
    else:
        current_count += count

# output the last word
if current_word == word:
    print('%s\t%s' % (current_word, current_count))
```

#### 3.4. Command

For Windows:

```
$ hadoop jar C:\hadoop-2.8.4\share\hadoop\tools\lib\hadoop-streaming-2.8.4.jar -input hdfs:///test/input/WordCount1.txt -output hdfs:///test/output/WordCountOutput0 -mapper "python C:\Users\park\Desktop\bigdata\new_instruction\word_mapper.py" -reducer "python C:\Users\park\Desktop\bigdata\new_instruction\word_reducer.py" -file C:\Users\park\Desktop\bigdata\new_instruction\word_mapper.py -file C:\Users\park\Desktop\bigdata\new_instruction\word_reducer.py
```

For Mac&Linux:

```
hadoop jar /usr/local/Cellar/hadoop-2.8.4/share/hadoop/tools/lib/hadoop-streaming-2.8.4.jar \
-file /Users/GYUNAM/Desktop/bigdata/instruction/word_mapper.py \
-mapper "python word_mapper.py" \
-file /Users/GYUNAM/Desktop/bigdata/instruction/word_reducer.py \
-reducer "python word_reducer.py" \
-input /test/input/WordCount1.txt \
-output /test/output/WordCountOutput
```

#### 3.5. Excercise

Write down the Python code of mapper and reducer to solve the problem from exercise 1: calculate the total price for each invoice id for a given document with the same format as shown in exercise 1. Run your code over the file PriceSum1.txt stored in HDFS



### 4. Process Mining with Hadoop

Let's say we have an event log with billions of events. How can we discover process model from this event log?

The answer is to use Hadoop framework. We can distribute the event log into multiple data nodes, and apply MapReduce programming model to compute directly follows relations (i.e., how many times activity x is followed by activity y). For this, we need to apply two MapReduce tasks. The first task is to generate traces from the event log by using MapReduce. The second is to discover directly follows relations (DFR) from the traces. Afterward, this direclty follows relations to discover a directly follows graph (DFG) and a workflow net.

Below is the overview of our approach

{{< figure src="pm-overview.png" lightbox="true" style="zoom:67%;" >}}

#### 4.1. MapReduce Task (1)

Below is the description of how it works:

{{< figure src="pm-task1.png" lightbox="true" style="zoom:67%;" >}}

##### 4.1.1 Map function

```python
#!/usr/bin/env python

import sys

# input comes from STDIN (standard input)
for line in sys.stdin:
	# remove whitespace and split row into values
    line_split = line.strip().split("\t")
    # assign case, activity, timestamp
    case = line_split[0]
    activity = line_split[1]
    timestamp = line_split[3]
    # write the results to STDOUT;
    # key: case, value: (timestamp,activity)
    print('%s\t%s\t%s' % (case, timestamp, activity))
```

##### 4.1.2 Reduce function

```python
#!/usr/bin/env python
import sys
import json

current_case = None

# input comes from STDIN
for line in sys.stdin:
	# remove whitespace and parse the input (case,(timestamp, activity)) we got from mapper.py
	line_split = line.strip().split("\t")
	caseid, timestamp, activity = line_split[0], line_split[1], line_split[2]
	# shuflling is done by Hadoop
	if caseid != current_case:
		# write result to STDOUT
		if current_case:
			print('%s\t%s' % (current_case,json.dumps(current_trace)))
		# reset current trace
		current_case = caseid
		current_trace = list()
		current_trace += [activity]
	else:
		current_trace += [activity]

# output the last word
if current_case == caseid:
	print('%s\t%s' % (caseid,json.dumps(current_trace)))
```

#### 4.2. MapReduce Task (2)

Below is the description of how it works:

{{< figure src="pm-task2.png" lightbox="true" style="zoom:67%;" >}}

##### 4.2.1. Map function

```python
#!/usr/bin/env python

import sys
import json

# input comes from STDIN
for line in sys.stdin:
	# remove whitespace and split row into values
	line_split = line.strip().split("\t")
	# load trace into list of activities
	activities = json.loads(line_split[1])
	for i in range(len(activities)-1):
		# write the results to STDOUT;
    	# key: (from,to), value: 1 (count of the relation)
		stru = activities[i] + "," + activities[i + 1]
		print('%s\t%s' % (stru, 1))
```

##### 4.2.2. Reduce function

```python
#!/usr/bin/env python
"""reducer.py"""

from operator import itemgetter
import sys

current_relation = None
current_count = 0
relation = None

# input comes from STDIN
for line in sys.stdin:
    # remove whitespace
    line = line.strip()
    # parse the input ((from,to),count) we got from mapper.py
    relation, count = line.split('\t', 1)
    # convert count (currently a string) to int
    count = int(count)
    # shuflling is done by Hadoop
    if current_relation!=relation:
        if current_relation:
            # write result to STDOUT
            print('%s\t%s' % (current_relation, current_count))
        current_relation = relation
        current_count = count
    else:
        current_count += count

# output the last relation
if current_relation == relation:
    print('%s\t%s' % (current_relation, current_count))
```

#### 4.3. Command

##### 4.3.1. MapReduce Task (1)

```
#For Windows users
!hadoop jar C:\hadoop-2.8.4\share\hadoop\tools\lib\hadoop-streaming-2.8.4.jar -file C:\hadoop-handson\pm_mapper1.py -mapper "python C:\hadoop-handson\pm_mapper1.py" -file C:\hadoop-handson\pm_reducer1.py -reducer "python C:\hadoop-handson\pm_reducer1.py" -input hdfs:///test/input/running-example.tsv -output hdfs:///test/output/DFG0

#For Mac/Linux users
!hadoop jar /usr/local/Cellar/hadoop-2.8.4/share/hadoop/tools/lib/hadoop-streaming-2.8.4.jar \
-file /Users/GYUNAM/Desktop/bigdata/instruction/pm_mapper1.py \
-mapper "python pm_mapper1.py" \
-file /Users/GYUNAM/Desktop/bigdata/instruction/pm_reducer1.py \
-reducer "python pm_reducer1.py" \
-input /test/input/running-example.tsv \
-output /test/output/DFG0
```

##### 4.3.2. MapReduce Task (2)

```
#For Windows users
!hadoop jar C:\hadoop-2.8.4\share\hadoop\tools\lib\hadoop-streaming-2.8.4.jar -file C:\hadoop-handson\pm_mapper2.py -mapper "python C:\hadoop-handson\pm_mapper2.py" -file C:\hadoop-handson\pm_reducer2.py -reducer "python C:\hadoop-handson\pm_reducer2.py" -input hdfs:///test/output/DFG0/part-00000 -output hdfs:///test/output/DFG0-final

#For Mac/Linux users
!hadoop jar /usr/local/Cellar/hadoop-2.8.4/share/hadoop/tools/lib/hadoop-streaming-2.8.4.jar \
-file /Users/GYUNAM/Desktop/bigdata/instruction/pm_mapper2.py \
-mapper "python pm_mapper2.py" \
-file /Users/GYUNAM/Desktop/bigdata/instruction/pm_reducer2.py \
-reducer "python pm_reducer2.py" \
-input /test/output/DFG0/part-00000 \
-output /test/output/DFG0-final
```

##### 4.3.3. Copy output from HDFS to Local

```
#For Windows users
!hadoop fs -copyToLocal /test/output/DFG0-final/part-00000 C:\hadoop-handson\dfr1.txt

#For Mac/Linux users
!hadoop fs -copyToLocal /test/output/DFG0-final/part-00000 ./dfr1.txt
```

#### 4.4 Process Discovery

For applying process mining techniques, we use [PM4PY](https://pm4py.fit.fraunhofer.de/).

```python
# 1. Import libraries
import os
import csv
from pm4py.objects.log.importer.xes import factory as xes_importer
from pm4py.objects.conversion.dfg import factory as dfg_mining_factory
from pm4py.algo.discovery.dfg import factory as dfg_factory
from pm4py.visualization.dfg import factory as dfg_vis_factory
from pm4py.visualization.petrinet import factory as pn_vis_factory


# 2. preprocessing
with open('dfr1.txt') as file:
    file_reader = csv.reader(file, delimiter='\t')
    dfg = dict()
    for row in file_reader:
        _from,_to=row[0].split(',')
        rel = (_from,_to)
        freq = int(row[1])
        dfg[rel] = freq

# 3. Visualize Directly-follows-graph (DFG)
gviz = dfg_vis_factory.apply(dfg)
dfg_vis_factory.view(gviz)

# 4. Discover and Visualize Workflow-Net
net, im, fm = dfg_mining_factory.apply(dfg)
gviz = pn_vis_factory.apply(net, im, fm)
pn_vis_factory.view(gviz)
```
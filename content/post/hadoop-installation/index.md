---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "The (easiest) Hadoop installation - Windows"
subtitle: ""
summary: ""
authors: [Gyunam Park]
tags: [BigData,Hadoop,Installation]
categories: [big data]
date: 2020-01-23T19:57:39+01:00
lastmod: 2020-01-23T19:57:39+01:00
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

# The (easiest) Hadoop installation - Windows

*(Last updated: 24. Jan. 2020 14:00)*

This blog post is a supplement for Hadoop instruction at *Introduction to Data Science, RWTH-Aachen*. The goal is to make sure that everyone can run Hadoop at his/her PC. Thus, I aim for explaining how to install Hadoop in the easiest manner by taking the simplest approach. This is not a succint way to install Hadoop (If you think you are quite familar with how (system-related) things work, I recommend you to find other blog posts).

Let's get started.

### 1. Install Java

Hadoop is based on Java language, so you need to have Java SE Development Kit in your PC.

You can download it in the link below:

https://www.oracle.com/technetwork/java/javase/downloads/jdk8downloads-2133151.html

### 2. Make soft link

If you do not specify the directory, JDK is installed at "Program Files". The problem is that the space in the path name (e.g., C:\Program Files\Java\jdk1.8.0_213 ) is not allowed in Hadoop configuration. To prevent this issue, we use **softlink**. The idea is simple. We do not physically move the JDK (you may already have several dependencies on it from different programs.). Instead, we make an artifitial link which connects to JDK.

Open terminal and type:

```
$ mkdir C:\tools
$ mklink /j C:\tools\java "YOUR_JDK_DIRECTORY"
```

### 3. Download hadoop

Download [hadoop](https://archive.apache.org/dist/hadoop/core/hadoop-2.8.4/
) and unzip it at C:\hadoop-2.8.4

### 4. Configure System Environment

We need to set Hadoop PATH in system environment. This step is required to use hadoop commands at CMD.

Add below to your system path

1. C:\hadoop-2.8.4\bin
2. C:\hadoop-2.8.4\sbin

### 5. Configure Hadoop

Go to "C:\hadoop-2.8.4\etc\hadoop". You will see a bunch of files, among which you need to modify 6 files.

1. yarn-site.xml

   - Yarn is resource manager. The yarn.nodemanager.aux-services property tells NodeManagers that an auxiliary service, called mapreduce.shuffle, will be needed. Afterward, we give the class name to use for shuffling.

   - Replace a block

     - ```
       <configuration>

       ...

       </configuration>
       ```

   - into

     - ```
       <configuration>
       <property>
         <name>yarn.nodemanager.aux-services</name>
         <value>mapreduce_shuffle</value>
       </property>
       <property>
           <name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>
           <value>org.apache.hadoop.mapred.ShuffleHandler</value>
         </property>
       </configuration>
       ```

2. core-site.xml

   - core-site is a website showing the overview of running Hadoop. We will specify the directory for temp files and set the port of the website as 9000.

   - Replace a block

     - ```
       <configuration>

       ...

       </configuration>
       ```

   - into

     - ```
       <configuration>
       <property>
         <name>hadoop.tmp.dir</name>
         <value>\C:\hadoop-2.8.4\data\tmp</value>
       </property>
       <property>
         <name>fs.defaultFS</name>
         <value>hdfs://localhost:9000</value>
       </property>
       </configuration>
       ```

3. hdfs-site.xml

   - This is the configuration for HDFS. We specify the number of replications for each file and the directories of namenode, secondary namenode, and datanode. We set the permission checking as false.

   - Replace a block

     - ```
       <configuration>

       ...

       </configuration>
       ```

   - into

     - ```
       <configuration>
       <property>
         <name>dfs.replication</name>
         <value>2</value>
       </property>
       <property>
         <name>dfs.permissions</name>
         <value>false</value>
       </property>
       <property>
         <name>dfs.namenode.name.dir</name>
         <value>\C:/hadoop-2.8.4\data\dfs\namenode</value>
       </property>
       <property>
         <name>dfs.namenode.checkpoint.dir</name>
         <value>\C:/hadoop-2.8.4\data\dfs\namesecondary</value>
       </property>
       <property>
         <name>dfs.datanode.data.dir</name>
         <value>\C:/hadoop-2.8.4\data\dfs\datanode</value>
       </property>
       </configuration>
       ```

4. mapred-site.xml

   - It defines MapReduce framework for Hadoop. We specify which framework we will use to run MapReduce, and it run as a Yarn application. In addition, we set the website port.

   - Replace a block

     - ```
       <configuration>

       ...

       </configuration>
       ```

   - into

     - ```
       <configuration>
       <property>
         <name>mapred.job.tracker</name>
         <value>localhost:9010</value>
       </property>
       <property>
         <name>mapreduce.framework.name</name>
         <value>yarn</value>
       </property>
       </configuration>
       ```

5. hadoop-env.sh

   - In this file, we specify environment variables for the JDK used by Hadoop.
   - We need to change "export JAVA_HOME=$JAVA_HOME" into "export JAVA_HOME=C:\tools\java".

6. hadoop-env.cmd

   - In addition to previous configuration, we need to change "set JAVA_HOME=$JAVA_HOME" into "set JAVA_HOME=C:\tools\java".

### 6. Start and run Hadoop

To set up Hadoop, we need to format the namenode.

At your CMD, type

```
$ hdfs namenode -format
```

(It is supposed to be done only once. However, if your system setting changes (e.g., changing IP address), you need to format it again. In this case, you first need to clear C:\hadoop-2.8.4\data\)

To launch Hadoop, type

```
$ start-all
$ jps //to check if it works
```

Open Hadoop Management web page http://localhost:50070.

To stop,

```
$ stop-all
```

You are DONE!
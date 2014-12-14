#Notes on Hadoop
======

## How to install Hadoop on a single node Linux system, however allowing allowing pseudo-distributed operation

1. Go to hadoop.apache.org and download your favorite release. Typically you want to download the .tar.gz file - let's call it hadoop-a.b.c.tar.gz.
2. Extract the package by ``` tar xzf hadoop-x.y.z.tar.gz ```
3. Check whether variable JAVA_HOME is set up by ```echo $JAVA_HOME```. If not

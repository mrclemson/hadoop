## How to install Hadoop on a single node Linux system, however allowing allowing pseudo-distributed operation

1. Go to hadoop.apache.org and download your favorite release. Typically you want to download the .tar.gz file - let's call it hadoop-a.b.c.tar.gz.
2. Extract the package by ``` tar xzf hadoop-x.y.z.tar.gz ```
3. Check whether variable JAVA_HOME is set up by ```echo $JAVA_HOME```. If not, add this to your profile ```export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64```. The actual directory of your java installation may change, but typicaly it should be under /usr/lib/jvm.
4. Since you are already editing the profile file, you may also want to add the bin and sbin directories to the PATH variable: first run ```export HADOOP_HOME=$my_hadoop_dir/hadoop-x.y.z```; and then add to the profile file ```export PATH="$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin"```. Note ```$my_hadoop_dir``` is the directory where the extracted hadoop package is.
5. Now you may want to test to see whether everything works up to this point. First you will need to refresh by exiting the current terminal and opening a new one; or you can run ```. .bashrc``` if .bashrc is where you store your profile. Afterword, you can run some random hadoop commands such as ```hadoop version``` or simply ```hadoop```.
6. The next step is to get ssh is up and running. To test, run ```ssh localhost```. If it says something like welcome etc., then you can skip to the next step. Otherwise, follow [this document](https://help.ubuntu.com/lts/serverguide/openssh-server.html).
7. All remaining steps, you can simply follow [the official website](http://hadoop.apache.org/docs/r2.6.0/hadoop-project-dist/hadoop-common/SingleCluster.html "Have fun!").
8. I encountered some issues: if I start with ```hadoop namenode -format``` followed by ```start-all.sh``` or ```start-dfs.sh```. The platform would run perfectly. But apparently I don't want to format the entire namenode and start from scratch every time. I did some further research, and was able to resolve it by changing two files ```core-site.xml``` and ```hdfs-site.xml``` both under ```$my_hadoop_dir/etc/hadoop```. ```core-site.xml``` should look something like
```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/home/eric/programs/hadoop/tmp</value>
    </property>
    <property>
        <name>fs.checkpoint.dir</name>
        <value>/home/eric/programs/hadoop/snn</value>
     </property>
</configuration>
```
And ```hdfs-site.xml``` should look like
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.name.dir</name>
        <value>/home/eric/programs/hadoop/nn</value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>/home/eric/programs/hadoop/dn</value>
    </property>
    <property>
        <name>dfs.permissions.supergroup</name>
        <value>hadoop</value>
    </property>
</configuration>
```

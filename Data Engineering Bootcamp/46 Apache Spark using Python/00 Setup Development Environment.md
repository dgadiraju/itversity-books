# Setup Environment

As part of this session we will see how to setup environment to practice Spark in a single node cluster with Hadoop.

* Setup on Windows
* Provision Server from AWS
* Setup Pre-Requisites
* Validate Python
* Setup and Configure Hadoop
* Start and Validate Hadoop Components
* Setup and Validate Spark
* Configure Jupyter with Spark

The content will be free, however if you want to watch videos to understand key concepts feel free to [join our YouTube Channel as a member](https://www.youtube.com/channel/UCakdSIPsJqiOLqylgoYmwQg/join).

Here is the video for the topics starting from **Setup on Windows** up to **Start and Validate Hadoop Components**. There is another video as a continuation, scroll down to Setup and Validate Spark to get the continued video.

[![Data Engineering - Setup Development Environment](http://img.youtube.com/vi/Z4pDhBaWB64/0.jpg)](http://www.youtube.com/watch?v=Z4pDhBaWB64 "Data Engineering - Setup Development Environment")

## Setup on Windows
One can setup Spark on Windows, but getting all the key technologies with respect to Data Engineering working on Windows is a bit challenge.
* If you have 16 GB RAM and Quad Core, then you can setup a single node Hadoop and Spark Cluster for practice.
* If you do not have high end laptop, you can provision a VM on AWS or GCP and practice. Make sure you stop the VM when you are not using it. 
* Here are the steps to setup the virtual machine on Windows. On Mac or Linux you do not need to have additional VM.
  * Install Virtual Box for Windows
  * Install Vagrant for Windows
  * Create Virtual Machine with CentOS 7
  * Make sure you mount the working directory on to the VM, otherwise you have to copy the code once in a while.
  
```
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  config.vm.synced_folder ".", "/vagrant", type: 'virtualbox'
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = "2"
    vb.memory = "4196"
  end
end
```

## Provision Server from AWS
Let us understand how to provision server from AWS quickly so that we can setup single node Big Data Cluster to practice key technologies such as Spark, NiFi, Kafka etc.

* Operating System - Centos 7
* Instance Type - **t2x.large** (4 vCores and 16 GB RAM).
* Root File System size - 60 GB
* Make sure elastic ip is configured as we will be stopping EC2 instance frequently.
* Make sure to understand the pricing.

Even though demo is given on AWS, you can use server with similar configuration from any cloud platform or bare metal servers.

## Setup Pre-Requisites
Let us setup some of the pre-requisites required to setup Hadoop, Spark, NiFi etc on a single node.
* Use `yum` to install JDK 1.8, git, wget, telnet etc.
```
sudo yum -y install python3 wget telnet java-1.8.0-openjdk-devel git
```
* JDK 1.8 is required by most of the Big Data technologies.
* **telnet** comes handy to troubleshoot whether the services are started using specific port number or not.
* **wget** will be used to download required softwares.
* **git** will be used to clone GitHub repositories of the data and code that is used as part of this course.

## Validate Python
Let us make sure Python 3 is installed and validated.
* Check all the versions of Python that are available.
* Quick overview of Python 3
* Installing libraries using Python 3 `python3 -m pip install pandas`

## Setup and Configure Hadoop
Let us understand how to download, setup and configure Hadoop on single node.
* Download from Hadoop.
* Configure HDFS.
* Here are the contents of **core-site.xml**.
```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
* Here are the contents of **hdfs-site.xml**.
```
<configuration>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/opt/hadoop/dfs/name</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/opt/hadoop/dfs/name</value>
    </property>
    <property>
        <name>dfs.namenode.checkpoint.dir</name>
        <value>/opt/hadoop/dfs/namesecondary</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/opt/hadoop/dfs/data</value>
    </property>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```
* Configure YARN.
* Here are the contents of **yarn-site.xml**.
```
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>
```
* Here are the contents of **mapred-site.xml**.
```
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.application.classpath</name>
        <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
    </property>
</configuration>
```
* Format HDFS so that directories for Namenode, Secondary Namenode as well as Datanode are created.
```
hdfs namenode -format
ls -ltr /opt/hadoop/dfs/
```

## Start and Validate Hadoop Components
Let us start and validate hadoop components.
* Ensure environment variables such as PATH are updated with bin folder of Hadooop.
* We can start HDFS components using `start-dfs.sh`.
* Run `jps` command to ensure **Namenode**, **Secondary Namenode** and **Datanode** are running.
* We can start YARN components using `start-yarn.sh`.
* Run `jps` command to ensure **Resource Manager** and **Node Mnager** are running.
* Create **/user/centos** folder structure or folder structure of your choice.
```
hdfs dfs -mkdir -p /user/centos
hdfs dfs -put /opt/hadoop/etc/hadoop /user/centos
hdfs dfs -cat /user/centos/core-site.xml
```

Here is the continuation for setting up development environment for Data Engineering using Apache Spark on top of Hadoop.

[![Data Engineering - Setup Development Environment](http://img.youtube.com/vi/jcdAO90yY2U/0.jpg)](http://www.youtube.com/watch?v=jcdAO90yY2U "Data Engineering - Setup Development Environment (Contd...)")

## Setup Datasets

Let us go ahead and setup retail_db dataset to get started with NiFi.

Please go to this [page](https://www.github.com/dgadiraju/retail_db.git) and take care of setting up retail_db data set.

* In my case I have setup retail_db dataset under /data folder on the server where we have setup NiFi.
```
sudo mkdir /data
sudo chown -R centos:centos /data
git clone https://www.github.com/dgadiraju/retail_db.git /data/retail_db
```

## Setup and Validate Spark
Let us understand how to setup and validate Spark. We will be using YARN to submit Spark Jobs for our practice.

* Download Spark 2.4.6 Version.
* Update Spark Configuration files.
* Validate Spark using Scala by running `spark-shell --master yarn --conf spark.ui.port=0`.
* Here is Scala based code which you can use to validate.
```
val df = spark.read.
    option("inferSchema", true).
    csv("/user/centos/retail_db/orders").
    toDF("order_id", "order_date", "order_customer_id", "order_status")

// Previewing Schema
df.printSchema

// Perform Operation and Preview Output
df.groupBy("order_date").count.show

// Write processed data to HDFS
df.groupBy("order_date").count.
    write.
    option("header", true).
    csv("/user/centos/retail_db/order_count_by_date")
```
* Validate Spark using Python by running `pyspark --master yarn --conf spark.ui.port=0`. Make sure to export PYSPARK_PYTHON to point to Python 3 if the default version is 2.7.
```
export PYSPARK_PYTHON=python3
pyspark --master yarn --conf spark.ui.port=0
```
* Here is Python based code which you can use to validate.
```
df = spark.read. \
    option("inferSchema", True). \
    csv("/user/centos/retail_db/orders"). \
    toDF("order_id", "order_date", "order_customer_id", "order_status")

# Previewing Schema
df.printSchema()

# Perform Operation and Preview Output
df.groupBy("order_date").count().show()

# Write processed data to HDFS
df.groupBy("order_date").count(). \
    write. \
    option("header", True). \
    csv("/user/centos/retail_db/order_count_by_date")
```
* Validate Spark SQL by running `spark-sql --master yarn --conf spark.ui.port=0`.

## Configure Jupyter with Spark
Let us have a environment on the VM where Jupyter and Spark are integrated. It can be the local VM or it can be VM on Cloud.
* Install Jupyter and Jupyter Lab
* Validate that Jupyter and Jupyter Lab are working fine
* Install Apache Toree for Jupyter. It will facilitate you to practice Spark using browser.
* Quick demo of using itversity books on the VM for the practice.

FROM atabak/hadoop-base:latest
MAINTAINER Atabak

# Update the APT cache 
RUN apt-get update 
RUN apt-get upgrade -y 

# Install and setup project dependencies 
RUN apt-get install -y curl wget git 

#prepare for Java download 
RUN apt-get install -y python-software-properties 
RUN apt-get install -y software-properties-common

#java
RUN apt-get install -y openjdk-8-jdk && \ 
    apt-get install -y ant && \ 
    apt-get clean && \ 
    rm -rf /var/lib/apt/lists/* && \ 
    rm -rf /var/cache/oracle-jdk8-installer;

RUN mkdir -p /usr/java/default
RUN sudo cp -r /usr/lib/jvm/java-8-openjdk-amd64/* /usr/java/default/

ENV JAVA_HOME /usr/java/default
ENV PATH $PATH:$JAVA_HOME/bin


#install spark
ENV SPARK_HOME /usr/local/spark
ENV SPARK_VERSION 2.1.1
ENV SPARK_BIN_VERSION $SPARK_VERSION-bin-hadoop2.7

RUN curl -s https://www.apache.org/dist/spark/spark-$SPARK_VERSION/spark-$SPARK_BIN_VERSION.tgz | tar xz -C /usr/local
RUN ln -s /usr/local/spark-$SPARK_BIN_VERSION $SPARK_HOME 
#&& \
#    rm /spark-$SPARK_BIN_VERSION.tgz


#RUN mkdir $SPARK_HOME/yarn-remote-client
#ADD yarn-remote-client $SPARK_HOME/yarn-remote-client

#RUN $BOOTSTRAP && $HADOOP_PREFIX/bin/hadoop dfsadmin -safemode leave && $HADOOP_PREFIX/bin/hdfs dfs -put $SPARK_HOME-2.1.1-bin-hadoop2.7/lib /spark

#ENV YARN_CONF_DIR $HADOOP_PREFIX/etc/hadoop
#ENV PATH $PATH:$SPARK_HOME/bin:$HADOOP_PREFIX/bin

# update boot script
ADD bootstrap.sh /etc/bootstrap.sh
#RUN chown root.root /etc/bootstrap.sh
#RUN chmod 700 /etc/bootstrap.sh


#install scala
ENV SCALA_VERSION 2.12.2 
ENV SCALA_HOME /usr/local/scala 
ENV PATH $PATH:$SPARK_HOME/bin:$SCALA_HOME/bin 

RUN wget https://downloads.lightbend.com/scala/$SCALA_VERSION/scala-$SCALA_VERSION.tgz && \ 
    tar -zxf /scala-$SCALA_VERSION.tgz -C /usr/local/ && \ 
    ln -s /usr/local/scala-$SCALA_VERSION $SCALA_HOME && \ 
    rm /scala-$SCALA_VERSION.tgz 

ADD start-master.sh /start-master.sh 
ADD start-worker /start-worker.sh 
ADD spark-shell.sh /spark-shell.sh 
ADD spark-defaults.conf /spark-defaults.conf 
ADD remove_alias.sh /remove_alias.sh 

ENV SPARK_MASTER_OPTS="-Dspark.driver.port=7001 -Dspark.fileserver.port=7002 -Dspark.broadcast.port=7003 -Dspark.replClassServer.port=7004 -Dspark.blockManager.port=7005 -Dspark.executor.port=7006 -Dspark.ui.port=4040 -Dspark.broadcast.factory=org.apache.spark.broadcast.HttpBroadcastFactory" 
ENV SPARK_WORKER_OPTS="-Dspark.driver.port=7001 -Dspark.fileserver.port=7002 -Dspark.broadcast.port=7003 -Dspark.replClassServer.port=7004 -Dspark.blockManager.port=7005 -Dspark.executor.port=7006 -Dspark.ui.port=4040 -Dspark.broadcast.factory=org.apache.spark.broadcast.HttpBroadcastFactory" 
ENV SPARK_MASTER_PORT 7077 
ENV SPARK_MASTER_WEBUI_PORT 8080 
ENV SPARK_WORKER_PORT 8888 
ENV SPARK_WORKER_WEBUI_PORT 8081 

EXPOSE 8080 7077 8888 8081 4040 7001 7002 7003 7004 7005 7006



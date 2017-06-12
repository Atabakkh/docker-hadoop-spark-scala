FROM ubuntu:16.04
MAINTAINER Atabak

USER root

#update 
RUN apt-get update
RUN apt-get install -y curl tar sudo openssh-server openssh-client rsync git wget vim ssh

#java
RUN apt-get install -y openjdk-8-jdk && \ 
    apt-get install -y ant && \ 
    apt-get clean && \ 
    rm -rf /var/lib/apt/lists/* && \ 
    rm -rf /var/cache/oracle-jdk8-installer;


#RUN apt-get update && \ 
#    apt-get install -y ca-certificates-java && \ 
#    apt-get clean && \ 
#    update-ca-certificates -f && \ 
#    rm -rf /var/lib/apt/lists/* && \ 
#    rm -rf /var/cache/oracle-jdk8-installer;


ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/ 
RUN export JAVA_HOME


#hadoop
RUN curl -s http://www.eu.apache.org/dist/hadoop/common/hadoop-2.7.3/hadoop-2.7.3.tar.gz | tar -xz -C /usr/local/
RUN cd /usr/local && ln -s ./hadoop-2.7.3 hadoop

ENV HADOOP_PREFIX /usr/local/hadoop
RUN sed -i '/^export JAVA_HOME/ s:.*:export JAVA_HOME=/usr/java/default\nexport HADOOP_PREFIX=/usr/local/hadoop\nexport HADOOP_HOME=/usr/local/hadoop\n:' $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh
RUN sed -i '/^export HADOOP_CONF_DIR/ s:.*:export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop/:' $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh
#RUN . $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh

RUN mkdir $HADOOP_PREFIX/input
RUN cp $HADOOP_PREFIX/etc/hadoop/*.xml $HADOOP_PREFIX/input

# pseudo distributed
ADD core-site.xml.template $HADOOP_PREFIX/etc/hadoop/core-site.xml.template
RUN sed s/HOSTNAME/localhost/ /usr/local/hadoop/etc/hadoop/core-site.xml.template > /usr/local/hadoop/etc/hadoop/core-site.xml
ADD hdfs-site.xml $HADOOP_PREFIX/etc/hadoop/hdfs-site.xml

ADD mapred-site.xml $HADOOP_PREFIX/etc/hadoop/mapred-site.xml
ADD yarn-site.xml $HADOOP_PREFIX/etc/hadoop/yarn-site.xml

RUN $HADOOP_PREFIX/bin/hdfs namenode -format



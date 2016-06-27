#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
FROM ubuntu

MAINTAINER Thamme Gowda N.<tgowdan@gmail.com>

# install missing part of ubuntu core
RUN apt-get update && \
    apt-get install -y software-properties-common software-properties-common

# Install Java.
RUN \
  echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
  apt-add-repository universe && \
  add-apt-repository -y ppa:webupd8team/java && \
  apt-get update && \
  apt-get install -y oracle-java8-installer maven git && \
  rm -rf /var/lib/apt/lists/* && \
  rm -rf /var/cache/oracle-jdk8-installer 


# Define commonly used JAVA_HOME variable
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

RUN mkdir /data/ /deploy /deploy/solr4 /deploy/parser-indexer

WORKDIR /data
RUN git clone --recursive https://github.com/USCDataScience/imagecat2.git
WORKDIR /data/imagecat2/tika-ner-corenlp
RUN mvn clean compile && mvn install

WORKDIR /data/imagecat2/parser-indexer
RUN mvn clean compile install 
RUN cp -R target/parser-indexer-*.jar /deploy/parser-indexer

# SETUP SOLR
WORKDIR /data
RUN wget -q "http://archive.apache.org/dist/lucene/solr/4.10.4/solr-4.10.4.tgz" 
RUN tar xf solr-4.10.4.tgz -C /deploy/solr4 --strip-components=1 && \
    rm -r /deploy/solr4/example/solr/* && \
    cp -r /data/imagecat2/solr/solr/* /deploy/solr4/example/solr/

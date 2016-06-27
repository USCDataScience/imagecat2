# TODO: add apache licence here

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

# Define default command.
# CMD ["bash"]

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

# COPY entrypoint_imagecatdev.sh /deploy/entrypoint_imagecatdev.sh
# ENTRYPOINT ["/deploy/entrypoint_imagecatdev.sh"]

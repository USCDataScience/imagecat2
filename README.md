# Imagecat Version 2

This project includes tools required to perform metadata analysis. Specifically, it uses Apache Tika to parse metadata from various files and then builds inverted index using solr.


## Quick Start Using Docker

1. Get and install docker for your operating system (if not already)

2. Build a docker image
 This can be done by

    docker build . -f docker/Dockerfile -t imagecat2

3. get inside docker container and start services

  docker run -it imagecat2 # or unique id for your build
  /deploy/solr/bin/solr start # starts solr
  java -jar /deploy/parser-indexer/parser-indexer*.jar # invokes parser indexer, more info check out https://github.com/USCDataScience/parser-indexer
  

## Questions / Bugs / Features ?

Please create an issue at https://github.com/uscdataScience/imagecat2/issues


## Developers

+ Thamme Gowda
+ Chris Mattmann



 
  




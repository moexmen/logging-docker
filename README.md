# Centralised Logging - Graylog & Logstash
A quick centralised logging setup for [Graylog](https://www.graylog.org/) (Version 1.2.1) and [Logstash](https://www.elastic.co/products/logstash) (Version 1.5.4). In this setup, Logstash will centralise and parse logs, and forward them to Graylog for analysis, visualisation and monitoring. The setup is created on docker for easy deployment and administration.

In this repoistory, a sample config file for is provided to parse live tweets. Please follow the instructions for the setup.

*Note that this is still a work in progress.*

# Getting Started

### Installation and Setup
1. Ensure [docker is installed](https://docs.docker.com/installation/)
2. Clone this repository
3. Run the following commands:
```
cp logstash/sample-Dockerfile logstash/Dockerfile
cp logstash/sample-docker-compose.yml logstash/docker-compose.yml
cp graylog/sample-Dockerfile graylog/Dockerfile
cp graylog/sample-docker-compose.yml graylog/docker-compose.yml
```

### Basic Configuration
##### Graylog
- Navigate to the `graylog` folder, uncomment and edit the environment variables in `docker-compose.yml`.

##### Logstash
- Navigate to `logstash/` folder, and make the necessary changes `sample-twitter-demo.conf`. You will require app keys with [Twitter](https://apps.twitter.com/).

### Starting the Containers to Check Live Tweets
The docker-compose files are deliberately kept separate for Graylog and Logstash. You may choose to create a single docker-compose file at the root. 
1. In the `graylog/`  folder, run the following commands:
```
docker-compose build
docker-compose up
```
2. Find the ip using `docker inpect gl`, and login to Graylog (*http://x.x.x.x:9000*) and add a GELF UDP input (System -> Inputs, `Default Port: 12201`)
3. In the `graylog/`  folder, run the following commands:
```
docker-compose build
docker-compose up
```
4. Navigate back to Graylog to see live updates!

### Manual Test for the Docker Setup
1. Start both containers. 
2. Enter the logstash container: `docker exec -it ls bash`
3. Start another logstash instance to forward logs to Graylog : `/opt/logstash/bin/logstash -e 'input{stdin{}}output{gelf{host =>'gl'}}' `
4. Type something and verify whether the messages appear on Graylog. 

### Extending the Logging System
##### First Steps
- For additional patterns for log forwarding, modify the provided config files and logstash dockerfile. 
- Remember to make the necessary changes in `Dockerfile` and `docker-compose.yml` (*exposing of ports, adding of configuration files / patterns*).
- *Note: The start script in the logstash container will run all config files that are added to `/etc/logstash/conf.d`.*

##### Future
- Please read [the docs](http://docs.graylog.org/en/1.2/pages/architecture.html) for scaling up Graylog in the future. 
- You can experiment with more complex/advanced configurations of logstash when required.


### About
The Logstash Dockerfile was adapted from [Sébastien Pujadas](https://pujadas.net), released under the [Apache 2 license](https://www.apache.org/licenses/LICENSE-2.0).


### To Do - Get Badge
Sample

[![](https://badge.imagelayers.io/sebp/elk:latest.svg)](https://imagelayers.io/?images=sebp/elk:latest 'Get your own badge on imagelayers.io')


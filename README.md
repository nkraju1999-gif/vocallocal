# vocallocal
### This code repository contains the following - 
* A python file that will fetch links(urls) to news articles for a certain set of keywords and topics. 
* This file can be found at ```src/news_publisher/NewsPublisher.py``` . 
* This file needs to be scheduled to run periodically.This file pushes all the fetched news article links to a GCP pub-sub topics.
* 
* A consumer script that fetches messages from the pub-sub topic and processes them. 
* This consumer makes use of certain REST APIs to process each message. The APIs are part of a microservice. 
* This consumer script can be found at ```src/news_consumer/NewsConsumer.py```
* The API endpoints for the above microservice can be found in the py files located in src/routes. 
* The microservice makes use of FastAPI. This microservice can be run by running the file ```src/news_collector_api.py```
* The microservice can be deployed as a separate instance by creating a docker container. 
* The files ``` docker-compose.yml``` and ```Dockerfile``` needs to be used to create the container for the microservice.
* The dockerfile ``` DockerFileToRunConsumer ``` is to be used to create a container to run the consumer.


### Requirements to run the code
* Redis
* Postgres
* python 3.11
* The dependencies can be found in ``` requirements.txt```
* If you are running the code locally, you need to set the redis details in the file ``` config/common_configs.py```
* If you are running the code locally, the postgres configs can be set in `` dev.env `` file.
* When deploying the code in production/creating docker images, the redis host and port can be set in ``docker-compose.yml``
* When deploying the code in production, the postgres configs can be set as environment variables in ``Dockerfile``

### Required API keys/credentials

* gcp/google credentials - These are required to make use of GCP pub-sub and google translator API. 
* Make sure you copy-paste the credentials in the file ```config/google_credentials.json```
  * api key from zyte -  This is required to make use of the web scrapping service from ZYTE ( https://www.zyte.com) . 
* set the key in ZYTE_API_KEY in ```dev.env```
* api key from groq (https://groq.com) . This is required because we do summarization of the fetched news articles. 
* For summarization we use LLM(large language models) from groq. set the API key in GROQ_API_KEY in `` dev.env``. 
* The code for calling groq LLMs can be found at ```src/llms/GroqLLM.py```
* api key from serp (https://serpapi.com) . This is required to fetch news articles for a given set of keywords and topics. 
* set the key in GOOGLE_NEWS_SERP_API_KEY in ```dev.env```

#### The postgres table names can be found in ```dev.env```
#### The table schemas can be found in ``config/postgres_configs.py``
#### The files in ```src/controllers``` contains the code to read and manipulate the various data from the tables

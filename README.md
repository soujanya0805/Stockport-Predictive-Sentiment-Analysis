# Stockport-Predictive-Sentiment-Analysis

stocksight
Stock market analyzer and stock predictor using Elasticsearch, Twitter, News headlines and Python natural language processing and sentiment analysis. How much do emotions on Twitter and news headlines affect a stock's price? Let's find out...



About
stocksight is an open source stock market analysis software that uses Elasticsearch to store Twitter and news headlines data for stocks. stocksight analyzes the emotions of what the author writes and does sentiment analysis on the text to determine how the author "feels" about a stock. It could be used for more than finding sentiment of just stocks, it could be used to find sentiment of anything...




Slack workspace
Join the conversation, get support, etc on stocksight Slack.




Hardware Requirement Specification

    • Processor: Minimum 1 GHz; Recommended 2GHz or more
    • Ethernet connection (LAN) OR a wireless adapter (Wi-Fi)
    • Hard Drive: Minimum 32 GB; Recommended 64 GB or more
    • Memory (RAM): Minimum 1 GB; Recommended 4 GB or above
    • Sound card w/speakers
    • Some classes require a camera and microphone

Software Requirement Specification

    • Platform: Anaconda
    • Environment: Python 3
    • Modules: Matplotlib, NumPy, Pandas,Keras,Tensorflow
    
    
Download
$ git clone https://github.com/soujanya0805/Stockport-Predictive-Sentiment-Analysis/git
$ cd stocksight


Install - Docker
*** See how to use below before building the Docker containers ***


Download/clone stocksight repo with git.
Set up stocksight, elasticsearch and kibana containers using Docker compose
cd stocksight
cp config.py.sample config.py
***see how to use below for config.py (stocksight config) changes***
docker-compose build && docker-compose up
This will volume mount config.py (stocksight settings) and twitteruserids.txt to those files in your local git cloned "stocksight" directory

Once all the containers have started up, shell into the container
docker exec -it stocksight_stocksight_1 bash

See examples below for running stocksigh



Install - local
Recommended to install Elasticsearch and Kibana in local machine or other machine/vm/docker

Install python requirements using pip
pip install -r requirements.txt

Install python nltk data
python -c "import nltk; nltk.download('punkt'); nltk.download('stopwords')"



How to use
Create a new twitter application and generate your consumer key and access token. https://developer.twitter.com/en/docs/basics/developer-portal/guides/apps.html https://developer.twitter.com/en/docs/basics/authentication/guides/access-tokens.html

Copy config.py.sample to config.py (stocksight config file)

Set elasticsearch settings in config.py for your env (for Docker, set elasticsearch_host = "elasticsearch")

Add twitter consumer key/access token and secrets to config.py

Edit config.py and modify NLTK tokens required/ignored and twitter feeds you want to mine. NLTK tokens required are keywords which must be in tweet before adding it to Elasticsearch (whitelist). NLTK tokens ignored are keywords which if are found in tweet, it will not be added to Elasticsearch (blacklist).





Examples
Run sentiment.py to create 'stocksight' index in Elasticsearch and start mining and analyzing Tweets using keywords and the stock symbol TSLA

$ python sentiment.py -s TSLA -k 'Elon Musk',Musk,Tesla,SpaceX --debug
Start mining and analyzing Tweets using keywords and the stock symbol TSLA and follow any url links in tweets and performing sentiment analysis on the link web page as well as the tweet

$ python sentiment.py -s TSLA -k 'Elon Musk',Musk,Tesla,SpaceX -l --debug
Start mining and analyzing Tweets from feeds in config using cached user ids from file (if you change any of the twitter feeds in the config file, you need to delete this file and recreate it without -f)

$ python sentiment.py -s TSLA -f twitteruserids.txt --debug
Start mining and analyzing News headlines and following headline links and scraping relevant text on landing page

$ python sentiment.py -s TSLA --followlinks --debug
Run stockprice.py to add stock prices to 'stocksight' index in Elasticsearch

$ python stockprice.py -s TSLA --debug



Kibana
Load 'stocksight' index in Kibana. For index pattern you can use 'stocksight' if you only have the single index or 'stocksight-*', etc. For time-field name you will want to use the date/time field 'date'.

To import the saved exported visualizations/dashboard, go to Kibana, click on management, click on saved objects, click on the import button and import the export.json file.

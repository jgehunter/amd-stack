# amd-stack
The goal of this project is to be able to store certain book data on a number of cryptocurrency assets. Fortunately, there are several exchanges that expose APIs that allow us to consult this data. In our project we will have to:
- Query the APIs periodically
- Store the resulting data
- Visualize the data

For this POC we will focus on a single instrument, BTC/USDT perpetual futures on the binance platform.

# 1. Data Storage
Based on the kind of data the binance API gives us for the BTC/USDT book: a json document containing timestamps of the las transaction and lists of bids and offers, the best database to store this data would be a document store database, we will choose MongoDB.

In order to set up quickly and to be able to eventually host the application anywhere we will get a premade MongoDB docker image, the steps to set it up can be found [here](https://hub.docker.com/_/mongo).

# 2. Data Sourcing
We will have to get data from the binance API, modify it slightly to make it fit into our desired output format and load it into the MongoDB database. In other words, we have to build an ETL procedure. Additionally, we wanto to have it run periodically, for example, every minute. There already exist a number of open source orchestrators, in this case we will use [Apache Airflow](http://airflow.apache.org/).

![image](https://user-images.githubusercontent.com/20443405/118408451-f3ac6800-b685-11eb-8aa4-7b81e3682009.png)
*Apache Airflow Webserver*

We will implement a very simple DAG.

![image](https://user-images.githubusercontent.com/20443405/118408634-b85e6900-b686-11eb-9b18-f67459a2a5d5.png)
*Extract Task*

![image](https://user-images.githubusercontent.com/20443405/118408647-c8764880-b686-11eb-93d8-7d26d282cb30.png)
*Transform Task*

![image](https://user-images.githubusercontent.com/20443405/118408658-d6c46480-b686-11eb-896d-bb36022901f5.png)
*Load Task*

# 3. Data Visualization
In order to validate the POC we will build a quick visualization. The easiest way to build a visualization in our case is to use Dash, we can build a simple Dash server which we can run in a container.

For the POC we will build a chart showing the BTC/USDT TOB mid, bid and ask.

![image](https://user-images.githubusercontent.com/20443405/118410074-f01cdf00-b68d-11eb-8011-147f5169cf5b.png)
*Example of POC Dashboard*

# Conclusions
As we have demonstrated here, a stack may be built entirely on containers, which facilitates for example hosting entire applications on cloud infrastructure. Additionally, the Airflow Orchestrator provides a very useful and simple interface which greatly facilitates ETL tasks. A possible useful next step would be to integrate a ML pipeline into the application.


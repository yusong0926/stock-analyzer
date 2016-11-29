# Stock Analyzer 

A big data platform to process stock data in real time.

Stock data as we know is time series data, I grab data from google finance and every record of data are around 200 bytes. For every record has last trading time,  last trading price , last trading currency, and the stock symbol, apple for example. For time series data, I need a way to quickly consume the data and pull to my system really fast so I chose apache kafka, which is high perfomence messging system, I was able to achieve  200k msg/s, if you count the messges multipy bu the size of the data, you will get 2T data / day, of course normal storage system wouldn't fit in my case. So I chose apache cassandra which is highly scalable peer to peer data storage system. The reason I chose it is also because  cassandra is peer to peer system, no single point failure , every node can go and I can simply grab a new one without hustle. Thirdly, because the data streamed in, I need a way to process data in real time, that is why I chose apache spark. I used spark streaming api to write simple algorithm to proces the data in real time.  Now I have all the way to store and process data, then I need way to visualize the data and present to other people.  So I developed a simple web app using redis, nodejs, and soket io to render the process result in web ui. I use nodejs, becaouse it's easy framework, and I use the socket io to keep websocket connection btw server and client so I can get server to push data to client in real time. So you can see stock change on the ui in real timeA


## Getting Started


### Prerequisites

####Install Dependencies

```
pip install -r requirements.txt
npm install
```

####Install cql to setup database

####Install spark-submit

####Install and Start Containers for Zookeeper, Kafka, Redis and Cassandra 

Run local-setup.sh to setup all the containers.

```
./local-setup.sh
```

###How to Run 

####Start Kafka to grab data from google finanace
```
python simple-data-producer.py AAPL stock-analyzer 127.0.0.1:9092
```
###Setup the database

```
CREATE KEYSPACE "stock" WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1} AND durable_writes = 'true';
USE stock;
CREATE TABLE stock (stock_symbol text, trade_time timestamp, trade_price float, PRIMARY KEY (stock_symbol,trade_time));
```
####Start Casandra to store the data

```
python data-storage.py stock-analyzer 127.0.0.1:9092 stock stock 127.0.0.1
```

####Start Spark to process the data from Kafka and send back to Kafka with another topic
```
spark-submit --jars spark-streaming-kafka-assembly_2.10-1.6.2.jar stream-processing.py stock-analyzer average-stock-price 127.0.0.1:9092
```
####Start Redis to cache the data
```
python redis-publisher.py average-stock-price 127.0.0.1:9092 average-stock-price 127.0.0.1 6379
```
####Start NodeJs to read data from Redis and visualize them to frontend
```
node index.js --port=3000 --redis_host=192.168.99.100 --redis_port=6379 --subscribe_topic=average-stock-price
```
####Visualize the Data
http://localhost:3000


## License

This project is licensed under the MIT License.

## Acknowledgments




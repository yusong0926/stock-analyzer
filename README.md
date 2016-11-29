Synopsis

A big data platform to process stock data in real time. Stock data as we know is time series data, I grab data from yahoo finance and every record of data are around 200 bytes. For every record has last trading time,  last trading price , last trading currency, and the stock symbol, apple for example. For time series data, I need a way to quickly consume the data and pull to my system really fast so I chose apache kafka, which is high perfomence messging system, I was able to achieve  200k msg/s, if you count the messges multipy bu the size of the data, you will get 2T data / day, of course normal storage system wouldn't fit in my case. So I chose apache cassandra which is highly scalable peer to peer data storage system. The reason I chose it is also because  cassandra is peer to peer system, no single point failure , every node can go and I can simply grab a new one without hustle. Thirdly, because the data streamed in, I need a way to process data in real time, that is why I chose apache spark. I used spark streaming api to write simple algorithm to proces the data in real time.  Now I have all the way to store and process data, then I need way to visualize the data and present to other people.  So I developed a simple web app using redis, nodejs, and soket io to render the process result in web ui. I use nodejs, becaouse it's easy framework, and I use the socket io to keep websocket connection btw server and client so I can get server to push data to client in real time. So you can see stock change on the ui in real timeA


Installation



Dependencies




How to Run





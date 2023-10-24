

<img src="img\background.png" alt="background"  />

1. **Download**

   1.1 Download on Linux

   1.2 Download on windows

   1.3 GUI (Redisinsight)

2. **Basic command/data types**

   2.1 Redis Key Commands

   2.2 Redis Data Type

   ​       2.2.1 Strings

   ​       2.2.2 Hash

   ​       2.2.3 Lists

   ​       2.2.4 Set

3. **Introduction to Extended Features**

   3.1 redis pub/sub

   3.2 Redis Persistence

   3.3 Redis master-slave replication 

   3.4 Redis Geospacial

   **Reference**

**1 Download**

***1.1 Download on Linux***

Downloading Redis on a Linux system is extremely simple. You just need to run two installation commands (there might be slight variations for different Linux distributions, but the overall process is similar). Here is the official tutorial:

You can install recent stable versions of Redis from the official `packages.redis.io` APT repository.

Prerequisites

If you're running a very minimal distribution (such as a Docker container) you may need to install `lsb-release`, `curl` and `gpg` first:

```
sudo apt install lsb-release curl gpg
```

Add the repository to the `apt` index, update it, and then install:

```bash
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

sudo apt-get update
sudo apt-get install redis[1]
```

When the operating system is based on RHEL or CentOS (both are Linux systems), the process is even simpler because Redis can typically be installed through the EPEL repository：

```
sudo yum install epel-release
```

(To install the EPEL repository)

```
sudo yum install redis
```

(To install Redis directly)

<img src="img\EPEL.png">



redis1

Start the server first, then try to connect:

```
redis-server(start the server)
redis-cli(connect to redis)
```

<img src="img\server1.png" style="zoom: 50%;" />

<img src="img\server2.png" style="zoom:50%;" />

When "ping" it returns "pong", you are successfully start the redis server.

***1.2 Download on windows：***

I still recommend using a Linux system to download and use Redis, as using a Windows system may limit you to older versions of Redis (the latest versions are often available for Linux), and official Redis Windows installation packages are less common. However, there are workarounds to use Redis on a Windows system. Here are two methods to achieve that：

**The first** method is to directly download a Redis installation package (not from the official website) with a version of 5.0+.

<img src="img\redis-windows (1).png" style="zoom: 33%;" />

<img src="img\redis-windows1 (2).png" alt="redis-windows1 (2)" style="zoom: 50%;" />

To use Redis after setting the installation location and running the port, you can simply click "Next" until the installation is complete.

To start Redis, you'll need to find the **redis-server** file in the folder where you installed Redis, and then click on it. This step is essential for starting the Redis service.

<img src="img\error1.png" style="zoom: 67%;" />

<img src="img\start.png" style="zoom: 50%;" />

Once Redis is started, you should be able to use it as normal.

<img src="img\start2.png" style="zoom: 33%;" />

![](img\successful.png)

**The second** method is to use Docker to download Redis and create a container to enable Windows to use the Redis service:

First, download the Redis image. Use the following command from the command prompt (cmd window):

```
docker pull redis
```

Next, start the Docker service and then start the Redis container to use it. In the example, port 6379 is used, but you can also choose your own port:

```
docker run -d --name my-redis-container -p 6379:6379 redis
```

![](img\docker1.png)

<img src="img\docker2.png" style="zoom: 33%;" />

You can see that Redis is already in a running state. Next, you can follow all the steps outlined in the first method.

**1.3 Redisinsight**

RedisInsight can visualize and optimize Redis data. It is a powerful desktop manager that offers an intuitive and efficient UI for Redis and supports a range of operations, including CRUD, in a feature-rich client.

![](img\redisinsight2.png)

In the website's graphical interface, you can select the operating system to download RedisInsight. When using it, as mentioned in step 1.2, you should first start the Redis service and then open RedisInsight to establish a connection.

<img src="img\redisinsight3.png" style="zoom: 33%;" />

Click "Add Redis Database" to establish a connection. Upon successful connection, you should receive a notification and see the operation window displayed.

<img src="img\startinsight.png" style="zoom:33%;" />

This tool has numerous advantages, including but not limited to:

- Browse, filter, and visualize Redis keys, perform CRUD operations, or delete keys in bulk. 
- Display data in pretty-print JSON, hexadecimal, MessagePack, and many other formats. Use friendly keyboard navigation. 
- Use the Tree view to group data and enhance the navigation.[2]

Personally, the code suggestion feature alone has already saved me a lot of time.

<img src="img\command.png" style="zoom:33%;" />

Due to space limitations, in summary, using this tool will greatly simplify your Redis usage. Download it and give it a try!

**2** **Basic command**

**2.1 Redis Key Commands**

*Redis offers a wide variety of basic commands. Here, I've selected some commonly used ones for explanation:*

1. `SET key value`: Sets a key in Redis with a specified value.
2. `GET key`: Retrieves data from the Redis database. If the key does not exist, it returns null.
3. `KEYS *`: Searches for all key values in the Redis database.
4. `DEL key`: Deletes the content of a specified key.
5. `DUMP key`: Serializes the given key and returns the serialized value.
6. `EXISTS key`: Checks if the specified key exists. It returns 1 if it exists and 0 if it doesn't.
7. `EXPIRE key seconds`: Sets an expiration time in seconds for a given key. If the key is reset with a new value, such as with `SET key value`, the expiration time will be invalidated.

**2.2 Redis Data Types**

2.2.1 Strings

The "String" data type is the most basic. It follows a key-value structure, where each key corresponds to a value, and the maximum size for a value is 512MB.

Grammar:

```
redis 127.0.0.1:6379> COMMAND KEY_NAME
```

Example:

```
redis 127.0.0.1:6379> SET w3ckey redis
OK
redis 127.0.0.1:6379> GET w3ckey
"redis"[3]
```

In this example, the SET and GET commands are used. The key is "w3ckey," and its corresponding value is the string "redis."

2.2.2 Hash

Hash is a collection of key-value pairs, where the values can be thought of as a map. Hashes are particularly suitable for storing objects, and each hash can store more than 4 billion key-value pairs.

```
redis 127.0.0.1:6379> HMSET w3ckey name "redis tutorial" description "redis basic commands for caching" likes 20 visitors 23000
OK
redis 127.0.0.1:6379> HGETALL w3ckey
1) "name"
2) "redis tutorial"
3) "description"
4) "redis basic commands for caching"
5) "likes"
6) "20"
7) "visitors"
8) "23000"[4]
```

In this example, various pieces of information about Redis, such as "name," "description," and "likes," are stored in a hash named "w3ckey." 

2.2.3 Lists

Lists are simple string lists that are sorted in the order of insertion. You can add data to the head or tail of a list, and each list can store approximately over 4 billion elements.

```
redis 127.0.0.1:6379> LPUSH w3ckey redis
(integer) 1
redis 127.0.0.1:6379> LPUSH w3ckey mongodb
(integer) 2
redis 127.0.0.1:6379> LPUSH w3ckey mysql
(integer) 3
redis 127.0.0.1:6379> LRANGE w3ckey 0 10
1) "mysql"
2) "mongodb"
3) "redis"[5]
```

In this example, **LPUSH** is used to insert three values into a list named "w3ckey."

2.2.4 Set

Redis Sets are unordered collections of string elements with no duplicate values. They are implemented using hash tables, and each set can store up to approximately 4 billion members.

Example

```
redisCopy coderedis 127.0.0.1:6379> SADD w3ckey redis
(integer) 1
redis 127.0.0.1:6379> SADD w3ckey mongodb
(integer) 1
redis 127.0.0.1:6379> SADD w3ckey mysql
(integer) 1
redis 127.0.0.1:6379> SADD w3ckey mysql
(integer) 0
redis 127.0.0.1:6379> SMEMBERS w3ckey
1) "mysql"
2) "mongodb"
3) "redis"[6]
```

In this example, the **SADD** command inserts three elements into a set named "w3ckey." 

**3** **Introduction to Extended Features**

After studying the basic commands, we have also delved into some of the extended features of Redis. Now, we have chosen to showcase a few of the functionalities that we find interesting.

**3.1  redis pub/sub**

Redis has a unique publish/subscribe (Pub/Sub) mechanism that allows one or more clients to subscribe to channels and receive messages published to these channels. This is particularly useful for certain applications, such as those that require real-time communication and event-driven behavior.

Here's how you can demonstrate this functionality:

1. Open two Redis command-line clients (for this demonstration, we'll use Windows with a virtualized Ubuntu environment).
2. In the first Redis command-line client, enter `SUBSCRIBE runoobChat`. This command subscribes to the "runoobChat" channel.
3. In the second Redis command-line client, enter `PUBLISH runoobChat "Redis PUBLISH test"`. This command publishes a message to the "runoobChat" channel. At this point, the first Redis command-line client will receive the test message sent by the second Redis command-line client.[7]

![](img\pub.png)

This technology can have practical applications in real life. Let's consider a scenario:

Imagine a business situation where, after customers place orders and make payments on a website, it is necessary to notify the inventory service for handling shipments. Later, if new services are introduced, such as a loyalty points program, which needs access to the results of order payments to increase user points, the situation becomes more complex. If only two services require access to order payment results, that's manageable, and software modifications can be made quickly. However, as the business continues to evolve, more and more new services may require access to order payment results. The consequence is that simply invoking a specific function becomes increasingly inefficient. When you make modifications to one aspect, other functionalities related to the same business logic also require adjustments. Furthermore, if there is an issue with one aspect of the business logic, it can potentially affect all functionalities.

![](img\pay.png)

Redis provides a message mechanism based on the "publish/subscribe" pattern. In this mode, message publishers and subscribers do not need to communicate directly with each other.

![](img\pubsub2.png)[8]

As shown in the diagram above, message publishers only need to publish messages to a specified channel, and every client subscribed to that channel will receive the message.

Using the Redis publish/subscribe mechanism, in the context of the business scenario mentioned earlier, the order payment service only needs to send a message to the "payment results" channel. Other downstream services can subscribe to the "payment results" channel, receive the relevant messages, and then proceed with their respective business processing. 

**3.2 Redis Persistence**
Redis is a type of in-memory database that stores all data in memory. In contrast to traditional relational databases like MySQL, Oracle, and SQL Server, which store data directly on disk, Redis offers extremely high read and write efficiency. However, there is a significant drawback to storing data in memory, namely that if there is a power failure or system crash, all the contents in the in-memory database will be lost.

To address this drawback, Redis provides a persistence mechanism that allows data in memory to be saved to disk files and uses backup files to recover data. The primary purpose of this mechanism is to ensure data persistence.

<img src="img\last.png" style="zoom: 50%;" />

You can find detailed explanations and configuration options in the Redis configuration file:

 by default Redis will save the DB:
* After 3600 seconds (an hour) if at least 1 change was performed

* After 300 seconds (5 minutes) if at least 100 changes were performed

* After 60 seconds if at least 10000 changes were performed

Based on the description above, you can customize the frequency of taking snapshots at fixed intervals, such as taking a snapshot every 3600 seconds:

![](img\shot.png)

Of course, you can manually trigger a snapshot using the "save" command:

![](img\dumb.png)

"dumb.rdb" is the snapshot file.

**3.3 Redis master-slave replication **

Master-slave replication involves replicating data from one master server to multiple slave servers to improve availability and data backup.

To configure this, you need to adjust the configuration file. In your demonstration, you're using port 6380 on a virtual machine running Ubuntu in a Windows environment.

![](img\conf1.png)



![](img\port.png)

change port to 6380

![](img\pidfile.png)

Change the PID file name of the slave node to "redis_6380" to distinguish it from the master node.

![](img\dbfile.png)

Change the slave node's dbfilename so that when the SAVE command is executed, the data on the slave node will be saved in this file.

![](img\replicaof.png)

Specify the IP address and port number of the master node.

![](img\copy.png)

As you can see, when the data in the master node at 6379 changes, the data in the slave node also changes in the same way, as depicted in the graph, showing the addition of two values, "haha" and "key."

Many small and medium-sized enterprises do not yet use Redis clusters, but they typically implement master-slave replication. With master-slave replication, when the master server fails, operations can promote a slave server to take over, allowing services to continue running. Otherwise, if the master server needs data recovery and a restart, it could lead to a lengthy downtime, impacting the continuity of online services.

![](img\copy2.jpg)[9]

**3.4 Redis Geospacial**

In today's booming era of mobile internet, Location-Based Services (LBS) applications are proliferating. For relatively simple applications with small data scales, we can use database queries to find the nearest city. We can assume the user's current longitude is `u_longitude` and latitude is `u_latitude`. In such cases, database queries are an effective solution. However, when dealing with massive data scales, as in systems that cover restaurants across the entire UK like Just Eat, the performance issues of SQL queries become evident.
The reason behind this is that the longitude and latitude fields in the database may involve functions that prevent the full utilization of indexes for query optimization, thus diminishing query performance. To achieve high-performance queries for geospatial data like this, Redis introduces the Geo data structure. With Geo, it becomes effortless to search for nearby locations within vast datasets.

![](img\GEO.png)

For example, here I added the city of Beijing based on longitude and latitude:

![](img\GEO2.png)

You can continue to add data after the first set of longitude, latitude, and city, allowing you to store multiple sets of geographical coordinates and associated cities simultaneously:

<img src="img\city.png" style="zoom: 50%;" />

As you can see, we now have data for five cities, and we can perform a series of operations on them:

![](img\GEO3.png)

You can query the longitude and latitude of a particular city, as shown in the example with Beijing's coordinates. When you have two or more city data entries, you can calculate the straight-line distance between them. In the example, we calculated the straight-line distance between Shanghai and Beijing, which is 1,067,597.9668 meters by default. You can, of course, change the unit by adding "KM" to the query to get the result in kilometers.
But the utility of this feature goes beyond that. You can even search for other cities around a particular city, using it as a central point:

![](img\GEO4.png)

In the example, a query was made for cities around Shanghai with a radius of 300 kilometers, and the result returned Hangzhou and Shanghai.



**Summary**

In summary, this Redis tutorial has covered the basics of downloading Redis, fundamental commands, practical applications, and delved into some advanced features. It is suitable for beginners with little to no prior knowledge of Redis, providing a comprehensive introduction and learning resource.



**Reference**

[1]Install on Ubuntu/Debian

Available at: https://redis.io/docs/getting-started/installation/install-redis-on-linux/

[2]Get a visual view of Redis data

Available at:https://redis.com/redis-enterprise/redis-insight/#insight-form

[3]Redis (String)

Available at:https://www.redis.net.cn/tutorial/3508.html

[4]Redis (Hash)

Available at:https://www.redis.net.cn/tutorial/3509.html

[5]Redis (List)

Available at:https://www.redis.net.cn/tutorial/3510.html

[6]Redis (Set)

Available at:https://www.redis.net.cn/tutorial/3511.html

[7] [8]Redis 发布订阅 (pub/sub)

Available at:https://www.runoob.com/redis/redis-pub-sub.html

[9]redis主从复制RCE]  *L0nm4r 4 October  2021*

Available at:(https://lonmar.cn/2021/04/10/redis(主从复制RCE/)


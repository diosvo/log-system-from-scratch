## DEPLOYMENT

### [Master Server]: <code style="color: red">Coordinator/Overlord + ZooKeeper</code>

**Step 1**

<hr/>

Start by downloading the Druid distribution.

```ruby
wget https://dlcdn.apache.org/druid/25.0.0/apache-druid-25.0.0-bin.tar.gz
```

**Step 2**

<hr/>

Unzip the downloaded file.

```ruby
tar -xzf apache-druid-25.0.0-bin.tar.gz
```

**Step 3**

<hr/>

Let's review the changes that we will cause by using the quickstart configuration.

```ruby
diff /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/coordinator-overlord/jvm.config /root/apache-druid-25.0.0/conf/druid/cluster/master/coordinator-overlord/jvm.config
```

These changes show Java command options that will decrease the JVM heap size from 15GB to 256MB.

```ruby
2,3c2,3
< -Xms256m
< -Xmx256m
---
> -Xms15g
> -Xmx15g
```

**Step 4**

<hr/>

Now, let's copy this configuration file into our cluster configuration.

```ruby
cp /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/coordinator-overlord/jvm.config /root/apache-druid-25.0.0/conf/druid/cluster/master/coordinator-overlord
```

We need to configure the servers so that they can discover each other.

<details>

<summary>Want to know how the discovery process works? Click here.</summary>

<hr/>

1. On each server, in the common configuration file, we set _druid.zk.service.host_ to tell the server how to contact ZooKeeper
2. In this same common configuration file we comment out _druid.host_, which forces the server to automatically determine its host name
3. Each server contacts ZooKeeper and registers who and where they are
4. Finally, ZooKeeper tells each of the servers how to contact other processes within the cluster

</details>

**Step 5**

<hr/>

Open the file: _/root/apache-druid-25.0.0/conf/druid/cluster/\_common/common.runtime.properties_.

**Step 6**

<hr/>

Search for line _druid.host=localhost_ and comment the line out using a pound-sign (#).

Also, search for _druid.zk.service.host_ and change its value (which is currently localhost) to

```ruby
master-server:2181
```

Now, we can launch the master server

```ruby
nohup /root/apache-druid-25.0.0/bin/start-cluster-master-with-zk-server > /root/apache-druid-25.0.0/log.out 2> /root/apache-druid-25.0.0/log.err < /dev/null & disown
```

**Step 7**

<hr/>

Check that ZooKeeper and the coordinator processes are running (you should see two Java processes near the bottom of the list of processes).

```ruby
ps -ef
```

**Step 8**

<hr/>

We can make sure that the running master server is using our configuration changes by querying its status API. The following command queries the API for druid.zk.service.host, which is the setting we changed (you may have to wait a minute for the web server to initialize).

```ruby
curl master-server:8081/status/properties | jq | grep druid.zk.service.host
```

**Step 9**

<hr/>

We can also query the API to make sure that _druid.host_ is not set (because we commented it out). The following command should not find _druid.hosts_ in the master-server properties.

```ruby
curl master-server:8081/status/properties | jq | grep druid.zk.service.host
```

### [Data Server]: <code style="color: red">Historical + MiddleManager</code>

**Step 1**

<hr/>

Start by downloading the Druid distribution.

```ruby
wget https://dlcdn.apache.org/druid/25.0.0/apache-druid-25.0.0-bin.tar.gz
```

**Step 2**

<hr/>

Unzip the downloaded file.

```ruby
tar -xzf apache-druid-25.0.0-bin.tar.gz
```

**Step 3**

<hr/>

Here the changes we will make to the Druid historical configuration.

```ruby
diff /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/historical/runtime.properties /root/apache-druid-25.0.0/conf/druid/cluster/data/historical/runtime.properties
```

The results of this command show that we are reducing resources such as number of threads for servicing HTTP requests and general processing, buffer sizes and cache sizes. Learn more [here](https://druid.apache.org/docs/latest/configuration/index.html#historical).

**Step 4**

<hr/>

Next, review the changes to the historical's JVM.

```ruby
diff /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/historical/jvm.config /root/apache-druid-25.0.0/conf/druid/cluster/data/historical/jvm.config
```

The results here show the changes in the historical JVM options to reduce the heap size, the direct memory mapping size and also changing the garbage collection algorithm.

**Step 5**

<hr/>

Review the changes to the middle manager's configuration.

```ruby
diff /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/middleManager/runtime.properties /root/apache-druid-25.0.0/conf/druid/cluster/data/middleManager/runtime.properties
```

We reduce many resources used by the middle manager. Learn more [here](https://druid.apache.org/docs/latest/configuration/index.html#middlemanager-configuration).

**Step 6**

<hr/>

Finally, review the changes to the middle manager's JVM configuration.

```ruby
diff /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/middleManager/jvm.config /root/apache-druid-25.0.0/conf/druid/cluster/data/middleManager/jvm.config
```

Similar to the historical process, we see we are reducing the middle manager's JVM heap size and garbage collection algorithm.

**Step 7**

<hr/>

Let's copy the historical files into our cluster configuration.

```ruby
cp /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/historical/* /root/apache-druid-25.0.0/conf/druid/cluster/data/historical
```

**Step 8**

<hr/>

Let's also copy the middle manager files into our cluster configuration.

```ruby
cp /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/middleManager/* /root/apache-druid-25.0.0/conf/druid/cluster/data/middleManager
```

**Step 9**

<hr/>

Now, let's copy the _common.runtime.properties_ file we edited in the first challenge to this server. Remember, this file tells the server **_how to contact ZooKeeper._**

```ruby
scp -o StrictHostKeyChecking=no master-server:/root/apache-druid-25.0.0/conf/druid/cluster/_common/common.runtime.properties /root/apache-druid-25.0.0/conf/druid/cluster/_common/common.runtime.properties
```

**Step 10**

<hr/>

We are set to launch the first data server.

```ruby
scp -o StrictHostKeyChecking=no master-server:/root/apache-druid-25.0.0/conf/druid/cluster/_common/common.runtime.properties /root/apache-druid-25.0.0/conf/druid/cluster/_common/common.runtime.properties
```

**Step 11**

<hr/>

Check that the historical and middleManager processes are running.

```ruby
ps -ef | grep "openjdk\-[8-8]" | awk 'NF{print $NF}'
```

### [Query Server]: <code style="color: red">Broker + Router</code>

**Step 1**

<hr/>

Start by downloading the Druid distribution.

```ruby
wget https://dlcdn.apache.org/druid/25.0.0/apache-druid-25.0.0-bin.tar.gz
```

**Step 2**

<hr/>

Unzip the downloaded file.

```ruby
tar -xzf apache-druid-25.0.0-bin.tar.gz
```

**Step 3**

<hr/>

We will change the broker's configuration.

```ruby
diff /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/broker/runtime.properties /root/apache-druid-25.0.0/conf/druid/cluster/query/broker/runtime.properties
```

Here, we limit the resources used by the broker. Learn more [here](https://druid.apache.org/docs/latest/configuration/index.html#broker).

**Step 4**

<hr/>

We will change the broker's JVM configuration too.

```ruby
diff /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/broker/jvm.config /root/apache-druid-25.0.0/conf/druid/cluster/query/broker/jvm.config
```

**Step 5**

<hr/>

Here are the changes to the router configuration.

```ruby
diff /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/router/runtime.properties /root/apache-druid-25.0.0/conf/druid/cluster/query/router/runtime.properties
```

One more time, we limit the resources consumed by the router. Learn more [here](https://druid.apache.org/docs/latest/configuration/index.html#router).

**Step 6**

<hr/>

Here are the changes to the router's JVM configuration.

```ruby
diff /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/router/jvm.config /root/apache-druid-25.0.0/conf/druid/cluster/query/router/jvm.config
```

And finally, we reduce the heap size of the router.

**Step 7**

<hr/>

Let's copy the Broker files into our cluster configuration.

```ruby
cp /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/broker/* /root/apache-druid-25.0.0/conf/druid/cluster/query/broker
```

**Step 8**

<hr/>

Let's copy the Router files into our cluster configuration.

```ruby
cp /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/router/* /root/apache-druid-25.0.0/conf/druid/cluster/query/router
```

**Step 9**

<hr/>

One last time, let's copy the _common.runtime.properties_ file we edited in the first challenge to this server. Remember, this file tells the server **_how to contact ZooKeeper._**

```ruby
cp /root/apache-druid-25.0.0/conf/druid/single-server/nano-quickstart/router/* /root/apache-druid-25.0.0/conf/druid/cluster/query/router
```

**Step 10**

<hr/>

We are set to launch the first data server.

```ruby
nohup /root/apache-druid-25.0.0/bin/start-cluster-query-server > /root/apache-druid-25.0.0/log.out 2> /root/apache-druid-25.0.0/log.err < /dev/null & disown
```

**Step 11**

<hr/>

Check that the broker and router processes are running.

```ruby
ps -ef | grep "openjdk\-[8-8]" | awk 'NF{print $NF}'
```

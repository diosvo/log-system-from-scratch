## INSTALLATION

**Step 1**

<hr/>

First, look at the Java version.

```
java -version
```

**Step 2**

<hr/>

Next, check that JAVA_HOME is set correctly.

```ruby
echo $JAVA_HOME
```

**Step 3**

<hr/>

Download Druid (here's a list of mirror sites if you want to find one close to your location). Depending on the download speed, this may take a few minutes.

```ruby
wget https://dlcdn.apache.org/druid/25.0.0/apache-druid-25.0.0-bin.tar.gz
```

**Step 4**

<hr/>

Unzip the downloaded file.

```ruby
tar -xzf apache-druid-25.0.0-bin.tar.gz
```

**Step 5**

<hr/>

We'll start Druid with nohup so that the Druid processes persist between Instruqt challenges.

```ruby
nohup /root/apache-druid-25.0.0/bin/start-nano-quickstart > /root/apache-druid-25.0.0/log.out 2> /root/apache-druid-25.0.0/log.err < /dev/null & disown
```

**Step 6**

<hr/>

You can see the various Druid processes with this command.

```ruby
ps -ef | grep "Main server [A-Za-z]*$" | awk 'NF{print $NF}'
```

See the result:

```
coordinator
broker
router
historical
middleManager
```

_**NOTE**: In this quickstart, the coordinator and overlord run in the same process named coordinator. Also, we aren't showing the ZooKeeper process in the list._

<hr/>

## RELATED

[1] 📍 you're here

[2] [Ingest sample data](2-ingestion.md)

[3] [Perform queries on the sample data](3-querying.md)

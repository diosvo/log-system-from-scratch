**Step 1**

<hr/>

We need to modify the Druid configuration to enable metrics. This lab environment uses the nano-quickstart configuration. open _common.runtime.properties_ file in the conf folder.

**Step 2**

<hr/>

Remove the default JVM monitor, and configure the emitter to use the log files by replacing the following lines.

```ruby
druid.monitoring.monitors=[]
druid.emitter=logging
```

**Step 3**

<hr/>

Let's start Druid and wait for Druid to initialize using the following commands:

```ruby
nohup /root/apache-druid-24.0.2/bin/start-nano-quickstart \
  > /root/log.out 2> /root/log.err \
  < /dev/null & disown
while [ $(curl localhost:8888/ 2>&1 >/dev/null | grep Fail | wc -w) -gt 0 ]
do
  echo "Waiting for Druid to initialize..."
  sleep 3
done
```

**Step 4**

<hr/>

Let's wait for Druid to emit some metrics. Remember, By default Druid emits metrics every minute.

```ruby
while [ $(grep -C 2 'org.apache.druid.java.util.emitter.core.LoggingEmitter - [{]' apache-druid-24.0.2/log/broker.log | wc -l) -eq 0 ]
do
  echo "Waiting for Druid to emit metrics..."
  sleep 5
done
```

**Step 5**

<hr/>

We can investigate the Broker's metrics in table format by using the following command.

```ruby
grep 'org.apache.druid.java.util.emitter.core.LoggingEmitter - [{]' apache-druid-24.0.2/log/broker.log \
  | awk 'BEGIN {FS = " - "} ; {print $2}' \
  | jq 'select(has("type") | not) | select(.feed == "metrics") | "\(.timestamp) \(.metric) \(.value)"' \
  | awk '{printf "%17.17s - %-30s %s\n", $1, $2, $3}'
```

We see Jetty and _Avatica_ messages. If you’re familiar with Druid, you’ll know that [_Jetty_](https://www.eclipse.org/jetty/) is what Druid uses for all its web services, and [_Avatica_](https://calcite.apache.org/avatica/) is a JDBC driver from Apache Calcite, which is used for SQL query parsing and planning.

These metrics are the basic metrics that the Broker always emits.

```ruby
"2023-06-23T08:39 - avatica/server/AvaticaProtobufHandler/Handler/RequestTimings 0"
"2023-06-23T08:39 - avatica/server/AvaticaJsonHandler/Handler/RequestTimings 0"
"2023-06-23T08:39 - avatica/remote/ProtobufHandler/Handler/Serialization 0"
"2023-06-23T08:39 - avatica/remote/JsonHandler/Handler/Serialization 0"
"2023-06-23T08:39 - jetty/numOpenConnections       0"
"2023-06-23T08:39 - jetty/threadPool/total         14"
"2023-06-23T08:39 - jetty/threadPool/idle          12"
"2023-06-23T08:39 - jetty/threadPool/isLowOnThreads 0"
"2023-06-23T08:39 - jetty/threadPool/min           14"
"2023-06-23T08:39 - jetty/threadPool/max           14"
"2023-06-23T08:39 - jetty/threadPool/queueSize     0"
"2023-06-23T08:39 - jetty/threadPool/busy          2"
```

**Step 6**

<hr/>

Let's ingest some sample data.

```ruby
TASK_ID=$(curl -XPOST \
  -H'Content-Type: application/json'   \
  http://localhost:8888/druid/v2/sql/task \
  -d @/root/ingestion.json \
  | tee >(jq > /dev/tty) \
  | jq -r '.taskId')

  while [ $(curl  -H'Content-Type: application/json' http://localhost:8888/druid/indexer/v1/task/$TASK_ID/reports  2> /dev/null \
    | jq .multiStageQuery.payload.status.status 2> /dev/null \
    | grep SUCCESS | wc -w) -eq 0 ]; \
    do
      echo "Waiting for ingestion to complete..."
       sleep 3
    done
  echo "Ingestion completing"
```

**Step 7**

<hr/>

Also, we can perform a query so we can see some query-related metrics.

```ruby
curl -s -X 'POST' \
  -H 'Content-Type:application/json' \
  -d @/root/query.json http://localhost:8888/druid/v2/sql \
  | jq
```

**Step 8**

<hr/>

Look again to see the query metrics.

```ruby
echo "Waiting one minute to ensure that Druid emits the query metrics..."
sleep 60
grep 'org.apache.druid.java.util.emitter.core.LoggingEmitter - [{]' apache-druid-24.0.2/log/broker.log \
  | awk 'BEGIN {FS = " - "} ; {print $2}' \
  | jq 'select(has("type") | not) | select(.feed == "metrics") | "\(.timestamp) \(.metric) \(.value)"' \
  | awk '{printf "%17.17s - %-30s %s\n", $1, $2, $3}'
```

Now, you will notice _sqlQuery_ metrics reflecting the query we performed.

<details>

<summary>What do the sqlMetrics tell us?</summary>

There are three SQL related metrics:

- time
- planningTimeMs
- bytes

Read more [here](https://druid.apache.org/docs/latest/operations/metrics.html#sql-metrics).

</details>

**NOTE**: _In the [documentation](https://druid.apache.org/docs/latest/operations/metrics.html#sql-metrics), you will notice that each metric has a list of "Dimensions". These "Dimensions" are the fields within the JSON metrics record. In this step, the jq command in the pipeline filters out all but the timestamp, label (i.e., metric) and value fields. The other fields maybe helpful for deeper dives into metrics, but in this exercise, we just want to get a sense of what changes when we add monitors._

Now, let's add the JVM metrics that were part of Druid's default configuration.

**Step 9**

<hr/>

Let's edit the _common.runtime.properties_ file again and add in the JVM monitor by replacing the following line.

```ruby
druid.monitoring.monitors=["org.apache.druid.java.util.metrics.JvmMonitor"]
```

**Step 10**

Restart the cluster so the configuration change will take effect (run this command in the Shell tab).

```ruby
kill $(ps -ef | grep 'perl /root' | awk 'NF{print $2}' | head -n 1)
while [ $(curl localhost:8888/ 2>&1 >/dev/null | grep Fail | wc -w) -eq 0 ]; do echo "Waiting for cluster to terminate..."; sleep 3; done
nohup /root/apache-druid-24.0.2/bin/start-nano-quickstart > /root/log.out 2> /root/log.err < /dev/null & disown
while [ $(curl localhost:8888/ 2>&1 >/dev/null | grep Fail | wc -w) -gt 0 ]; do echo "Waiting for cluster to initialize..."; sleep 3; done; sleep 5
```

**Step 11**

Finally, wait a minute (for the Broker to emit metrics) and then let's look at the Broker's metrics again to see what has changed.

```ruby
sleep 60
grep 'org.apache.druid.java.util.emitter.core.LoggingEmitter - [{]' apache-druid-24.0.2/log/broker.log \
  | awk 'BEGIN {FS = " - "} ; {print $2}' \
  | jq 'select(has("type") | not) | select(.feed == "metrics") | "\(.timestamp) \(.metric) \(.value)"' \
  | awk '{printf "%17.17s - %-30s %s\n", $1, $2, $3}'
```

We see that adding the JVM monitor causes the Broker to emit metrics specific to the JVM.

<details>

<summary>What metrics does JvmMonitor emit?</summary>

The JVM metrics include metrics relating to:

- Thread pools
- Buffer pools
- Memory utilization
- Garbage collection

Read more [here](https://druid.apache.org/docs/latest/operations/metrics.html#jvm).

</details>

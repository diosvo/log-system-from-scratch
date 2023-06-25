**Step 1**

<hr/>

We need to modify the Druid configuration to enable metrics. This lab environment uses the nano-quickstart configuration. open _common.runtime.properties_ file in the conf folder.

**Step 2**

<hr/>

Enable emitting metrics to the log files by changing druid.emitter (in the Monitoring section) to logging.

```ruby
druid.emitter=logging
```

<details>

<summary>What do the three configuration settings do?</summary>

The first setting, _**druid.monitoring.monitors**_, determines which metrics Druid emits. The default only emits JVM metrics.

**NOTE**: _Some monitors may only apply to specific Druid process and should be configured in the process-specific configuration files._

The second setting, _**druid.emitter**_, determines the target of the metrics output. The default is noop, which specifies no metrics output, but here we are sending metrics to the log files. Other emitters target different outputs, e.g., an HTTP server.

The third setting, _**druid.emitter.logging.logLevel**_, tells Druid what logging security level to use when emitting metrics.

Read more [here](https://druid.apache.org/docs/latest/configuration/index.html#enabling-metrics).

</details>

**Step 3**

<hr/>

Use the following commands to launch Druid and wait for Druid to initialize.

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

By default, Druid emits metrics every minute (Read about _druid.monitoring.emissionPeriod_ [here](https://druid.apache.org/docs/latest/configuration/index.html#enabling-metrics)). So if we look in the logs now, we will not see metrics yet.

Let's wait for Druid to emit some metrics.

```ruby
while [ $(grep -C 2 'org.apache.druid.java.util.emitter.core.LoggingEmitter - [{]' apache-druid-24.0.2/log/broker.log | wc -l) -eq 0 ]
do
  echo "Waiting for Druid to emit metrics..."
  sleep 5
done
```

**Step 5**

<hr/>

Druid emits the metrics into the processes' log files. The following commands show us an example of metrics in the context of the log file.

```ruby
grep -C 2 'org.apache.druid.java.util.emitter.core.LoggingEmitter - [{]' apache-druid-24.0.2/log/broker.log \
  | head -n 5
```

**NOTE**: _In this lab we will look at metrics only in the Broker log. However, Druid emits similar JVM metrics to all the processes' log files. But remember, unlike JVM metrics, there are some metrics that we can only gather from specific processes._

**Step 6**

<hr/>

We can extract just the JSON from the metric and format the JSON by using the following commands.

```ruby
grep 'org.apache.druid.java.util.emitter.core.LoggingEmitter - [{]' apache-druid-24.0.2/log/broker.log \
  | head -n 1 \
  | awk 'BEGIN {FS = " - "} ; {print $2}' \
  | jq 'select(has("type") | not) | select(.feed == "metrics")'
```

<details>

<summary>Why do we want to extract only the JSON?</summary>

As we have previously seen, the metrics messages are scattered among the other log messages. As a result, the metrics contain the normal log message boiler plate. But, when we are interested in zooming in on the metrics, this boiler plate is not helpful. So, we extract the JSON portion of the log message, which focuses on the metrics information that we care about.

</details>

**Step 7**

<hr/>

In the previous step, we saw an example of a single JSON metric. Let’s use a similar command line filter to extract all the metrics from the log file.

We can extract the timestamps, the metric names, and associated values for all metrics (in tabular format) using the following commands.

```ruby
grep 'org.apache.druid.java.util.emitter.core.LoggingEmitter - [{]' apache-druid-24.0.2/log/broker.log \
  | awk 'BEGIN {FS = " - "} ; {print $2}' \
  | jq 'select(has("type") | not) | select(.feed == "metrics") | "\(.timestamp) \(.metric) \(.value)"' \
  | awk '{printf "%17.17s - %-30s %s\n", $1, $2, $3}'
```

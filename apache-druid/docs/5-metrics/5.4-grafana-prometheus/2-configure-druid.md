## CONFIGURE DRUID

Let's configure Druid to emit metrics to Prometheus, and then Launch Druid.

Read more about the Prometheus emitter [here](https://druid.apache.org/docs/latest/development/extensions-contrib/prometheus.html).

**Step 1**

<hr/>

We've already downloaded Druid for you, so all we need to do is configure it to work with Prometheus. Start by installing the Druid extensions for the Prometheus emitter with the following command.

Read more about this command [here](https://druid.apache.org/docs/latest/development/extensions.html#loading-community-extensions).

```ruby
pushd /root/apache-druid-24.0.2
java \
  -cp "lib/*" \
  -Ddruid.extensions.directory="extensions" \
  -Ddruid.extensions.hadoopDependenciesDir="hadoop-dependencies" \
  org.apache.druid.cli.Main tools pull-deps \
  --no-default-hadoop \
  -c "org.apache.druid.extensions.contrib:prometheus-emitter:24.0.2"
popd
```

**Step 2**

<hr/>

Configure all the Druid processes to use the Prometheus emitter by modifying the common runtime properties with the following `sed` commands.

```ruby
sed -i 's/druid.extensions.loadList=\[/druid.extensions.loadList=\["prometheus-emitter", /g' \
  /root/apache-druid-24.0.2/conf/druid/single-server/nano-quickstart/_common/common.runtime.properties
sed -i 's/noop/prometheus/g' \
  /root/apache-druid-24.0.2/conf/druid/single-server/nano-quickstart/_common/common.runtime.properties
```

**Step 3**

<hr/>

Modify each of the Druid process configuration files to configure the emitter port for that process. This is as easy as adding the required configuration line ([see the docs](https://druid.apache.org/docs/latest/development/extensions-contrib/prometheus.html#configuration)) to each of the process runtime properties individually. So we can use a simple `echo >>` command here to append the necessary configuration lines.

```ruby
echo 'druid.emitter.prometheus.port=9091' \
  >> /root/apache-druid-24.0.2/conf/druid/single-server/nano-quickstart/broker/runtime.properties
echo 'druid.emitter.prometheus.port=9092' \
  >> /root/apache-druid-24.0.2/conf/druid/single-server/nano-quickstart/coordinator-overlord/runtime.properties
echo 'druid.emitter.prometheus.port=9093' \
  >> /root/apache-druid-24.0.2/conf/druid/single-server/nano-quickstart/historical/runtime.properties
echo 'druid.emitter.prometheus.port=9094' \
  >> /root/apache-druid-24.0.2/conf/druid/single-server/nano-quickstart/middleManager/runtime.properties
echo 'druid.emitter.prometheus.port=9095' \
  >> /root/apache-druid-24.0.2/conf/druid/single-server/nano-quickstart/router/runtime.properties
```

**Step 4**

<hr/>

Start Druid and wait for it to initialize.

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

**Step 5**

<hr/>

Since we’ve configured the Prometheus Emitter on each Druid process with its own port, we can curl these emitters' ports to see the metrics the emitters provide. Let's try hitting the Broker emitter to see an example of the metrics Prometheus will retrieve.

```ruby
curl localhost:9091 | tail -10
```

Notice that the comments above the actual metrics describe what the metrics monitor. For example, you may see something like the following:

```ruby
# HELP druid_query_wait_time_created Seconds spent waiting for a segment to be scanned.
# TYPE druid_query_wait_time_created gauge
druid_query_wait_time_created 1.669740725945E9
```

Where:

- _HELP_ describes the metric - in this case it's the time spent scanning segments
- _TYPE_ describes the metric type, which in this case is a gauge or value
- _druid_query_wait_time_created_ is the metric label with the associated measured value

**Step 6**

<hr/>

We can verify that Prometheus is storing these metrics by performing a query using the Prometheus console. The simplest Prometheus query consists merely of the name of the metric.

Paste the following metric name (or type in _druid_ to see the full list), and click Execute.

```ruby
druid_query_wait_time_created
```

We see results from each of the Druid processes as indicated by the port number.

**NOTE**: _If you don't see any JVM metrics in the list, it may be because no JVM metrics have been emitted yet. Wait a bit and refresh the Prometheus console. Eventually, the JVM metrics will appear in the list._

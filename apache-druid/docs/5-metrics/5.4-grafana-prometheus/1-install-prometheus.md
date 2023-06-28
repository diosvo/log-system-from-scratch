## INSTALL PROMETHEUS

This lab consists of three exercises, which configure and install Prometheus, Druid and Grafana, respectively. In this first exercise will focus on Prometheus.

<details>

<summary>What is Prometheus?</summary>

Prometheus is a time series database that we will use to capture the Druid metrics. Read about Prometheus [here](https://prometheus.io/). Prometheus and Grafana are a common toolset that many systems use for monitoring.

If you are wondering if we could use Druid to capture metrics, the answer is yes! However, many people are more familiar with using Prometheus, so because of this familiarity, we'll use Prometheus in this lab.

Also, the point of capturing metrics in Prometheus is _NOT_ so we can query Prometheus directly, but as a metrics-store for Grafana.

</details>

<br/>

**Step 1**

<hr/>

Download Prometheus.

```ruby
wget https://github.com/prometheus/prometheus/releases/download/v2.40.2/prometheus-2.40.2.linux-amd64.tar.gz
tar -xzf /root/prometheus-2.40.2.linux-amd64.tar.gz
```

**Step 2**

<hr/>

Configure Prometheus to work with Druid.

<details>

<summary>What does this command do?</summary>

This `sed` command changes the Prometheus configuration file to cause Prometheus to poll ports 9091-9095 instead of port 9090 (the default). We will later configure each of the Druid processes to emit metrics using these ports - one per Druid process.

</details>

```ruby
sed -i 's/\["localhost:9090"\]/\["localhost:9091", "localhost:9092", "localhost:9093", "localhost:9094", "localhost:9095"\]/g' \
  /root/prometheus-2.40.2.linux-amd64/prometheus.yml
```

<details>

<summary>How does Prometheus work with Druid?</summary>

Prometheus polls target services for metrics, and then stores those metrics. We will configure each of the Druid processes with a Prometheus emitter which will provide an endpoint target for Prometheus. Since we are running a single node Druid cluster, each of these target endpoints must use a separate port. The `sed` command above changes the Prometheus configuration file so that Prometheus will use all the endpoints in the list.
Step 3

</details>

**Step 3**

<hr/>

Start Prometheus.

```ruby
pushd /root/prometheus-2.40.2.linux-amd64
nohup /root/prometheus-2.40.2.linux-amd64/prometheus --config.file=./prometheus.yml > /root/prometheus.log 2> /root/prometheus.err & disown
popd
```

**Step 4**

<hr/>

If you see the console, you are good to go!

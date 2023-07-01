## Local Development

### Build image

Use `docker build` command to build the image. This example names the image as `custom-fluentd:latest`:

```ruby
cd /Users/diosvo/Documents/learning/log-system/log-system-from-scratch/helpers/docker-compose/fluentd/custom-fluentd \
&& docker build -t custom-fluentd:latest ./
```

### Test it

Once the image is built, it's ready to run. Following commands run Fluentd sharing `./log` directory with the host machine:

```ruby
mkdir -p log \
&& docker run -it --rm --name custom-docker-fluent-logger -v $(pwd)/log:/fluentd/log custom-fluentd:latest
```

### Access the container

Open another shell tab to access the container:

```ruby
docker exec -it --user root custom-docker-fluent-logger bash
```

### Interact with Fluentd service

```ruby
systemctl enable fluentd
systemctl start fluentd
systemctl status fluentd
```

### Monitoring with Prometheus

We can get Fluent's metrics via cURL:

```ruby
curl http://localhost:24231/metrics
```

or expose these metrics in Prometheus dashboard

## References

| Name                                                                                             |
| ------------------------------------------------------------------------------------------------ |
| [Install by Docker](https://docs.fluentd.org/container-deployment/install-by-docker)             |
| [Fluentd from Docker](https://hub.docker.com/r/fluent/fluentd/)                                  |
| [How to Monitor Nginx with Prometheus and Grafana?](https://www.youtube.com/watch?v=H45YV-l6GOY) |

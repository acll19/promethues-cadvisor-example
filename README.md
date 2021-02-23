# Prometheus Docker example

## Network

`docker network create monitoring`

## Prometheus

```Bash
docker run --name my-prometheus \
-v ${PWD}/prometheusconf.yml:/etc/prometheus/prometheus.yml 
--publish published=9090,target=9090,protocol=tcp \
 --network=monitoring \
 --detach=true \
prom/prometheus
```

## cAvisor

```Bash
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:ro \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --publish=8080:8080 \
  --detach=true \
  --name=cadvisor \
  --privileged \
  --device=/dev/kmsg \
  --network=monitoring \
  gcr.io/cadvisor/cadvisor:v0.38.6
```

## Metrics URL

http://localhost:8080/metrics

## Memeater

 `docker run --rm --name memeater -v $(pwd)/memex:/tmp/ -w /tmp ubuntu sh memeater.sh 5m`
 
## Grafana

`docker run --detach=true --name=grafana --publish published=3000,target=3000 grafana/grafana`
 
## Panels

(Lines)

`container_memory_max_usage_bytes{id=~"/docker/(70559fcfe415|a4bf95b5221e|42b13bc9a41f)(.)*} / 1024 / 1024`

 (Lines)

 `container_memory_usage_bytes{id=~"/docker/(70559fcfe415|a4bf95b5221e|42b13bc9a41f)(.)*"} / 1024 / 1024`

 (Singlestat)

 `machine_cpu_cores`

 (Singlestat)

 `avg(container_fs_limit_bytes{id=~"/docker/(70559fcfe415|a4bf95b5221e|42b13bc9a41f)(.)*} / 1024 / 1024 / 1024)`

## Doc

 https://github.com/google/cadvisor
 https://docs.docker.com/config/thirdparty/prometheus/
 https://github.com/google/cadvisor/blob/master/docs/storage/prometheus.md

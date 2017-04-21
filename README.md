# grafana-with-prometheus
Grafana with Prometheus, Node Exporter and Cadvisor metrics with ec2_discovery

## Node Exporter -> Host Metrics
## Cadvisor -> Docker Container Metrics
## Memcached Metrics
## MongoDB Metrics
## Elasticsearch Metrics

## Server configuration
### Install docker
-	sudo yum install docker

### Install docker-compose
-	sudo pip install docker-compose

### Give permissions to the user
-	sudo usermod -aG docker ${USER}

### Install git
-	sudo yum install git

### Clone repo
-	git clone "url"

### Run docker-compose in the main folder
-	docker-compose up -d

### Enter to grafana with user: admin and password: admin
-	http://<ip>:80

### Configure datasource to Prometheus
-	url: http://prometheus:9090
-	access: proxy

### import dashboard from grafana directory

## Client configuration
### Install docker
-	sudo yum install docker

### if is not running
-	sudo service docker start

### Node_exporter to take host metrics

-	sudo docker run -d -p 9100:9100 --name=node_exporter -v "/proc:/host/proc" -v "/sys:/host/sys" -v "/:/rootfs" --net="host" prom/node-exporter -collector.procfs /host/proc -collector.sysfs /host/proc -collector.filesystem.ignored-mount-points "^/(sys|proc|dev|host|etc)($|/)"

### Cadvisor to take containers metrics

-	sudo docker run   --volume=/:/rootfs:ro   --volume=/var/run:/var/run:rw   --volume=/sys:/sys:ro   --volume=/var/lib/docker/:/var/lib/docker:ro   --publish=8080:8080   --detach=true   --name=cadvisor   --privileged=true   --volume=/cgroup:/cgroup:ro   google/cadvisor:latest

### Memcached

-	sudo docker run -d -p 9150:9150 prom/memcached-exporter --memcached.address="{ip from host}:11211"

### MongoDB

-	sudo docker run -d -p 9104:9104 eses/mongodb_exporter --mongodb.uri="mongodb://{ip from host}:27017"

### Elasticsearch

-	Elasticsearch plugin (change prometheus metrics target like this: metrics_path: "/_prometheus/metrics" and port 9200)

-	cd /usr/share/elasticsearch

-	for elasticsearch version 5
-	./bin/elasticsearch-plugin install -b https://distfiles.compuscene.net/elasticsearch/elasticsearch-prometheus-exporter-5.3.0.0.zip

-	for elasticsearch version 2
-	./bin/plugin install https://github.com/vvanholl/elasticsearch-prometheus-exporter/releases/download/2.4.1.0/elasticsearch-prometheus-exporter-2.4.1.0.zip

-	restart the nodes

# GLA Time Series Databases
This repo contains supplementary materials to my presentation at GLA summit 2020.


## Getting started

### Docker 
In my example I was running an UBUNTU server with a docker on it. Description how to get docker on your UBUNTU machine can be found at:
https://docs.docker.com/engine/install/ubuntu/
Which boils down to:
```
sudo snap install docker 
```

Most likely you would like to run docker as a non-root user. For that please follow:
https://docs.docker.com/engine/install/linux-postinstall/

### Starting influxDB-grafana container
More detailed description of steps can be found at:
https://towardsdatascience.com/get-system-metrics-for-5-min-with-docker-telegraf-influxdb-and-grafana-97cfd957f0ac

In order to run influxDB-grafana container you need to set up two folders that will keep configuration data:

```
mkdir ~/influx
mkdir ~/grafana
```

then you are ready to launch containers (kodos to https://github.com/philhawthorne/docker-influxdb-grafana ):

```
docker run -d \
  --name docker-influxdb-grafana \
  -p 3003:3003 \
  -p 3004:8083 \
  -p 8086:8086 \
  -v ~/influx:/var/lib/influxdb \
  -v ~/grafana:/var/lib/grafana \
  philhawthorne/docker-influxdb-grafana:latest
```
### Setting up InfluxDB
After startup you need to create a database in influxDB. Navigate to:
http://host:3004

* Log in with root/root
* Navigate to Explore in leftmost menu
* Type `CREATE DATABASE "TestDatabase"` in query field and press "Submit query" to create a database
* Type `CREATE USER "user1" WITH PASSWORD '1resu'` in query field and press "Submit query" to create an user

### Setting up Grafana
Navigate to 
http://host:3003
log in with admin/admin credentials
#### Setting up Data Source
Select the Add data source menu to tell Grafana where to get the Influxdb data.
There we need to select Type = InfluxDB, give the Name for this data source, then put 
```
http://host:8086
```
as the URL
```
TestDatabase
```
as database, and `user1\1resu` as credentials

## Line protocol
In order to put data into database one can use line protocol:
https://docs.influxdata.com/influxdb/v1.8/write_protocols/line_protocol_tutorial/

Now you can write to database with:
```
curl -i -XPOST http://host/write?db=TestDatabase --data-binary "teat1,tag1=01,tag2=sun value=3.14"


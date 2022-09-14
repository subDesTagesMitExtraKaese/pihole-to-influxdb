# pihole-to-influxdb
## Introduction
Based slightly on my other project, [speedtest-to-influxdb](https://github.com/chriscn/speedtest-to-influxdb). This project leverages the [Pi-Hole](https://pi-hole.net/) API to gather data about your PiHole instance and store it inside of InfluxDB for your future projects.

This project is automatically built through GitHub actions and the DockerHub file can be found [here](https://hub.docker.com/r/chriscn/pihole-to-influxdb).
## Setup
### Configuring the script
The InfluxDB connection settings can be configured as followed:
- "INFLUX_DB_URL=http://192.168.xxx.xxx:8086"
- "INFLUX_DB_ORG=\<your org name\>"
- "INFLUX_DB_TOKEN=\<token\>"
- "INFLUX_DB_BUCKET=pihole"

The PiHole settings can be configured as followed:
- PIHOLE_HOSTNAME=192.168.xxx.xxx
- PIHOLE_INTERVAL=15 *Interval in seconds*
### Authentication
Certain parts of the API require you to be authenticated, this can be achieved by supplying the `PIHOLE_AUTHENTICATION` token with the token from the API settings page of the admin interface.  
By doing this you'll gain access to two new measurements (tables): 
- query_types
- forward_destinations
#### Sidenote
This does mean that your password is stored in plaintext as an environmental variable and as such as malicious actor could find it and access your PiHole instance. You are advised to use this at your own risk.
### Docker Command
```
    docker run -d --name pihole-to-influx \
    -e 'INFLUX_DB_URL'='<influxdb url>' \
    -e 'INFLUX_DB_ORG'='<influxdb org>' \
    -e 'INFLUX_DB_TOKEN'='<influxdb token>' \
    -e 'INFLUX_DB_BUCKET'='pihole' \
    -e 'PIHOLE_INTERVAL'='1800' \
    -e 'PIHOLE_HOSTNAME'='192.168.xxx.xxx'  \
    chriscn/pihole-to-influxdb
```
### docker-compose
```yaml
version: '3'
services:
    pihole-to-influxdb:
        image: chriscn/pihole-to-influxdb
        container_name: pihole-to-influxdb
        environment:
        - "INFLUX_DB_URL=http://192.168.xxx.xxx:8086"
        - "INFLUX_DB_ORG=<your org name>"
        - "INFLUX_DB_TOKEN=<token>"
        - "INFLUX_DB_BUCKET=pihole"
        - "PIHOLE_HOSTNAME=192.168.xxx.xxx"
        - "PIHOLE_INTERVAL=15"
```
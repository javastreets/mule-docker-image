
## Specifications

Java Version: Oracle JDK 8
Linux: Alpine

## Pulling image

docker pull javastreets/mule

## Tags

* `latest` : Points to the latest Mule Community edition version.
* `ce430` : Mule Community Edition 4.3.0-20210119
* `raspberrypi3` : Latest Mule Community Runtime running on Raspberry Pi3
* `raspberrypi3-ce381` : Mule Community Runtime 3.8.1 running on Raspberry Pi3

## Starting container
To expose an HTTP port 8081 for an application:

`docker run -d --name mymule -P -v ~/mule/apps:/opt/mule/apps -v ~/mule/logs:/opt/mule/logs javastreets/mule`

**NOTE:** Default timezone is set to UTC. To change it, set `TZ` environment variable while starting the container. Eg. `-e  TZ=America/New_York`.

**!!WARNING!!** Pre-430 image tags has America/New_York as default timezone.

**Mounted Volumes**

| Volume |	Description |
| ----------- | ----------- |
| /opt/mule/apps |	Mule Application deployment directory |
| /opt/mule/domains |	Mule Domains deployment directory |
| /opt/mule/conf |	Configuration directory |
| /opt/mule/logs |	Logs directory |


**Exposed Ports:**
8081	Default HTTP port

To expose additional ports, use `-p` option while running container.

-p 1234:1234

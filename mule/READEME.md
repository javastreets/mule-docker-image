
## Specifications

Jave Version: Oracle JDK 8
Linux: Alpine

## Pulling image

docker pull javastreets/mule

## Starting container
To expose an HTTP port 8081 for an application:

`docker run -d --name mymule -P -v ~/mule/apps:/opt/mule/apps -v ~/mule/logs:/opt/mule/logs javastreets/mule`

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

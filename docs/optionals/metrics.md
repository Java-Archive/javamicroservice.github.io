# Metrics
This section describes the different metrics the MicroService has to offer.

## Active Resources
Normally the MicroService prints out all of it's hosted resources, e.g. servlet classes.
With the activeresources module you can enable the option to ask for all resources made available through urls by the MicroService.
To retrieve the active resources call the path **/info/activeurls**, it will return a JSON message as response.

Example call:
```
http://127.0.0.1:7081/rest/info/activeurls
```
Response:
```json
{"restUrls":["http://0.0.0.0:7081/rest/pojo","http://0.0.0.0:7081/rest/info/activeurls","http://0.0.0.0:7081/rest/test","http://0.0.0.0:7081/rest/OverviewTest/pathA - paramA"],
"servletUrls":["http://0.0.0.0:7080/microservice/test","http://0.0.0.0:7080/microservicetestServlet"],
"singletonUrls":[],
"servletCounter":2}
```

## Health

## JVM 

## Performance
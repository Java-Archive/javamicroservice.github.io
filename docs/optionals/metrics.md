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
{
  "restUrls":["http://0.0.0.0:7081/rest/pojo","http://0.0.0.0:7081/rest/info/activeurls","http://0.0.0.0:7081/rest/test","http://0.0.0.0:7081/rest/OverviewTest/pathA - paramA"],
  "servletUrls":["http://0.0.0.0:7080/microservice/test","http://0.0.0.0:7080/microservicetestServlet"],
  "singletonUrls":[],
  "servletCounter":2}
```

## Health
The MicroService is based on the highly lightweight Undertow server. To retrieve information about the actual health of
the underlying server this module gives you access to the data of the Undertow SessionManager. 

Example call:
```
http://127.0.0.1:7081/rest/metrics/health
```

Response:
```json
[
  {
    "transientSessions": [],
    "deploymentName": "ROOT.war",
    "allSessions": [],
    "activeSessions": [],
    "startTime": 1479842722116,
    "maxActiveSessions": -1,
    "activeSessionCount": 0,
    "averageSessionAliveTime": 0,
    "createdSessionCount": 0,
    "expiredSessionCount": 0,
    "maxSessionAliveTime": 0,
    "rejectedSessions": 0
  }
]
```
## JVM 
Additionally to the core health of the Undertow Server, this module gives you an inside into the JVM it's running in.
The information about the JVM splits into four sections, resulting in four paths (starting with **/metics/jvm**) you can call:
- **loadinfos**: GarbageCollector
- **osinfos**: informations about the os it's running on
- **specinfos**: RuntimeMXBean with system properties of the JVM
- **memoryinfos**: memory usage

### loadinfos
Content: 
- collectionCount
- collectionTime
- memoryPoolNames
- name


Example call:
```
http://127.0.0.1:7081/rest/metrics/jvm/loadinfos
```
Response:
```json
[
  {
    "collectionCount":8,
    "collectionTime":79,
    "memoryPoolNames":["PS Eden Space","PS Survivor Space"],
    "name":"PS Scavenge"
  },
  {
    "collectionCount":0,
    "collectionTime":0,
    "memoryPoolNames":["PS Eden Space","PS Survivor Space","PS Old Gen"],
    "name":"PS MarkSweep"
  }
 ]
```

### osinfos
Content: 
- name
- arch
- version
- processors


Example call:
```
http://127.0.0.1:7081/rest/metrics/jvm/osinfos
```
Response:
```json
{
  "name":"Windows 10",
  "arch":"amd64",
  "version":"10.0",
  "processors":8
}
```

### specinfos
Content: 
- name
- vendor
- version


Example call:
```
http://127.0.0.1:7081/rest/metrics/jvm/specinfos
```
Response:
```json
{
  "name":"Java Virtual Machine Specification",
  "vendor":"Oracle Corporation",
  "version":"1.8"
}
```

### memoryinfos
Content: 
- freeMemory
- maxMemory
- totalMemory
- heapMemoryUsage
- nonHeapMemoryUsage
- objectPendingFinalizationCount


Example call:
```
http://127.0.0.1:7081/rest/metrics/jvm/memoryinfos
```
Response:
```json
{
  "freeMem":196903200,
  "maxMem":3808428032,
  "totalMem":743440384,
  "objectPendingFinalizationCount":0,
  "heapMemoryUsage":{"init":268435456,"used":546537184,"committed":743440384,"max":3808428032},
  "nonHeapMemoryUsage":{"init":2555904,"used":26764928,"committed":27852800,"max":-1}
}
```

## Performance
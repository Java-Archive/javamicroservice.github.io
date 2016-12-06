# Administration Module
The MicroService administration module makes it possible to conveniently shut down the service with an HTTP call. 
Once the module is included to your service, it will host another path **/admin/basicadministration/{timeout}** besides the servlet and rest paths. 
The path parameter behind the last **/** sets the milliseconds the service will wait till it stops.  
To include this function just add the artifact **rapidpm-microservice-modules-optionals-admin** as a dependency in your pom.
  
  
Example call to shutdown a MicroService:
```
http://127.0.0.1:7080/rest/admin/basicadministration/1000

Output:
[XNIO-2 task-1] WARN org.rapidpm.microservice.Main - shutdown delay [ms] = 1000
[Timer-0] WARN org.rapidpm.microservice.Main - delayed shutdown  now = 2016-11-22T19:33:59.159

```


# ServiceWrapper
Born out of the need to start and stop the MicroService as a service on Windows, the ServiceWrapper (combined with Apache's prunsrv) makes this possible.
But we can't stop the MicroService without closing the terminal or the need to stop it with the [shutdown call](admin.md) inside a Windows service. Another way is necessary to stop it.
Basically the ServiceWrapper works as a proxy for the start & stop calls between the MicroService and the Windows service.
Here is a step-by-step guide to create a service for a MicroService on Windows:
Requirements: 
    - prunsrv.exe from [Apache](http://www.apache.org/dist/commons/daemon/binaries/windows/) 
    - path to the jvm.dll
    - if no port is given, the MicroService will use the default port 7080, otherwise provide a property file with rest.port=...
    - if you are using log4j provide a log4j2 config file (optional)

1. Create an install script, e.g. install_myservice.bat
```
set SERVICENAME=MyService
set DISPLAYNAME=MyService
set JVMPARAMS=--Jvm "C:/Program Files/Java/jre1.8.0_112/bin/server/jvm.dll" --JvmMs 128 --JvmMx 1024 ++JvmOptions=-Dlog4j.configurationFile="file://C:/log4j2.xml"
set CLASSPATH="C:/myservice.jar"
set MAINCLASS=org.rapidpm.microservice.optionals.service.ServiceWrapper
set STARTPARAMS=-i#"C:/myservice.properties"

C:/prunsrv //IS//%SERVICENAME% --DisplayName="%DISPLAYNAME%" --Startup=manual --Classpath=%CLASSPATH% --StartMode=jvm --StartClass=%MAINCLASS% ++StartParams=%STARTPARAMS% --StopMode=jvm --StopClass=%MAINCLASS% --StopParams=SHUTDOWN --StopTimeout 5 

pause
```

2. Run the bat file
3. Now Windows should open a popup and asks for permission to install the new service. After you done this the service should
be listet in the services.msc.
4. Once the service is started it will create a file named microservice.rest. This file is used to store the actual port of the service.
5. If you stop the service, the ServiceWrapper will call the [shutdown url](admin.md). 


To deinstall the service create another script, e.g. remove_myservice.bat:
```
@echo off

C:/example/prunsrv //DS//MyService

pause
```


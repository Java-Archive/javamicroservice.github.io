# Welcome to the Project Java MicroService

[![Join the chat at https://gitter.im/RapidPM/rapidpm-microservice](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/RapidPM/rapidpm-microservice?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![](https://build.rapidpm.org/app/rest/builds/buildType:id:RapidPM_Develop_Microservice_Snapshot/statusIcon)](https://build.rapidpm.org/viewType.html?buildTypeId=RapidPM_Develop_Microservice_Snapshot&guest=1)
A base implementation for a microservice.

The Core Service will listen on IP 0.0.0.0
The base configuration will start Servlets at port 7080 and REST-Endpoints at 7081.

## SNAPSHOTS
If you are using maven you could add the following to your settings.xml to get the snapshots.

```
   <profile>
      <id>allow-snapshots</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <repositories>
        <repository>
          <id>snapshots-repo</id>
          <url>https://oss.sonatype.org/content/repositories/snapshots</url>
          <releases>
            <enabled>false</enabled>
          </releases>
          <snapshots>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
          </snapshots>
        </repository>
      </repositories>
    </profile>
```

## Getting started
The use of the MicroService is fairly simple. Just call **Main.deploy()**. This will start the MicroService with
all servlets and REST endpoints it can find. Here is small tutorial:
We create a simple servlet listening on **microservice/test**. Calling it will invoke the doWork() method of our service:
```java
@WebInitParam(value = "Hello World", name = "message")
@WebServlet(urlPatterns = "/test")
public class MyServlet extends HttpServlet {

  @Override
  public void init(final ServletConfig config) throws ServletException {
    super.init(config);
  }

  @Override
  protected void doGet(final HttpServletRequest req, final HttpServletResponse resp) throws ServletException, IOException {
    PrintWriter writer = resp.getWriter();
    writer.write(new Service().doWork()); 
    writer.close();
  }

  @Override
  protected void doPost(final HttpServletRequest req, final HttpServletResponse resp) throws ServletException, IOException {
    doGet(req, resp);
  }
}

class Service {
  public String doWork() {
    return "Hello World";
  }
}
```

If you are familiar with packaging of jars you can skip this section. But if you want to know how to conveniently package your jar with Maven and the [maven-shade-plugin](https://maven.apache.org/plugins/maven-shade-plugin/]), go on. 
This plugin helps you packaging the jar with it's dependencies. Also we will give it the information, which main class it has to execute (manifest). 
After a clean-install Maven will generate you a jar with the MicroService Main as the main class.
```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-shade-plugin</artifactId>
  <executions>
    <execution>
      <phase>package</phase>
      <goals>
        <goal>shade</goal>
      </goals>
      <configuration>
        <dependencyReducedPomLocation>${project.build.directory}/reduced-pom.xml</dependencyReducedPomLocation>
        <transformers>
          <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
            <mainClass>org.rapidpm.microservice.Main</mainClass>
          </transformer>
        </transformers>
        <minimizeJar>false</minimizeJar>
        <artifactSet>
          <excludes>
            <exclude>junit:junit</exclude>
            <exclude>jmock:*</exclude>
            <exclude>org.openjdk.jmh:*</exclude>
            <exclude>*:xml-apis</exclude>
            <exclude>org.apache.maven:lib:tests</exclude>
            <exclude>log4j:log4j:jar:</exclude>
          </excludes>
        </artifactSet>
      </configuration>
    </execution>
  </executions>
</plugin>
```

After starting the jar you will find the following line in the terminal, telling you that the servlet has been loaded. Now you can call the servlet with **http://localhost:7080/microservice/test**
```terminal
List Servlet - URLs
http://0.0.0.0:7080/microservice/test
```


## Examples

See [rapidpm-microservice-examples](https://github.com/RapidPM/rapidpm-microservice-examples) for more demos


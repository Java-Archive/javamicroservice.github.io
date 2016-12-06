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
all servlets and REST endpoints it can find.   

### REST example
Let's create a simple service that will multiply two parameters. The service will listen on the path **/multiply** and returns 
 an Integer. Here is the code:

```java
@Path("/multiply")
public class Service {

  @GET()
  @Produces("text/plain")
  public int get(@QueryParam("x") int x, @QueryParam("y") int y) {
    return Math.multiplyExact(x, y);
  }
}
```
  
And the corresponding JUnit Test:

```java
public class RestTest {

  @Test
  public void testRestPath() throws Exception {
    Main.deploy();
    
    Client client = ClientBuilder.newClient();

    final String generateURL = TestPortProvider.generateURL("/rest" + "/multiply");
    System.out.println("generateURL = " + generateURL);
    int val = client
        .target(generateURL)
        .queryParam("x", 2)
        .queryParam("y", 21)
        .request()
        .get(Integer.class);
    Assert.assertEquals(42, val); // the only truth
    
    client.close();
    Main.stop();
  }

}
```

### Servlet example
Here is a simple WebServlet, that will be deployed by the MicroService as soon as **Main.deploy()** is called:

```java
@WebServlet(urlPatterns = "/hello")
public class HelloServlet extends HttpServlet {

  @Override
  protected void doGet(final HttpServletRequest req, final HttpServletResponse resp) throws ServletException, IOException {
    PrintWriter writer = resp.getWriter();
    writer.write("hello " + req.getParameter("name"));
    writer.close();
  }

  @Override
  protected void doPost(final HttpServletRequest req, final HttpServletResponse resp) throws ServletException, IOException {
    doGet(req, resp);
  }
}
```

And the corresponding JUnit Test:

```java
public class ServletTest extends BaseTest{
  private static String url;

  @Before
  public void setUp() throws Exception {
    Main.deploy();
    url = "http://127.0.0.1:" + System.getProperty(MainUndertow.SERVLET_PORT_PROPERTY)
        + MainUndertow.MYAPP
        + HelloServlet.class.getAnnotation(WebServlet.class).urlPatterns()[0];
    System.out.println("generateURL = " + url);
  }
  
  @After
  public void tearDown() throws Exception {
    Main.stop();
  }

  @Test
  public void testServletGetRequest() throws Exception {
    Client client = ClientBuilder.newClient();

    Response response = client
        .target(url)
        .queryParam("name", "marvin")
        .request()
        .get();
    Assert.assertEquals("hello marvin", response.readEntity(String.class));
    response.close();
    client.close();
  }
}
```

## Examples

See [rapidpm-microservice-examples](https://github.com/RapidPM/rapidpm-microservice-examples) for more demos



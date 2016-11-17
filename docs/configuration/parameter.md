# Parameters

The MicroService supports by  default the following startup parameters:
* restPort
* restHost
* servletPort
* servletHost

For example, to set the servlet port use "-servletPort 7081".

Additionally, if you want to define your own parameters add the dependency **rapidpm-microservice-modules-optionals-cli** and use the interface *CmdLineStartupAction*:

```java
package org.rapidpm.microservice.optionals.cli;

public interface CmdLineStartupAction {
    List<Option> getOptions();
    void execute(CommandLine cmdLine);
}
```

Here's an example implementation that reads in a single property:
```java
public class MyPropertyCmdLineOption implements CmdLineStartupAction {

  public static final String OPT = "mp";

  @Override
  public List<Option> getOptions() {
    return Collections.singletonList(new Option(OPT, "myproperty", true, "set a value for this system property"));
  }

  @Override
  public void execute(CommandLine commandLine) {
    String optionValue = commandLine.getOptionValue(OPT);
    if (commandLine.hasOption(OPT) && !optionValue.isEmpty()) {
      System.setProperty("myproperty", optionValue);
    }
  }
}  
```
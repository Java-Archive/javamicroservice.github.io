# CommandLine Instructions
For an overview of the default commandline instructions pass the argument `-h` to the microservice jar. It will show you this:
```
 -h, -help:  Print this page
 -restPort:  Port for REST
 -restHost:  Host IP for REST
 -servletPort:  Port for optionslets
 -servletHost:  Host IP for optionslets
```

If you want to define your own parameters add the dependency **rapidpm-microservice-modules-optionals-cli** and use the interface *CmdLineStartupAction*:

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
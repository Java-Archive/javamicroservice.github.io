# Configuration

The MicroService supports by  default the following startup parameters:
* restPort
* restHost
* servletPort
* servletHost

E.g. setting the servlet port will be "-servletPort 7081".

To define your own parameters use the interface *CmdLineStartupAction* 
and implement your own action.

```java
public interface CmdLineStartupAction {
    List<Option> getOptions();
    void execute(CommandLine cmdLine);
}
```

Here's an example implementation that reads in a single property:
```java
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
```
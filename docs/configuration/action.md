# Startup / Shutdown Actions

One great thing about the MicroService is that it gives you the ability
to define what should be happen before the MicroService starts and what should
be done after the MicroService stopped.  
To do this define a new action using the interface *MainStartupAction* or *MainShutdownAction* and
implement the method execute().
```java
@FunctionalInterface
public interface MainStartupAction {
  void execute(Optional<String[]> args);
}

@FunctionalInterface
public interface MainShutdownAction {
  void execute(Optional<String[]> args);
}
```



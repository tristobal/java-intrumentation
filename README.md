# INSTRUMENTATION


Provides services that allow Java programming language agents to instrument programs running on the JVM. The mechanism for instrumentation is modification of the byte-codes of methods.

JVM instrumentation feature was introduced in JDK 1.5 and is based on byte code instrumentation (BCI)


## AGENTS

A _javaagent_ is a JVM “plugin”, a specially crafted .jar file, that utilizes the Instrumentation API that the JVM provides.
Every agents have the following parts:

- An agent class (or more) that is deployed as a JAR file.
- A Manifest file to tell the JVM what capabilities to give to the agent.

### Attributes of MANIFEST
* Premain-Class. 
* Agent-Class
* Boot-Class-Path. A list of paths to be searched by the bootstrap class loader.
* Can-Redefine-Classes. Is the ability to redefine classes needed by this agent.
* Can-Retransform-Classes. Is the ability to retransform classes needed by this agent.
* Can-Set-Native-Method-Prefix. Is the ability to set native method prefix needed by this agent. 

There is 2 ways to load the agent:

- Command-line interface.
- Dynamic loading.

![agent-loading](agent-loading.png)

![instrumentation-classes](instrumentation-classes.png)

The **premain** method is invoked when an agent is started **before** the application (Command-line interface).
```
java -javaagent:/path/to/agent.jar -jar myapp.jar

```
The **agentmain** method is invoked when an agent is started **after** the application is already running
(Dynamic loading).

```
public static void premain(String agentArgs, Instrumentation inst);
public static void premain(String agentArgs);
```

```
public static void agentmain(String agentArgs, Instrumentation inst);
public static void agentmain(String agentArgs);
```

## Byte Code Instrumentation
With the premain and agentmain methods, the agent can register a **ClassFileTransformer** instance by providing an implementation
of this interface in order to transform class files.

![classfile-transformation](classfile-transformation.png)

```
byte[] transform(ClassLoader loader, String className, Class<?> classBeingRedefined,
                 ProtectionDomain protectionDomain, byte[] classfileBuffer);
```

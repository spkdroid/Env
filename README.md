# Environment Variables

### Traditional Java EE Approach

In traditional Java EE applications, configuration parameters are often stored in external configuration files (e.g., `properties` files), JNDI (Java Naming and Directory Interface), or as environment variables at the operating system level. Here's how you can access these different sources:

1. **Using System Environment Variables**

   Environment variables can be set at the OS level and accessed in Java using `System.getenv()`. This method retrieves the value of an environment variable, and it's straightforward to use:

   ```java
   String myEnvVar = System.getenv("MY_ENV_VAR");
   if (myEnvVar != null) {
       // Use the environment variable
   } else {
       // Handle the case where the environment variable is not set
   }
   ```

2. **Using JNDI for Resource Injection**

   Java EE supports resource injection, which can be used to inject resources from the application server's environment into your application. This method is particularly useful for resources like data sources, JMS connections, or environment entries defined in the application server.

   To use JNDI for environment entries, you first define them in your deployment descriptor (`web.xml` for web applications or `ejb-jar.xml` for EJB modules) or via annotations:

   ```xml
   <env-entry>
       <env-entry-name>myEnvVar</env-entry-name>
       <env-entry-type>java.lang.String</env-entry-type>
       <env-entry-value>MyValue</env-entry-value>
   </env-entry>
   ```

   Then, you can inject this value into your EJB or servlet using the `@Resource` annotation:

   ```java
   import javax.annotation.Resource;
   
   public class MyBean {
       @Resource(name = "myEnvVar")
       private String myEnvVar;

       public void doSomething() {
           // Use myEnvVar
       }
   }
   ```

### Modern Approach with Application Servers and Containers

In modern Java EE (now Jakarta EE) and microservice environments, especially those running in containers (e.g., Docker, Kubernetes), environment variables are commonly used for configuration.

1. **Docker Containers**

   When running your Java EE application in a Docker container, you can specify environment variables in your Dockerfile or docker-compose file:

   ```dockerfile
   ENV MY_ENV_VAR MyValue
   ```

   or in `docker-compose.yml`:

   ```yaml
   services:
     myapp:
       environment:
         - MY_ENV_VAR=MyValue
   ```

   Access these variables in your Java code using `System.getenv()` as shown earlier.

2. **Kubernetes**

   In Kubernetes, you can define environment variables in your deployment configuration:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: myapp
   spec:
     containers:
       - name: myapp
         image: myapp:latest
         env:
           - name: MY_ENV_VAR
             value: "MyValue"
   ```

   Again, access them in Java using `System.getenv()`.

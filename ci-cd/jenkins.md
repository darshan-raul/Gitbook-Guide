# Jenkins

Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.

### architecture

The Jenkins WAR file bundles [Winstone](https://github.com/jenkinsci/winstone), a [Jetty](https://www.eclipse.org/jetty/) servlet container wrapper, and can be started on any operating system or platform with a version of Java supported by Jenkins

Let's break it down:

1. **WAR (Web Application Archive) file**:
   * A WAR file is a standard file format used for distributing and deploying Java web applications.
   * It is a compressed archive file (similar to a ZIP file) that contains all the necessary components of a web application, such as Java classes, configuration files, libraries, and static resources (HTML, CSS, JavaScript, etc.).
   * The WAR file structure follows a specific directory layout defined by the Java Servlet specification.
2. **Winstone**:
   * Winstone is a lightweight Java servlet container that is bundled with the Jenkins WAR file.
   * A servlet container is a software component that manages and runs Java servlets and web applications.
   * Winstone is based on the Jetty servlet container, which is a popular open-source project for running Java web applications.
   * By bundling Winstone, the Jenkins WAR file becomes a self-contained package that can run the Jenkins web application without requiring a separate servlet container to be installed.
3. **Jetty servlet container wrapper**:
   * As mentioned earlier, Winstone is based on Jetty, which is a widely used servlet container.
   * Winstone acts as a wrapper around the Jetty servlet container, providing a simplified and lightweight way to run Java web applications like Jenkins.
   * The term "wrapper" in this context means that Winstone encapsulates and manages the underlying Jetty servlet container, abstracting away some of the complexities and configuration details.

By packaging Jenkins as a WAR file bundled with Winstone (a Jetty servlet container wrapper), it simplifies the deployment process and makes Jenkins easier to run in various environments. This self-contained approach has several advantages:

1. **Easy deployment**: The Jenkins WAR file can be deployed as-is, without the need to install and configure a separate servlet container like Apache Tomcat or Jetty.
2. **Portability**: The bundled servlet container allows Jenkins to run consistently across different operating systems and environments, as long as a Java Runtime Environment (JRE) is available.
3. **Isolation**: By bundling its own servlet container, Jenkins is isolated from other web applications or servlet containers running on the same machine, reducing potential conflicts or interference.
4. **Customization**: If needed, the bundled Winstone (and underlying Jetty) can be configured and customized to meet specific requirements, such as adjusting memory settings, enabling SSL, or modifying servlet container behavior.

In summary, the statement "Jenkins says it's a war file which bundles Winstone, a Jetty servlet container wrapper" indicates that Jenkins is packaged as a self-contained WAR file that includes its own lightweight servlet container (Winstone, based on Jetty) for running the Jenkins web application. This approach simplifies deployment and provides portability across different environments, as long as a compatible Java Runtime Environment is available.

Theoretically, Jenkins can also be run as a servlet in a traditional servlet container like [Apache Tomcat](https://tomcat.apache.org/) or [WildFly](https://www.wildfly.org/), but in practice this is largely untested and there are many caveats.



A [LTS (Long-Term Support) release](https://www.jenkins.io/download/lts/) is chosen every 12 weeks from the stream of regular releases as the stable release for that time period.

## references:

{% embed url="https://www.jenkins.io/doc/" %}

{% embed url="https://www.jenkins.io/doc/book/using/best-practices/" %}

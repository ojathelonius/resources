## Tomcat
Apache Tomcat is an open-source Java Servlet Container.

* [Enable hot reload for static resources](#enable-hot-reload-for-static-resources)

### Enable hot reload for static resources
If Tomcat serves static files that are built externally (e.g. with webpack), it is possible to refresh the web application without republishing the web app.

In `server.xml`'s GUI, tick **Serve modules without publishing**.

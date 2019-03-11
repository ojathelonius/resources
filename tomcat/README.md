## Tomcat
Apache Tomcat is an open-source Java Servlet Container.

* [Enable hot reload for static resources](#enable-hot-reload-for-static-resources)
* [Troubleshoot : XML descriptors might not be available at runtime, causing web app to hang](#xml-descriptors-availability-at-runtime)
* [Find relevant log files](#find-relevant-log-files)

### Enable hot reload for static resources
If Tomcat serves static files that are built externally (e.g. with webpack), it is possible to refresh the web application without republishing the web app.

In `server.xml`'s GUI, tick **Serve modules without publishing**.


### XML descriptors availability at runtime
XML files might reference a **Document Type Definition** (DTD) file that is not available at runtime.

```xml
<!-- if apache.org is down or unavailable from your machine, this XML will never be loaded -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE tiles-definitions PUBLIC "-//Apache Software Foundation//DTD Tiles Configuration 2.0//EN"
	"http://tiles.apache.org/dtds/tiles-config_2_0.dtd">
```

This will cause the web application to hang without providing any error message for several minutes.

Try using another source for the DTD or simply change the DTD version and try again.

### Find relevant log files
It is more convenient to see log files sorted by date

```bash
ls -altr
```

It is also possible to filter files based on the date of modification :

```bash
ls -ltr | grep "$(date '+%b %e')"
```



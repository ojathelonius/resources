## Eclipse

**Troubleshoot**
* [Error : unable to find taglib](#error-unable-to-find-taglib)

### Error : unable to find taglib
When accessing the web-app, errors can appear as such :

```
org.apache.jasper.JasperException: Unable to find taglib [s] for URI: [/struts-tags]
```

```
org.apache.jasper.JasperException: L'uri absolue: [http://tiles.apache.org/tags-tiles] ne peut être résolu dans le fichier web.xml ou dans les fichiers jar déployés avec cette application
```

This happens usually because the **Serve modules without publishing** is ticked in `server.xml`. Untick it to make the error go away.

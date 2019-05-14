## Compression

* [Compression with wildcard and folder change](#compression-with-wildcard-and-folder-change)

### Compression with wildcard and folder change

Say a directory is the following : 
```
target/result-0.0.1/dist/
```

We want to archive it without keeping the `result-0.0.X/dist/` folder structure.

**tar** provides the `-C` option which allows changing directory before executing any command, however it does not work with wildcards which we need here as we are dealing with a version number.

Instead, simply cd to the final folder and back :

```bash
cd target/result-*/dist && tar -czvf ../../result.tar.gz *
```

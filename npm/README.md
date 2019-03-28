## npm
npm is a package manager for the JavaScript programming language.

* [Extract flat dependencies as CSV](#extract-flat-dependencies-as-csv)

### Extract flat dependencies as CSV
Here is a way to list all `package.json` dependencies along with their version conveniently using  `npm list` :

```bash
npm ls --parseable=true --long=true --only=prod | cut -f3 -d":" | sed "s/@/;/g"
```

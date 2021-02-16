# Typescript Compiler Cheatsheet
## [Using tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#using-tsconfigjson-or-jsconfigjson)
```bash
# By invoking tsc with no input
> tsc
# By invoking tsc with --project option
> tsc -p tsconfig.json
```

### Useful tsconfig.json fields
* `moduleResolution`: node
  * Compile to node.js style module (CommonJS)

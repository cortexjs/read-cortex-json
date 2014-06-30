# read-cortex-json [![NPM version](https://badge.fury.io/js/read-cortex-json.svg)](http://badge.fury.io/js/read-cortex-json) [![Build Status](https://travis-ci.org/cortexjs/read-cortex-json.svg?branch=master)](https://travis-ci.org/cortexjs/read-cortex-json) [![Dependency Status](https://gemnasium.com/cortexjs/read-cortex-json.svg)](https://gemnasium.com/cortexjs/read-cortex-json)

The thing [cortex](http://www.npmjs.org/package/cortex) uses to read cortex.json files or fallback to read cortex configurations from package.json.

Cortex supports both `cortex.json` and the `'cortex'` field of `package.json`. But with `read-cortex-json`, you don't have to care about all this, and you should **ALWAYS** use this module to read and save `cortex.json`. 

## Install

```bash
npm install read-cortex-json --save
```

## Usage

```js
var cortexJson = require('read-cortex-json');
```

### cortexJson.read(cwd, callback)

- cwd `path` current working directory
- callback `function(err, json)`
- err `Error`
- json `Object`

Reads and returns the information of a cortex package.

```js
cortexJson.read('/path/to/your/repo', function(err, json){
  if (err) {
    console.error('There was an error', e);
    return;
  }
  
  console.log('The package data is:', json);
});
```

### cortexJson.enhanced(cwd, callback)

This method is different from `cortexJson.read()` that it will validate some stuff and flavors the object with some default values which cortex registry needs.

- `cortex.main` will be `require.resolve()`d. That is, if there is a `index.js`, but `cortex.main` is `'index'`, and after `enhanced()`, `cortex.main` will be `'index.js'`.
- `cortex.css` and `cortex.entries` will always be an **globbed** array(even an empty array) after `enhanced()`
- If a path, either from `main`, `css`, or `entries`, is explicitly defined but not found, an error will throw.
- If a package has neither `main`, `css`, nor `entries`, an `'CORTEX_NO_ENTRY'` error will throw.


### cortexJson.clean(cwd, json, callback)

Cleans the `json` for legacy.

### cortexJson.save(cwd, json, callback)

- callback `function(err)`

Save the json to the file.

### cortexJson.package_root(cwd, callback)

- callback `function(root)`

Traverses up from the `cwd` and gets the first directory that contains a package file of cortex.

Sometimes user might run cortex commands inside the subtle directories of the current repo, and we must make sure cortex commands always run at the root directory.

### cortexJson.cached_document(name, cache_root, callback)

- name `String` package name
- cache_root `path` cache directory of the current profile
- callback `function(err, document)`
- document `json`

Gets the cached document.






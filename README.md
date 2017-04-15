# api documentation for  [gulp-compass (v2.1.0)](https://github.com/appleboy/gulp-compass)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-compass.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-compass) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-compass.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-compass)
#### Compile Compass files

[![NPM](https://nodei.co/npm/gulp-compass.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/gulp-compass)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-compass/build/screenCapture.buildCi.browser.apidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-compass/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-compass/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-compass/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Bo-Yi Wu"
    },
    "bugs": {
        "url": "https://github.com/appleboy/gulp-compass/issues"
    },
    "dependencies": {
        "gulp-util": "^3.0.4",
        "through2": "^0.6.5",
        "which": "^1.1.1"
    },
    "description": "Compile Compass files",
    "devDependencies": {
        "coveralls": "^2.11.2",
        "del": "^1.1.1",
        "gulp": "^3.8.10",
        "gulp-istanbul": "^0.6.0",
        "gulp-jscs": "^1.6.0",
        "gulp-jshint": "^1.9.2",
        "gulp-load-plugins": "^0.8.0",
        "gulp-mocha": "^2.0.0",
        "iconv-lite": "^0.4.7",
        "jshint-reporter-jscs": "^0.1.0",
        "map-stream": "0.0.5",
        "mocha": "^2.1.0",
        "should": "^4.6.5"
    },
    "directories": {},
    "dist": {
        "shasum": "1fef21ed547fc8685e754fc31954c4fc6dd2bcb3",
        "tarball": "https://registry.npmjs.org/gulp-compass/-/gulp-compass-2.1.0.tgz"
    },
    "engines": {
        "node": ">=0.10.0"
    },
    "gitHead": "0b1d104fc52d933b72db7701602bd385eace0ba7",
    "homepage": "https://github.com/appleboy/gulp-compass",
    "keywords": [
        "gulp",
        "gulpplugin",
        "compass",
        "scss",
        "sass",
        "css",
        "compile",
        "preprocessor",
        "style"
    ],
    "license": "MIT",
    "main": "lib",
    "maintainers": [
        {
            "name": "appleboy"
        }
    ],
    "name": "gulp-compass",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git://github.com/appleboy/gulp-compass.git"
    },
    "scripts": {
        "test": "gulp"
    },
    "version": "2.1.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-compass](#apidoc.module.gulp-compass)
1.  [function <span class="apidocSignatureSpan"></span>gulp-compass (opt)](#apidoc.element.gulp-compass.gulp-compass)
1.  [function <span class="apidocSignatureSpan">gulp-compass.</span>toString ()](#apidoc.element.gulp-compass.toString)



# <a name="apidoc.module.gulp-compass"></a>[module gulp-compass](#apidoc.module.gulp-compass)

#### <a name="apidoc.element.gulp-compass.gulp-compass"></a>[function <span class="apidocSignatureSpan"></span>gulp-compass (opt)](#apidoc.element.gulp-compass.gulp-compass)
- description and source-code
```javascript
gulp-compass = function (opt) {
  var files = [];

  var collectNames = function(file, enc, cb) {
    if (file.isNull()) {
      return cb(null, file);
    }

    if (file.isStream()) {
      return cb(new gutil.PluginError(PLUGIN_NAME, 'Streaming not supported'));
    }

    if (path.basename(file.path)[0] !== '_') {
      files.push(file);
    }

    return cb();
  };

  var readFileAndPush = function(pathToCss, outputStream, cb) {
    // Read each generated file so it can continue being streamed.
    fs.readFile(pathToCss, function(err, contents) {
      if (err) {
        return cb(new gutil.PluginError(PLUGIN_NAME, 'Failure reading in the CSS output file'));
      }

      // Fix garbled output.
      if (!(contents instanceof Buffer)) {
        contents = new Buffer(contents);
      }

      outputStream.push(new gutil.File({
        base: opt.css,
        path: pathToCss,
        contents: contents
      }));

      cb();
    });
  };

  var compile = function(cb) {
    var _this = this;
    var fileNames = files.map(function(f) {
      return f.path;
    });

    compass(fileNames, opt, function(code, stdout, stderr, pathsToCss, options) {
      if (code === 127) {
        return cb(new gutil.PluginError(PLUGIN_NAME, 'You need to have Ruby and Compass installed ' +
          'and in your system PATH for this task to work.'));
      }

      // support error callback
      if (code !== 0) {
        return cb(new gutil.PluginError(PLUGIN_NAME, stdout || 'Compass failed'));
      }

      cb = callCounter(files.length, cb);
      pathsToCss.forEach(function(f) {
        readFileAndPush(f, _this, cb);
      });
    });
  };

  return through.obj(collectNames, compile);
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.gulp-compass.toString"></a>[function <span class="apidocSignatureSpan">gulp-compass.</span>toString ()](#apidoc.element.gulp-compass.toString)
- description and source-code
```javascript
toString = function () {
    return toString;
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)

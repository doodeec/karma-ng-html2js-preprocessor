# karma-ng-require-html2js-preprocessor

> Preprocessor for converting HTML files to [AngularJS](http://angularjs.org/) templates. Template modules are served as RequireJS modules.

## Installation

The easiest way is to keep `karma-ng-require-html2js-preprocessor` as a devDependency in your `package.json`.
```json
{
  "devDependencies": {
    "karma": "~0.10",
    "karma-ng-require-html2js-preprocessor": "~0.1"
  }
}
```

You can simple do it by:
```bash
npm install karma-ng-require-html2js-preprocessor --save-dev
```

Remember to define a RequireJS path corresponding to angular source
```js
requirejs.config({
    paths: {
        'angular': "path-to-angular/angular.min",
        ...
    },
    baseUrl: ...
    shim: ...
});
```

## Configuration
```js
// karma.conf.js
module.exports = function(config) {
  config.set({
    preprocessors: {
      '**/*.html': ['ng-html2js']
    },

    files: [
      '*.js',
      '*.html',
      '*.html.ext',
      // if you wanna load template files in nested directories, you must use this
      '**/*.html'
    ],

    ngRequireHtml2JsPreprocessor: {
      // strip this from the file path
      stripPrefix: 'public/',
      stripSufix: '.ext',
      // prepend this to the
      prependPrefix: 'served/',

      // or define a custom transform function
      cacheIdFromPath: function(filepath) {
        return cacheId;
      },

      // setting this option will create only a single module that contains templates
      // from all the files, so you can load them all with module('foo')
      moduleName: 'foo'
    }
  });
};
```

## How does it work ?

This preprocessor converts HTML files into JS strings and generates Angular modules. These modules, when loaded, puts these HTML files into the `$templateCache` and therefore Angular won't try to fetch them from the server.

For instance this `template.html`...
```html
<div>something</div>
```
... will be served as `template.html.js`:
```js
define(['angular'], function() {
  angular.module('template.html', []).config(function($templateCache) {
    $templateCache.put('template.html', '<div>something</div>');
  });
});
```

For any query contact me via [email].


[email]: mailto:doodeec@gmail.com

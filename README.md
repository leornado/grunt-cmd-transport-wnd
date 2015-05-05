# grunt-cmd-transport-wnd

> Transport javascript into cmd.

This plugin modified from grunt-cmd-transport  `~0.5.1` for transport revved js files, it's 4 personal test, not for common use.

1st seajs file:
console/js/console.js

```js
define(function (require) {

  var app = require('console/js/app.js');
  console.log('========');

});
```
2nd seajs file:
console/js/app.js

```js
define(function (require) {

  console.log('-------');

});
```

GruntFile.js like
also see * [`grunt-usemin-wnd`](https://www.npmjs.com/package/grunt-usemin-wnd)

```js
...
usemin: { // from grunt-usemin-wnd
  options: {
    assetsDirs       : [
      '<%= config.dist %>/console/*',
      '<%= config.dist %>/console',
      '<%= config.dist %>'
    ],
    patterns         : {
      js: [
        [
          /['"]usemin:([^'"]*)["']/gm,
          'Replacing seajs require parts',
          function (m) { // add ext '.js'
            return path.extname(m) == '.js' ? m : m + '.js';
          }, ,
          function (m) { // cut prefix 'usemin:'
            return m.replace(/usemin:/, '');
          }
        ]
      ]
    }
  },
  js     : ['<%= config.dist %>/*/js/*.js']
},
transport: {
  options: {
    usemin      : true,
    useminPrefix: 'usemin:' // see usemin.
  },
  dist: {
    options: {
      paths: ['<%= config.dist %>']
    },
    files  : [
      {
        expand: true,
        cwd   : '<%= config.dist %>',
        src   : [
          '*/js/*.js'
        ],
        dest  : '<%= config.dist %>'
      }
    ]
  }
}
...
grunt.registerTask('build', [
  'clean:dist',
  'includereplace:dist',
  'useminPrepare',
  'concurrent:dist',
  'svgmin',
  'autoprefixer',
  'concat',
  'copy:dist',
  'cssmin',
  'imagemin',
  'transport',
  'uglify',
  'rev',
  'usemin',
  'replace',
  'htmlmin'
]);
``` 
[cause of usemin replace transported js at last, so the revved file hash is not equals to the real value of file content](#)

then target file will transport to:

1st seajs file:
```js
"use strict";define("console/js/d5c52cbc.console",["console/js/0a46e031.app"],function(a){a("console/js/0a46e031.app.js");console.log("========")});
```

## Getting Started

This plugin requires Grunt `~0.4.1`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-cmd-transport --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-cmd-transport');
```

## The "transport" task

### Overview

In your project's Gruntfile, add a section named `cmd_transport` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  transport: {
    options: {
      // Task-specific options go here.
    },
    your_target: {
      // Target-specific file lists and/or options go here.
    },
  },
})
```

### Options

#### options.paths

Type: `Array`
Default value: `['sea-modules']`

Where are the modules in the sea.

#### options.idleading

Type: `String`
Default value: `""`

Prepend idleading to generate the id of the module.

#### options.alias

Type: `Object`
Default value: `{}`

Alias of modules.

#### options.debug

Type: `Boolean`
Default value: `true`

Create a debugfile or not.

#### options.handlebars

Type: `Object`

Options for handlebars compiler.

Configure handlebars ID:

```js
options: {
    handlebars: {
        id: 'handlebars'
    }
}
```

#### options.uglify

Type: `Object`

Uglify prettifier, you really don't have to change this value.


#### options.parsers

Transport a specific filetype with the right parser.

You can write your own parsers, for example `coffeeParser`:

```js
options: {
    parsers: {
        '.coffee': [coffeeParser]
    }
}
```

Sorry for the missing documentation on how to write a parser.

### Usage Examples

Gruntfile use default options.


```js
grunt.initConfig({
    transport: {
        target_name: {
            files: [{
                cwd: 'src',
                src: '**/*',
                dest: 'dist'
            }]
        }
    }
});
```

## Contributing

In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

## Release History

**Jan 23th, 2015** `0.5.1`

Fix css2js generate wrong code

**Jan 22th, 2015** `0.5.0`

Support hash

**Dec 4th, 2013** `0.4.1`

fix Windows path #58

**Oct 15th, 2013** `0.4.0`

- delete hack for grunt file object #45 **not compatible**

- resolve deps error if require file and folder with the same name #50

**Sep 4th, 2013** `0.3.0`

Remove styleBox id logic added in 0.2.12, now require outside css module do not adding to styleBox,
that resolve lots of bugs.

**Sep 4th, 2013** `0.2.12`

styleBox css module should has styleBox id.

**Oct 28st, 2013** `0.2.11`

stylebox support array.

**Oct 28st, 2013** `0.2.10`

stylebox support :root selector
support id/deps sepecified
don't resolve text!path/to/some.xx

**Jul 1st, 2013** `0.2.9`

fix deps duplicate

**Jun 27th, 2013** `0.2.8`

- improve parsing css
- add testcase

**Jun 26th, 2013** `0.2.7`

- improve log
- remove .js extname in dependencies
- add styleBox option

**Jun 19th, 2013** `0.2.6`

Show parsing JS error log.

**Jun 17th, 2013** `0.2.5`

Handlebars ID configurable.

Bugfix for not showing JS parse error.

**May 28th, 2013** `0.2.4`

Use a specified version of Handlebars.

**May 6th, 2013** `0.2.3

Don't stop the process when the file not exists.

**April 25th, 2013** `0.2.2`

Fix on filter id.

**April 15th, 2013** `0.2.1`

Restore tplParser.

**April 11th, 2013** `0.2.0`

Changed the option configuration.

**April 10th, 2013** `0.1.3`

Upgrade dependencies.

**April 9th, 2013** `0.1.2`

Bugfix for parsing nested relative dependencies.

**April 1st, 2013** `0.1.1`

Template process on source data.

**April 1st, 2013** `0.1.0`

First version.

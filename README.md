grex
====

[Gremlin](https://github.com/tinkerpop/gremlin/wiki) inspired javascript client for [Rexster Graph Server](https://github.com/tinkerpop/rexster/wiki).

## Dependancies

__batch kibble__ (extension)

Place the batch-kibble-XXX.jar in the ``/ext`` folder in the rexster server directory.
Add an ``allow`` tag to the database extensions configuration in the rexster.xml file.

    <extensions>
        <allows>
            <allow>tp:gremlin</allow>
            <allow>tp:batch</allow>
        </allows>
    </extensions>

[__Q__](http://documentup.com/kriskowal/q/)

A tool for making and composing asynchronous promises in JavaScript.

## Installation

grex can be loaded as:

-   a ``<script>`` tag in the browser (creating a ``g`` global variable)

    ```
     <script type="text/javascript" src="q.min.js"></script>    
     <script type="text/javascript" src="grex-client.js"></script>
    ```

-   a Node.js and CommonJS module available from NPM as the ``grex`` package

    ```bash
    $ npm install grex
    ```

    then in node

    ```
    var g = require(“grex”);
    ```

-   a RequireJS module

## Introduction

grex tries to implement Gremlin syntax as closely as possible. However, there are some differences.

* All method calls require brackets __()__, even if there are no arguments.
* __Closures__ do not translate to javascript and are passed in as string arguments to grex methods. 

    ```e.g.
    g.v(1).out().gather("{it.size()}")
    ```
* __Comparators__ and __Float__'s are not native javascript Types so need to be passed in as a string to grex methods. Floats need to be suffixed with a 'f'.

    ```e.g.
    g.v(1).outE.has("weight", "T.gte", "0.5f").property("weight")
    ```
* Certain methods cannot be implemented. Such as ``aggregate``, ``store``, ``table``, ``tree`` and ``fill``. These methods take a local object and populate it with data, which cannot be done in this environment.

## Getting Started

###Options

Options specify the location and name of the database.

####host (default: localhost)

Location of Rexster server

####port (default: 8182)

Rexster server port

####graph (default: tinkergraph)

Graph database name

####idRegex (default: false)

This can remain as false ids are numbers. If the id is not a number (i.e. alpha-numeric or string), then idRegex must to be set. This property will enable grex to distinduish between an id and another expression.

```e.g.
g.setOptions({ host: 'myDomain', graph: 'myOrientdb', idRegex: /^[0-9]+:[0-9]+$/ });
```

## Examples

A good resource to understand the Gremlin API is [GremlinDocs](http://gremlindocs.com/). Below are examples of gremlin and it's equivalent grex syntax.

__N.B.:__ Grex uses the [Q](http://documentup.com/kriskowal/q/) module to return a Promise when making Ajax calls.

```javascript
var g = require("grex");

var result = function (res) {
    console.log('Result=>'+res);
};

var error = function (err) {
    console.log('Error=>'+err);
};
```

```gremlin>
g.v(1).outE.or(_().has('id', T.eq, "9"), _().has('weight', T.lt, 0.6f))
```

```grex>
g.v(1).outE().or(g._().has('id', 'T.eq', 9), g._().has('weight', 'T.lt', '0.6f')).get().then(result, error); 
```

##License
###The MIT License (MIT)

Copyright (c) 2013 entrendipity pty ltd

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
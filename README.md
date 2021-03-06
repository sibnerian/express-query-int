express-query-int
=================

> Convert query strings to numbers for express/connect applications.

[![npm](https://img.shields.io/npm/v/express-query-int.svg)](https://www.npmjs.com/package/express-query-int)
[![build status](https://travis-ci.org/mariusc23/express-query-int.svg)](https://travis-ci.org/mariusc23/express-query-int)

## Installation

    npm install --save express-query-int


## Getting Started
The module will recursively attempt to parse every property in `req.query`.

Load it right after the `bodyParser`:

```js
var queryParser = require('express-query-int');

// [...]

app.use(bodyParser.json());
app.use(queryParser());
```

#### Without
```js
// ?a=1&b[c]=2
console.log(req.query);
// => { a: '4', b: { c: '2' } }
```

#### With
```js
// ?a=1&b[c]=2
console.log(req.query);
// => { a: 4, b: { c: 2 } }
```

### Floating Point
By default the parser will use `parseInt` to convert numbers. You can use `parseFloat` or your own function.

```js
app.use(queryParser({
  parser: parseFloat
}));
```

### Protected Keys
By default the parser will transform any string that resembles a number. Sometimes, though, you'll have a query argument like `id` that you want to stay a string. For cases like this, just pass in a `protect` function - the parser won't be applied for keys where `protect(key)` is true.

```js
app.use(queryParser({
  protect: function (key) {
    return key === 'id';
  }
}));
```

### Custom Parser
Provide a function that takes two arguments:

- `value`: a string potentially representing a number
- `radix`: 10

```js
app.use(queryParser({
  parser: function(value, radix) {
    if (true) {
      return modifiedValue;
    }
    else {
      return NaN;
    }
  }
}));
```

## License
Copyright (c) 2015 Marius Craciunoiu. Licensed under the MIT license.

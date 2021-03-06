# RAML Sanitize

[![NPM version][npm-image]][npm-url]
[![Build status][travis-image]][travis-url]
[![Test coverage][coveralls-image]][coveralls-url]

Strict sanitization of [RAML parameters](https://github.com/raml-org/raml-spec/blob/master/raml-0.8.md#named-parameters) into correct types for JavaScript. If sanitization fails, the original value is returned.

## Why?

This module sanitizes values using the RAML parameter syntax. You should use this if you need to convert any request parameters (usually strings) into the corresponding JavaScript types. For example, form request bodies, query parameters and headers all have no concept of types. After running sanitization, you can use [raml-validate](https://github.com/blakeembrey/raml-validate) to validate the strict values.

## Installation

```shell
npm install raml-sanitize --save
```

## Usage

The module exports a function that needs to be invoked to get a sanitization instance.

```javascript
var sanitize = require('raml-sanitize')();

var user = sanitize({
  username: {
    type: 'string'
  },
  password: {
    type: 'string'
  },
  birthday: {
    type: 'date',
    default: new Date()
  }
});

user({
  username: 'blakeembrey',
  password: 'hunter2'
});
// => { username: 'blakeembrey', password: 'hunter2', birthday: new Date() }
```

**Module does not currently support [wild-card parameters](https://github.com/raml-org/raml-spec/blob/master/raml-0.8.md#headers)**

### Type sanitization

The module comes with built-in type sanitization of all [RAML parameters](https://github.com/raml-org/raml-spec/blob/master/raml-0.8.md#named-parameters) - `string`, `number`, `integer`, `date` and `boolean`. To add a new type sanitization, add a new property with the corresponding name to the `sanitize.TYPES` object.

### Rule sanitization

The module can be extended with rule sanitization by adding properties to the `sanitize.RULES` object. A few core rules are implemented by default and can not be overriden - `repeat`, `default` and `type`.

#### Empty values

Empty values are automatically allowed to pass through sanitization. The only values considered to be empty are `undefined` and `null`.

#### Default values

When the value is empty and a `default` value has been provided, it will return the default value instead.

#### Repeated values

When the repeat flag is set to `true`, the return value will be an array. If the value is not an array, it will be wrapped in an array. If the value is empty, an empty array will be returned.

### Caveats

#### Invalid Sanitization

If a sanitization is invalid, the original value will be returned instead.

#### Booleans

Only `false`, `0`, `"false"`, `"0"` and `""` will return `false`. Everything else is considered `true`.

## License

Apache 2.0

[npm-image]: https://img.shields.io/npm/v/raml-sanitize.svg?style=flat
[npm-url]: https://npmjs.org/package/raml-sanitize
[travis-image]: https://img.shields.io/travis/mulesoft/node-raml-sanitize.svg?style=flat
[travis-url]: https://travis-ci.org/mulesoft/node-raml-sanitize
[coveralls-image]: https://img.shields.io/coveralls/mulesoft/node-raml-sanitize.svg?style=flat
[coveralls-url]: https://coveralls.io/r/mulesoft/node-raml-sanitize?branch=master

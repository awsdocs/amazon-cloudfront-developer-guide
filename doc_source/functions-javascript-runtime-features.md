# JavaScript runtime features for CloudFront Functions<a name="functions-javascript-runtime-features"></a>

The CloudFront Functions JavaScript runtime environment is compliant with [ECMAScript \(ES\) version 5\.1](https://www.ecma-international.org/ecma-262/5.1/) and also supports some features of ES versions 6 through 9\. It also provides some nonstandard methods that are not part of the ES specifications\. The following topics list all the supported language features\.

**Topics**
+ [Core features](#writing-functions-javascript-features-core)
+ [Primitive objects](#writing-functions-javascript-features-primitive-objects)
+ [Built\-in objects](#writing-functions-javascript-features-builtin-objects)
+ [Error types](#writing-functions-javascript-features-error-types)
+ [Globals](#writing-functions-javascript-features-globals)
+ [Built\-in modules](#writing-functions-javascript-features-builtin-modules)
+ [Restricted features](#writing-functions-javascript-features-restricted-features)

## Core features<a name="writing-functions-javascript-features-core"></a>

The following core features of ES are supported\.

**Types**  
All ES 5\.1 types are supported\. This includes Boolean values, numbers, strings, objects, arrays, functions, function constructors, and regular expressions\.

**Operators**  
All ES 5\.1 operators are supported\.  
The ES 7 exponentiation operator \(`**`\) is supported\.

**Statements**  
The `const` and `let` statements are not supported\.
The following ES 5\.1 statements are supported:  
+ `break`
+ `catch`
+ `continue`
+ `do-while`
+ `else`
+ `finally`
+ `for`
+ `for-in`
+ `if`
+ `return`
+ `switch`
+ `throw`
+ `try`
+ `var`
+ `while`
+ Labeled statements

**Literals**  
ES 6 template literals are supported: multiline strings, expression interpolation, and nesting templates\.

**Functions**  
All ES 5\.1 function features are supported\.  
ES 6 arrow functions are supported, and ES 6 rest parameter syntax is supported\.

**Unicode**  
Source text and string literals can contain Unicode\-encoded characters\. Unicode code point escape sequences of six characters \(for example, `\uXXXX`\) are also supported\.

**Strict mode**  
Functions operate in strict mode by default, so you don’t need to add a `use strict` statement in your function code\. This cannot be changed\.

## Primitive objects<a name="writing-functions-javascript-features-primitive-objects"></a>

The following primitive objects of ES are supported\.

**Object**  
The following ES 5\.1 methods on objects are supported:  
+ `create` \(without properties list\)
+ `defineProperties`
+ `defineProperty`
+ `freeze`
+ `getOwnPropertyDescriptor`
+ `getOwnPropertyNames`
+ `getPrototypeOf`
+ `hasOwnProperty`
+ `isExtensible`
+ `isFrozen`
+ `prototype.isPrototypeOf`
+ `isSealed`
+ `keys`
+ `preventExtensions`
+ `prototype.propertyIsEnumerable`
+ `seal`
+ `prototype.toString`
+ `prototype.valueOf`
The following ES 6 methods on objects are supported:  
+ `assign`
+ `is`
+ `prototype.setPrototypeOf`
The following ES 8 methods on objects are supported:  
+ `entries`
+ `values`

**String**  
The following ES 5\.1 methods on strings are supported:  
+ `fromCharCode`
+ `prototype.charAt`
+ `prototype.concat`
+ `prototype.indexOf`
+ `prototype.lastIndexOf`
+ `prototype.match`
+ `prototype.replace`
+ `prototype.search`
+ `prototype.slice`
+ `prototype.split`
+ `prototype.substr`
+ `prototype.substring`
+ `prototype.toLowerCase`
+ `prototype.trim`
+ `prototype.toUpperCase`
The following ES 6 methods on strings are supported:  
+ `fromCodePoint`
+ `prototype.codePointAt`
+ `prototype.endsWith`
+ `prototype.includes`
+ `prototype.repeat`
+ `prototype.startsWith`
The following ES 8 methods on strings are supported:  
+ `prototype.padStart`
+ `prototype.padEnd`
The following ES 9 methods on strings are supported:  
+ `prototype.trimStart`
+ `prototype.trimEnd`
The following nonstandard methods on strings are supported:  
+ `prototype.bytesFrom(array | string, encoding)`

  Creates a byte string from an array of octets or an encoded string\. The string encoding options are `hex`, `base64`, and `base64url`\.
+ `prototype.fromBytes(start[, end])`

  Creates a Unicode string from a byte string where each byte is replaced with the corresponding Unicode code point\.
+ `prototype.fromUTF8(start[, end])`

  Creates a Unicode string from a UTF\-8 encoded byte string\. If the encoding is incorrect, it returns `null`\.
+ `prototype.toBytes(start[, end])`

  Creates a byte string from a Unicode string\. All characters must be in the \[0,255\] range\. If not, it returns `null`\.
+ `prototype.toUTF8(start[, end])`

  Creates a UTF\-8 encoded byte string from a Unicode string\.

**Number**  
All ES 5\.1 methods on numbers are supported\.  
The following ES 6 methods on numbers are supported:  
+ `isFinite`
+ `isInteger`
+ `isNaN`
+ `isSafeInteger`
+ `parseFloat`
+ `parseInt`
+ `prototype.toExponential`
+ `prototype.toFixed`
+ `prototype.toPrecision`
+ `EPSILON`
+ `MAX_SAFE_INTEGER`
+ `MAX_VALUE`
+ `MIN_SAFE_INTEGER`
+ `MIN_VALUE`
+ `NEGATIVE_INFINITY`
+ `NaN`
+ `POSITIVE_INFINITY`

## Built\-in objects<a name="writing-functions-javascript-features-builtin-objects"></a>

The following built\-in objects of ES are supported\.

**Math**  
All ES 5\.1 math methods are supported\.  
In the CloudFront Functions runtime environment, the `Math.Random()` implementation uses OpenBSD `arc4random` seeded with the timestamp of when the function runs\.
The following ES 6 math methods are supported:  
+ `acosh`
+ `asinh`
+ `atanh`
+ `cbrt`
+ `clz32`
+ `cosh`
+ `expm1`
+ `fround`
+ `hypot`
+ `imul`
+ `log10`
+ `log1p`
+ `log2`
+ `sign`
+ `sinh`
+ `tanh`
+ `trunc`
+ `E`
+ `LN10`
+ `LN2`
+ `LOG10E`
+ `LOG2E`
+ `PI`
+ `SQRT1_2`
+ `SQRT2`

**Date**  
All ES 5\.1 `Date` features are supported\.  
For security reasons, `Date` always returns the same value—the function’s start time—during the lifetime of a single function run\. For more information, see [Restricted features](#writing-functions-javascript-features-restricted-features)\.

**Function**  
The `apply`, `bind`, and `call` methods are supported\.  
Function constructors are not supported\.

**Regular expressions**  
All ES 5\.1 regular expression features are supported\. The regular expression language is Perl compatible\. ES 9 named capture groups are supported\.

**JSON**  
All ES 5\.1 JSON features are supported, including `parse` and `stringify`\.

**Array**  
The following ES 5\.1 methods on arrays are supported:  
+ `isArray`
+ `prototype.concat`
+ `prototype.every`
+ `prototype.filter`
+ `prototype.forEach`
+ `prototype.indexOf`
+ `prototype.join`
+ `prototype.lastIndexOf`
+ `prototype.map`
+ `prototype.pop`
+ `prototype.push`
+ `prototype.reduce`
+ `prototype.reduceRight`
+ `prototype.reverse`
+ `prototype.shift`
+ `prototype.slice`
+ `prototype.some`
+ `prototype.sort`
+ `prototype.splice`
+ `prototype.unshift`
The following ES 6 methods on arrays are supported:  
+ `of`
+ `prototype.copyWithin`
+ `prototype.fill`
+ `prototype.find`
+ `prototype.findIndex`
The following ES 7 methods on arrays are supported:  
+ `prototype.includes`

**Typed arrays**  
The following ES 6 typed arrays are supported:  
+ `Int8Array`
+ `Uint8Array`
+ `Uint8ClampedArray`
+ `Int16Array`
+ `Uint16Array`
+ `Int32Array`
+ `Uint32Array`
+ `Float32Array`
+ `Float64Array`
+ `prototype.copyWithin`
+ `prototype.fill`
+ `prototype.join`
+ `prototype.set`
+ `prototype.slice`
+ `prototype.subarray`
+ `prototype.toString`

**ArrayBuffer**  
The following methods on `ArrayBuffer` are supported:  
+ `prototype.isView`
+ `prototype.slice`

**Promise**  
The following methods on promises are supported:  
+ `reject`
+ `resolve`
+ `prototype.catch`
+ `prototype.finally`
+ `prototype.then`

**Crypto**  
The cryptographic module provides standard hashing and hash\-based message authentication code \(HMAC\) helpers\. You can load the module using `require('crypto')`\. The module exposes the following methods that behave exactly as their Node\.js counterparts:  
+ `createHash(algorithm)`
+ `hash.update(data)`
+ `hash.digest([encoding])`
+ `createHmac(algorithm, secret key)`
+ `hmac.update(data)`
+ `hmac.digest([encoding])`
For more information, see [Crypto \(hash and HMAC\)](#writing-functions-javascript-features-builtin-modules-crypto) in the built\-in modules section\.

**Console**  
This is a helper object for debugging\. It only supports the `log()` method, to record log messages\.

## Error types<a name="writing-functions-javascript-features-error-types"></a>

The following error objects are supported:
+ `Error`
+ `EvalError`
+ `InternalError`
+ `MemoryError`
+ `RangeError`
+ `ReferenceError`
+ `SyntaxError`
+ `TypeError`
+ `URIError`

## Globals<a name="writing-functions-javascript-features-globals"></a>

The `globalThis` object is supported\.

The following ES 5\.1 global functions are supported:
+ `decodeURI`
+ `decodeURIComponent`
+ `encodeURI`
+ `encodeURIComponent`
+ `isFinite`
+ `isNaN`
+ `parseFloat`
+ `parseInt`

The following global constants are supported:
+ `NaN`
+ `Infinity`
+ `undefined`

## Built\-in modules<a name="writing-functions-javascript-features-builtin-modules"></a>

The following built\-in modules are supported\.

**Topics**
+ [Crypto \(hash and HMAC\)](#writing-functions-javascript-features-builtin-modules-crypto)
+ [Query string](#writing-functions-javascript-features-builtin-modules-query-string)

### Crypto \(hash and HMAC\)<a name="writing-functions-javascript-features-builtin-modules-crypto"></a>

The cryptographic module \(`crypto`\) provides standard hashing and hash\-based message authentication code \(HMAC\) helpers\. You can load the module using `require('crypto')`\. The module provides the following methods that behave exactly as their Node\.js counterparts\.

**Hashing methods**

`crypto.createHash(algorithm)`  
Creates and returns a hash object that you can use to generate hash digests using the given algorithm: `md5`, `sha1`, or `sha256`\.

`hash.update(data)`  
Updates the hash content with the given `data`\.

`hash.digest([encoding])`  
Calculates the digest of all of the data passed using `hash.update()`\. The encoding can be `hex`, `base64`, or `base64url`\.

**HMAC methods**

`crypto.createHmac(algorithm, secret key)`  
Creates and returns an HMAC object that uses the given `algorithm` and `secret key`\. The algorithm can be `md5`, `sha1`, or `sha256`\.

`hmac.update(data)`  
Updates the HMAC content with the given `data`\.

`hmac.digest([encoding])`  
Calculates the digest of all of the data passed using `hmac.update()`\. The encoding can be `hex`, `base64`, or `base64url`\.

### Query string<a name="writing-functions-javascript-features-builtin-modules-query-string"></a>

**Note**  
The [CloudFront Functions event object](functions-event-structure.md) automatically parses URL query strings for you\. That means that in most cases you don’t need to use this module\.

The query string module \(`querystring`\) provides methods for parsing and formatting URL query strings\. You can load the module using `require('querystring')`\. The module provides the following methods\.

`querystring.escape(string)`  
URL\-encodes the given `string`, returning an escaped query string\. The method is used by `querystring.stringify()` and should not be used directly\.

`querystring.parse(string[, separator[, equal[, options]]])`  
Parses a query string \(`string`\) and returns an object\.  
The `separator` parameter is a substring for delimiting key and value pairs in the query string\. By default it is `&`\.  
The `equal` parameter is a substring for delimiting keys and values in the query string\. By default it is `=`\.  
The `options` parameter is an object with the following keys:    
`decodeURIComponent function`  
A function to decode percent\-encoded characters in the query string\. By default it is `querystring.unescape()`\.  
`maxKeys number`  
The maximum number of keys to parse\. By default it is `1000`\. Use a value of `0` to remove the limitations for counting keys\.
By default, percent\-encoded characters within the query string are assumed to use the UTF\-8 encoding\. Invalid UTF\-8 sequences are replaced with the `U+FFFD` replacement character\.  
For example, for the following query string:  

```
'name=value&abc=xyz&abc=123'
```
The return value of `querystring.parse()` is:  

```
{
name: 'value',
abc: ['xyz', '123']
}
```
`querystring.decode()` is an alias for `querystring.parse()`\.

`querystring.stringify(object[, separator[, equal[, options]]])`  
Serializes an `object` and returns a query string\.  
The `separator` parameter is a substring for delimiting key and value pairs in the query string\. By default it is `&`\.  
The `equal` parameter is a substring for delimiting keys and values in the query string\. By default it is `=`\.  
The `options` parameter is an object with the following keys:    
`encodeURIComponent function`  
The function to use for converting URL\-unsafe characters to percent\-encoding in the query string\. By default it is `querystring.escape()`\.
By default, characters that require percent\-encoding within the query string are encoded as UTF\-8\. To use a different encoding, specify the `encodeURIComponent` option\.  
For example, for the following code:  

```
querystring.stringify({ name: 'value', abc: ['xyz', '123'], anotherName: '' });
```
The return value is:  

```
'name=value&abc=xyz&abc=123&anotherName='
```
`querystring.encode()` is an alias for `querystring.stringify()`\.

`querystring.unescape(string)`  
Decodes URL percent\-encoded characters in the given `string`, returning an unescaped query string\. This method is used by `querystring.parse()` and should not be used directly\.

## Restricted features<a name="writing-functions-javascript-features-restricted-features"></a>

The following JavaScript language features are either unsupported or restricted due to security concerns\.

**Dynamic code evaluation**  
Dynamic code evaluation is not supported\. Both `eval()` and `Function` constructors throw an error if attempted\. For example, `const sum = new Function('a', 'b', 'return a + b')` throws an error\.

**Timers**  
The `setTimeout()`, `setImmediate()`, and `clearTimeout()` functions are not supported\. There is no provision to defer or yield within a function run\. Your function must synchronously run to completion\.

**Date and timestamps**  
For security reasons, there is no access to high\-resolution timers\. All `Date` methods to query the current time always return the same value during the lifetime of a single function run\. The returned timestamp is the time when the function started running\. Consequently, you cannot measure elapsed time in your function\.

**File system access**  
There is no file system access\. For example, there is no `fs` module for file system access like there is in Node\.js\.

**Network access**  
There is no support for network calls\. For example, XHR, HTTP\(S\), and socket are not supported\.
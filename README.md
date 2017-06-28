# CommonJS Tree Shaker

## Usage

```js
const acorn = require('acorn');
const Analyzer = require('common-shake').Analyzer;

const a = new Analyzer();

a.run(acorn.parse(`
  'use strict';
  const lib = require('./a.js');
  exports.a = lib.a;
`, { locations: true }), 'index.js');

a.run(acorn.parse(`
  'use strict';
  exports.a = 42;
`, { locations: true }), 'a.js');

a.resolve('index.js', './a.js', 'a.js');

console.log(a.getModule('index.js').getInfo());
// { bailouts: false, declarations: [ 'a' ], uses: [] }

console.log(a.getModule('a.js').getInfo());
// { bailouts: false, declarations: [ 'a' ], uses: [ 'a' ] }

const module = a.getModule('a.js');
a.getDeclarations().forEach((decl) => {
  console.log(module.isUsed(decl.name) ? 'used' : 'not used');
  console.log(decl.name, decl.ast);
});
```

## LICENSE

This software is licensed under the MIT License.

Copyright Fedor Indutny, 2017.

Permission is hereby granted, free of charge, to any person obtaining a
copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to permit
persons to whom the Software is furnished to do so, subject to the
following conditions:

The above copyright notice and this permission notice shall be included
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN
NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM,
DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR
OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE
USE OR OTHER DEALINGS IN THE SOFTWARE.

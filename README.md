This repo contains a repro case for a possible bug in Babel involving
`babel-plugin-transform-es2015-destructuring`,
`babel-plugin-transform-es2015-modules-commonjs`, and
`babel-plugin-transform-es3-property-literals`.

The es3-property-literals plugin normally converts keys that are like `default`
to be quoted, which provides better compatibility  for browsers that do not
support ES5 (e.g. IE8). This seems to work when the source code contains the
property that needs to be quoted, but when used in conjunction with these other
two plugins in some situations the property is not quoted.


In `src/okay.js` we have the following code:

```js
import foo from 'foo';
const bar = [1, 2, 3];
const [x] = bar;
```

Part of which gets transpiled to the following line in `build/okay.js`:

```js
function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { 'default': obj }; }
```

In `src/bad.js` we have the following code:

```js
import foo from 'foo';
const [x] = bar;
```

Part of which gets transpiled to the following line in `build/bad.js` (note the
missing quotes around `default`:

```js
function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }
```

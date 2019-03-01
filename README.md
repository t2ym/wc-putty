[![npm version](https://badge.fury.io/js/wc-putty.svg)](https://badge.fury.io/js/wc-putty)

# wc-putty

Complementary polyfills for [@webcomponents/webcomponentsjs](https://github.com/webcomponents/webcomponentsjs)

## List of Polyfills

| Property/Method | Target Class | Target Browsers | Features |
|:----------------|:-------------|:----------------|:------|
| `attributeChangedCallback()` | Custom element base class | IE 11, Edge, Safari 9 | Attribute changes via properties |
| `children`      | `DocumentFragment` | IE 11     | |
| `children`      | `SVGElement` | IE 11           | |
| `from()`        | `Array`      | IE 11           | `Iterable` protocol |
| `constructor()` | `Set`        | IE 11           | `Iterable` initializer |
| `Symbol.species`| `Set`        | IE 11           | |
| `Symbol.iterator` | `Set`      | IE 11           | |
| `values()`      | `Set`        | IE 11           | |
| `add()`         | `Set`        | IE 11           | Key equality for `-0` and `0` |
| `has()`         | `Set`        | IE 11           | Key equality for `-0` and `0` |
| `constructor()` | `Map`        | IE 11           | `Iterable` initializer |
| `Symbol.species`| `Map`        | IE 11           | |
| `Symbol.iterator` | `Map`      | IE 11           | |
| `keys()`        | `Map`        | IE 11           | |
| `values()`      | `Map`        | IE 11           | |
| `entries()`     | `Map`        | IE 11           | |
| `set()`         | `Map`        | IE 11           | Key equality for `-0` and `0` |
| `has()`         | `Map`        | IE 11           | Key equality for `-0` and `0` |

## Install

```
    npm install wc-putty
```

## Import

```js
    import { polyfill } from 'wc-putty/polyfill.js';
```

## Usage

```js
    // Polyfilled only on IE11, Edge, and Safari 9
    class MyElement extends polyfill(HTMLElement) {
      static get observedAttributes() { return [ 'lang' ] }
      attributeChangedCallback(name, oldValue, newValue) {
        // Attribute changes via built-in properties such as lang are detected properly
        // Attribute changes via setAttribute() are detected and the method is called only once per change
      }
    }
    customElements.define('my-element', MyElement);
    // Triggers attributeChangedCallback('lang', null, 'en-US')
    document.createElement('my-element').lang = 'en-US';

    // Polyfilled only on IE 11
    // Set
    let set = new Set([0, -0, 1]);
    set.size === 2; // key equality for -0 and 0
    set.has(-0) && set.has(0) && set.has(1);
    [...set]; // spread iterable
    for (let item of set) {} // iterate over a Set object
    // Map
    let map = new Map((function * () { yield * [0, -0, 1]; })());
    map.size === 2; // key equality for -0 and 0
    map.has(-0) && map.has(0) && map.has(1);
```

## Notes

- The polyfills are complementary to [@webcomponents/webcomponentsjs](https://github.com/webcomponents/webcomponentsjs)
  - `@webcomponents/webcomponentsjs/webcomponents-{bundle|loader}.js` have to be loaded beforehand
- The polyfills are adaptive
  - They do not touch the target classes if the features are supported natively or currently polyfilled and functional
- Global objects are polyfilled except for `attributeChangedCallback()`
- The main module `polyfill.js` is provided as an ES module and evaluated only once to polyfill the target global objects
- Transpilation for the target browsers is required
  - Basic polyfills such as babel runtime have to be loaded beforehand
- Bundling is required if the target browser does not support ES modules natively
- Attribute changes via `setAttributeNS()` can trigger 2 calls of `attributeChangedCallback()` if polyfilled by `polyfill()` mixin

## License

[BSD-2-Clause](https://github.com/t2ym/wc-putty/blob/master/LICENSE.md)

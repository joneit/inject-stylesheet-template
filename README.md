Resolve registered CSS stylesheet template & inject strategically into head.

### Definitions

_resolve:_ uses [`automat`]() to merge values of optional parameters into stylesheet

_registered:_ dereferenced from the "registry" (_i.e.,_ the calling context)

_strategically:_ inserts each stylesheet consecutively into `<head>` either before first stylesheet loaded with page _or_ at end of `<head>` if nonesuch

### Return value

Returns a reference to the injected `<style>` element.

### Examples

#### Non-strict mode

In non-strict mode, the registry (calling context) defaults to the global object:

```javascript
var inject = require('inject-stylesheet-template');
var day = 'body { background-color: #ffe }';
var styleEl = inject('day'); // inject <style id="day">body { background-color: #ffe }</style> into <head>
```

#### Strict mode

In strict mode, calling context is undefined so the registry _must_ be specified:

```javascript
var styleEl = inject.call(window, 'day');
```

or:

```javascript
var registry = {
    day: 'body { background-color: #ffe }',
    eve: 'body { background-color: #666; color: #eee; }'
};
var hr = (new Date).getHours();
var styleEl = inject.call(registry, 6 < hr || hr < 18 ? 'day' : 'eve');
```

#### Inject strategically

```
inject(true, 'day'); // before first stylesheet in <head> loaded with page
inject(false, 'day'); // end of <head>
inject('day'); // per inject.before (true by default)
```

#### Merge parameter values

```
inject(true, 'day'); // before first stylesheet in <head> loaded with page
inject(false, 'day'); // end of <head>
inject('day'); // per inject.before (true by default)
```

#### Remove injected stylesheet

```javascript
styleEl.remove(); // IE-unfriendly
styleEl.parentNode.removeChild(styleEl); // IE-friendly
```
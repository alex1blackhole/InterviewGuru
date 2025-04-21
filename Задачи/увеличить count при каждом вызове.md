#JavaScript 

```js
var fn = (function() {
    var count = 0;
    return function() {
        return ++count
    }
})();
```

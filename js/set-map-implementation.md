## How is Set and Map implemented

I was wondering how is Set implemented. I wanted to know what was the time complexity (big-O). It is useful to know that when you are looking in performance bottlenecks in your code.

In the official documentation of ecma-262 standard, [section 23.2](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-set-objects) it is stated that:

> Set objects must be implemented using either hash tables or other mechanisms that, on average, provide access times that are sublinear on the number of elements in the collection

V8 is javascript engine used in Chromium based web browsers and node.js.

in V8, as far as I could find, Set and Map are implemented using variant of hash tables that generally O(1) time complexity.

### Reference

 * [V8 implementation of OrderedHashTable](https://codereview.chromium.org/220293002/) based on [Deterministic hash tables](https://wiki.mozilla.org/User:Jorend/Deterministic_hash_tables)
 * [Hash code in V8](https://v8.dev/blog/hash-code)
 * Stackoverflow answer: [es6 Map and Set complexity, v8 implementation](https://stackoverflow.com/questions/33611509/es6-map-and-set-complexity-v8-implementation)
 * Stackoverflow answer: [Javascript ES6 computational/time complexity of collections](https://stackoverflow.com/questions/31091772/javascript-es6-computational-time-complexity-of-collections/31092145#31092145)
 
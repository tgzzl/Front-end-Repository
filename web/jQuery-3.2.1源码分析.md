# jQuery源码分析

## 整体架构
![image](http://dl.iteye.com/upload/attachment/0073/2601/3fc8106d-6afd-314c-a6bf-a64157145e67.jpg)


```JavaScript
// Define a local copy of jQuery
var jQuery = function (selector, context) {
  // The jQuery object is actually just the init constructor 'enhanced'
  // Need init if jQuery is called (just allow error to be thrown if not included)
  return new jQuery.fn.init(selector, context)
}
jQuery.fn = jQuery.prototype = {
  jquery: version,
  constructor: jQuery
}

var init = jQuery.fn.init = function (selector, context, root) {
  // ...
  return this
}
// Give the init function the jQuery prototype for later instantiation
init.prototype = jQuery.fn
```

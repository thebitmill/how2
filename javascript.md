# JavaScript

## client-, offset- & scrollHeight/-Width

+ __clientHeight__: Returns the height of the visible area for an object, in
	pixels. The value contains the height with the padding, but it does not
	include the scrollBar, border, and the margin.
+ __offsetHeight__: Returns the height of the visible area for an object, in
	pixels. The value contains the height with the padding, scrollBar, and the
	border, but does not include the margin.

So, offsetHeight includes scrollbar and border, clientHeight doesn't.

## Viewport

Source: <http://stackoverflow.com/questions/1248081/get-the-browser-viewport-dimensions-with-javascript>

### Cross-browser @media (width) and @media (height) values

```js
var w = Math.max(document.documentElement.clientWidth, window.innerWidth || 0)
var h = Math.max(document.documentElement.clientHeight, window.innerHeight || 0)
```

### window.innerWidth and .innerHeight

+ gets CSS viewport @media (width) and @media (height) which include scrollbars
+ initial-scale and zoom variations may cause mobile values to wrongly scale down to what PPK calls the visual viewport and be smaller than the @media values
+ zoom may cause values to be 1px off due to native rounding
+ undefined in IE8-

### document.documentElement.clientWidth and .clientHeight

+ equals CSS viewport width minus scrollbar width
+ matches @media (width) and @media (height) when there is no scrollbar
+ same as jQuery(window).width() which jQuery calls the browser viewport
+ available cross-browser

### Other Resource

+ [Live outputs for various dimensions](http://ryanve.com/lab/dimensions/)
+ [verge](http://github.com/ryanve/verge) uses cross-browser viewport techniques
+ [actual](http://github.com/ryanve/actual) uses matchMedia to obtain precise dimensions in any unit

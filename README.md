# RequestQueue
A simple queue for XMLHttpRequests or other async functions. Optimized for Greasemonkey GM_xmlhttpRequests


## Documentation

```js
new RequestQueue([maxParallel[, maxTotal]])
```
Create queue object
 * *maxParallel*
   defaults to 1 i.e. no paralllel requests
 * *maxTotal*
   defaults to unlimited

---


```js
RequestQueue.prototype.add(req[, fun[, thisArg]])
```
Schedule a request
* *req*
  An object that is the only argument of fun. This will be extended and therefore must be mutable.
  At least one of the following methods onload, onerror or onabort must be implemented.
* *fun*
  defaults to `GM_xmlhttpRequest`
* *thisArg*
  The value of this provided for the call to fun. Keep strict mode in mind!

Results in a call like this: `thisArg.fun(req)`

---


```js
RequestQueue.prototype.abortRunning()
```
Abort running requests

For all scheduled requests that are currently running: `result = thisArg.fun(req)`
this will call `result.abort()`
For GM_xmlhttpRequest this will subsequently fire an onabort event

---


```js
RequestQueue.prototype.abortPending()
```
Abort pending requests

Clear the list of pending requests i.e. requests that were not sent yet.

---


```js
RequestQueue.prototype.abort()
```
Abort both running and pending requests



## Example

```js
var rq = new RequestQueue(1); // 1 -> Allow no parallel requests
rq.add({
  method: "GET",
  url: "http://www.example.org/page1",
  onload: function (response){
    console.log("onload 1");
  }
});
rq.add({
  method: "GET",
  url: "http://www.example.org/page2",
  onload: function (response){
    console.log("onload 2");
  }
});
```

The request to page 1 will be sent immediately.
The request to page 2 will be sent after the the first request has finished, but before the first onload event is called.


## Version history
* Version 5
 * New methods: resetTotal(), hasReachedTotal(), hasRunning()
* Version 4
 * Fix for XMLHttpRequest.abort() 
* Version 3
 * Fix for Chrome
* Version 2
 * Fix for Firefox 36.0 
* Version 1
 * initial version

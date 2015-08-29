js中监控变量的变化

#### QA & Tool.
* [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/watch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/watch)
* [http://stackoverflow.com/questions/1269633/watch-for-object-properties-changes-in-javascript](http://stackoverflow.com/questions/1269633/watch-for-object-properties-changes-in-javascript)


#### Replace plain URLs with links 

js code.

```
/*
* object.watch v0.0.1: Cross-browser object.watch
*
* By Elijah Grey, http://eligrey.com
*
* A shim that partially implements object.watch and object.unwatch
* in browsers that have accessor support.
*
* Public Domain.
* NO WARRANTY EXPRESSED OR IMPLIED. USE AT YOUR OWN RISK.
*/
    
// object.watch
if (!Object.prototype.watch)
    Object.prototype.watch = function (prop, handler) {
        var oldval = this[prop], newval = oldval,
            getter = function () {
                return newval;
            },
            setter = function (val) {
                oldval = newval;
                return newval = handler.call(this, prop, oldval, val);
            };
        if (delete this[prop]) { // can't watch constants
            if (Object.defineProperty) // ECMAScript 5
                Object.defineProperty(this, prop, {
                    get: getter,
                    set: setter
                });
            else if (Object.prototype.__defineGetter__ && Object.prototype.__defineSetter__) { // legacy
                Object.prototype.__defineGetter__.call(this, prop, getter);
                Object.prototype.__defineSetter__.call(this, prop, setter);
            }
        }
    };
    
// object.unwatch
if (!Object.prototype.unwatch)
    Object.prototype.unwatch = function (prop) {
        var val = this[prop];
        delete this[prop]; // remove accessors
        this[prop] = val;
    };
```

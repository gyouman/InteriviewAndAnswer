```javascript
    var eventUtil = {
        add : function(el, type, handler) {
            if(el.addEventListener) {
                el.addEventListener(type, handler, false);
            }else if( el.attachEvent ) {
                el.attachEvent("on"+type, handler);
            }else{
                el["on"+type] = handler;
            }
        },
        off : function(el, type, handler) {
            if( el.removeEventListener ) {
                el.removeEventListener(type, handler, false)
            }else if( el.detachEvent ) {
                el.detachEvent(type, handler);
            }else{
                el["on"+type] = null;
            }
        }
     };
```
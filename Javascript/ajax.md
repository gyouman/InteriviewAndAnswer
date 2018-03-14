```javascript
    function ajax(opt){
        opt = opt || {}
        opt.type = opt.type.toUpperCase()||'POST';
        opt.url = opt.url || '';
        opt.async = opt.async || true;
        opt.data = opt.data||null;
        opt.success = opt.success || function(){}
        var xmlHttp = null;
        if(window.xmlHttpRequest){
            xmlHttp = new XMLHttpRequest()
        }else{
            xmlHttp = new ActiveXObject('Micorsoft.XMLHTTP)
        }
        var params = []
        for(var key in opt.data){
            parmas.push(key + "=" opt.data[key])
        }
        var postData = params.join('&');
        if(opt.type.toUpperCase()=='GET'){
            xmlHttp.open(opt.type,opt.url + postData,opt.async)
            xmlHttp.send(null)
        }else if(opt.type.toUpperCase*(=='POST')){
            xmlHttp.open(opt.type.opt.url,opt.async)
            xmlHttp.setRequestHeader('Content-TYpe','application/x-www-form-urencoded;charset="utf-8"')
            xmlHttp.send(data)
        }
        xmlHttp.onreadyStateChange = function(){
            if(xmlHttp.readyState==4&& xmlHttp.status==200){
                opt.success(xmlHttp.responseText)
            }
        }
    }
```
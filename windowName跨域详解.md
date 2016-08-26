# window.name跨域详解

@(github.com/tsrot)[window.name|跨域]

地址：http://blog.xieliqun.com/2016/08/25/windowName-cross-domain/

>  window.name属性的神奇之处在于name 值在不同的页面（甚至不同域名）加载后依旧存在（如果没修改则值不会变化），并且可以支持非常长的 name 值（2MB）。并且window.name很方便。


----------

### 用法介绍

name 在浏览器环境中是一个全局/window对象的属性，且当在 frame 中加载新页面（可以是不同域的）时，name 的属性值依旧保持不变（只要不重新赋值）。name属性的值虽然不变，但对它的访问还是受到同域原则，不允许访问。所以我们要把iframe重新定向回原域，这样name的值也没变，并且可以访问了。

在最顶层，name 属性是不安全的，对于所有后续页面，设置在 name 属性中的任何信息都是可获得的。然而 windowName 模块总是在一个 iframe 中加载资源，并且一旦获取到数据，或者当你在最顶层浏览了一个新页面，这个 iframe 将被销毁，所以其他页面永远访问不到 window.name 属性。


### 函数封装

为了方便以后的使用，我们把window.name进行一下封装。

```javascript
function proxy(url, func){
  var isFirst = true,//判断url第一次是否加载完
      ifr = document.createElement('iframe'),
      loadFunc = function(){
        if(isFirst){ 
        //加载完url后，重新定向回原域
          ifr.contentWindow.location = 'about:blank';
          isFirst = false;
        }else{ 
        //iframe回到原域后，获取name的值，执行回调函数，然后销毁iframe
          func(ifr.contentWindow.name);
          ifr.contentWindow.close();
          document.body.removeChild(ifr);
          ifr.src = '';
          ifr = null;
        }
      };

  ifr.src = url; //第一次加载url
  ifr.style.display = 'none';
  document.body.appendChild(ifr);

  //监听iframe是否加载，加载完执行loadFunc
  if(ifr.attachEvent){
  	ifr.attachEvent('onload', loadFunc);
  }else{
  	ifr.onload = loadFunc;
  }

}
```


### 详细案例

假设我现在在http://127.0.0.1:8080 ，我有一个页面叫做a.html，现在我想获取http://127.0.0.1:9090 上的b.html的数据。

a.html
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
<script>
function proxy(url, func){
  var isFirst = true,
      ifr = document.createElement('iframe'),
      loadFunc = function(){
        if(isFirst){
          ifr.contentWindow.location = 'about:blank';
          isFirst = false;
        }else{
          func(ifr.contentWindow.name);
          ifr.contentWindow.close();
          document.body.removeChild(ifr);
          ifr.src = '';
          ifr = null;
        }
      };

  ifr.src = url;
  ifr.style.display = 'none';
  document.body.appendChild(ifr);

  if(ifr.attachEvent){
  	ifr.attachEvent('onload', loadFunc);
  }else{
  	ifr.onload = loadFunc;
  }

}
</script>
</head>
<body>
<script>
  proxy('http://127.0.0.1:9090/b.html', function(data){
    console.log(JSON.parse(data).name);
    console.log(JSON.parse(data).age);
  });
</script>
</body>
</html>
```

b.html
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
<script>
	var obj = {
		name:"tsrot",
		age:24
	};
    window.name = JSON.stringify(obj);
</script>
</body>
</html>
```
id选择器

```javascript
let value = $("#id")
let value = $("p")
value.html("") //innerHTML
```

css属性选择器

```javascript
$("p").css("background-color","blue")
```

window.onload替代$()

获取内容

```javascript
let content = $("#id").val();
```

设置内容

```javascript
$("#id").val("content")
```

将String封装并添加进div

```javascript
let jqObj = $("<p>"+content)+"<p>">
$("div").append(jqObj)
```

![image-20220402151502372](/Users/ayanamirei/Library/Application Support/typora-user-images/image-20220402151502372.png)

修改type属性

![image-20220402154311709](/Users/ayanamirei/Library/Application Support/typora-user-images/image-20220402154311709.png)

添加和移除class

![image-20220402154658413](/Users/ayanamirei/Library/Application Support/typora-user-images/image-20220402154658413.png)

全选

![image-20220402155510086](/Users/ayanamirei/Library/Application Support/typora-user-images/image-20220402155510086.png)

```
jQuery.ajax(...)
      部分参数：
            url：请求地址
            type：请求方式，GET、POST（1.9.0之后用method）
        headers：请求头
            data：要发送的数据
    contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www-form-urlencoded; charset=UTF-8")
          async：是否异步
        timeout：设置请求超时时间（毫秒）
      beforeSend：发送请求前执行的函数(全局)
        complete：完成之后执行的回调函数(全局)
        success：成功之后执行的回调函数(全局)
          error：失败之后执行的回调函数(全局)
        accepts：通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
        dataType：将服务器端返回的数据转换成指定类型
          "xml": 将服务器端返回的内容转换成xml格式
          "text": 将服务器端返回的内容转换成普通文本格式
          "html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行。
        "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
          "json": 将服务器端返回的内容转换成相应的JavaScript对象
        "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
```

> ```
> jQuery.ajax(...)
>       部分参数：
>             url：请求地址
>             type：请求方式，GET、POST（1.9.0之后用method）
>         headers：请求头
>             data：要发送的数据
>     contentType：即将发送信息至服务器的内容编码类型(默认: "application/x-www-form-urlencoded; charset=UTF-8")
>           async：是否异步
>         timeout：设置请求超时时间（毫秒）
>       beforeSend：发送请求前执行的函数(全局)
>         complete：完成之后执行的回调函数(全局)
>         success：成功之后执行的回调函数(全局)
>           error：失败之后执行的回调函数(全局)
>         accepts：通过请求头发送给服务器，告诉服务器当前客户端可接受的数据类型
>         dataType：将服务器端返回的数据转换成指定类型
>           "xml": 将服务器端返回的内容转换成xml格式
>           "text": 将服务器端返回的内容转换成普通文本格式
>           "html": 将服务器端返回的内容转换成普通文本格式，在插入DOM中时，如果包含JavaScript标签，则会尝试去执行。
>         "script": 尝试将返回值当作JavaScript去执行，然后再将服务器端返回的内容转换成普通文本格式
>           "json": 将服务器端返回的内容转换成相应的JavaScript对象
>         "jsonp": JSONP 格式使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数
> ```
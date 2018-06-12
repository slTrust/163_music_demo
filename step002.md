### 获取外链

如果你不想要自己设置上传文件的信息，你可以注释掉以下代码，这样获取的就是你上传的文件名

```
'Key': function (up, file) {
    // 若想在前端对每个文件的key进行个性化处理，可以配置该函数
    // 该配置必须要在 unique_names: false , save_key: false 时才生效

    var key = "";
    // do something with key here
    return key
}
```


上传成功后，七牛会返回对应的相关信息(注意七牛这里的信息是伪代码，需要你自己一步一步试出来)

```
'FileUploaded': function (up, file, info) {
    uploadStatus.textContent = '上传完毕';
    // 每个文件上传成功后,处理相关的事情
    // 其中 info.response 是文件上传成功后，服务端返回的json，形式如
    // {
    //    "hash": "Fh8xVqod2MQ1mocfI4S4KpRL6D98",
    //    "key": "gogopher.jpg"
    //  }
    // 参考http://developer.qiniu.com/docs/v6/api/overview/up/response/simple-response.html

    // var domain = up.getOption('domain');
    // var res = parseJSON(info.response);
    // var sourceLink = domain + res.key; // 获取上传成功后的文件的Url
},
```

> 修改domain,bucket 域名(就是你七牛创建的那个应用的默认域名)

```
domain: 'p5h9wznrb.bkt.clouddn.com',
```


> 问题来了，你上传的文件有可能是中文,但是歌曲的外链不能有中文需要进行编码

```
var domain = up.getOption('domain');
var res = JSON.parse(info.response);
// res.key 是文件名  但是是中文 在url里无法打开
var sourceLink = domain + res.key; // 获取上传成功后的文件的Url
```

稍微正确的处理

```
//var sourceLink = domain + encodeURI(res.key);
//但是打印后发现还是有问题形如 //p5h9wznrb.bkt.clouddn.com%E7%88%B1%E6%AD%BB%E4%BA%86%E6%98%A8%E5%A4%A9.m4a
//你该这样加上http 和 /
 var sourceLink = `http://${domain}/${encodeURI(res.key)}`;
```


> 真正正确的处理


```
encodeURIComponent 和 encodeURI 的区别

http://x.com/search?q=  http://a.com/%EF  ?a=1
当一个网址是另一个网址的一部分的时候 如果这个子网址有查询参数就会造成歧义
解决的办法就是使用 encodeURIComponent
http://x.com/search?q=+encodeURIComponent(http://a.com/%EF?a=1)
```

```
//var sourceLink = domain + encodeURI(res.key);
//但是打印后发现还是有问题形如 //p5h9wznrb.bkt.clouddn.com%E7%88%B1%E6%AD%BB%E4%BA%86%E6%98%A8%E5%A4%A9.m4a
//你该这样加上http 和 /
 var sourceLink = `http://${domain}/${encodeURIComponent(res.key)}`;
```



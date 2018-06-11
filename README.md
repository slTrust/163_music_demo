"# 163_music_demo" 

### 仿网易云音乐1.0.0

### 技术栈

- 前端 jq
- 后端 leanCloud API
- 数据库 LeanCloud + 七牛

### 功能点

- 管理员页面(PC)
- 用户页(手机端)

#### 注册leanCloud

1. 创建应用如 「163_music_demo」
2. 进入该应用「163_music_demo」
3. 创建Class(数据) 如果失败你就用代码方式创建
- User 管理员用户
- Song 所有歌曲
- PlayList 所有歌单
4. 看帮助->文档-> （快速入门）javascript->初始化代码
- 安装和引用SDK

```
npm install leancloud-storage --save
```

5. 根据依赖引入av-min.js

- 继续抄文档的例子初始化

注意这个是我应用的APP_ID和APP_KEY(你该用你的才对)

```
var APP_ID = 'xxxx-xxxxx';
var APP_KEY = 'xxxxxxxx';

AV.init({
  appId: APP_ID,
  appKey: APP_KEY
});
```

6. 编写测试代码（一般不用验证，如果验证失败说明这个服务商挂掉了，你也没必要往下看了）

```
// 创建数据库
var TestObject = AV.Object.extend('TestObject');
// 创建一条记录
var testObject = new TestObject();
// 保存记录
testObject.save({
    words: 'Hello World!'
}).then(function (object) {
    alert('LeanCloud Rocks!');
})
```

7. 在leanCloud应用里删掉这个TestObject创建一个我们歌曲的Class

自行脑补or语文填空or数学高级技巧(换元法)

```
// 创建数据库
var TestObject = AV.Object.extend('Playlist');
// 创建一条记录
var testObject = new TestObject();
// 保存记录
testObject.save({
    name:'test',
    cover:'test', //背景图
    creatorId:'test', //创建者
    description:'test',//歌曲描述
    songs:["1","2","3"],  //歌曲列表
}).then(function (object) {
    alert('LeanCloud Rocks!');
})
```

#### 注册七牛账号

1. 注意是要上传身份证的所以别瞎传东西
2. 注意是收费的你创建的应用产生的key不要上传github（别人盗用概不负责）
3. 查看帮助文档,找到js sdk
4. JavaScript SDK 源码地址(特别说明我用的1.x版本)

```
cnpm i qiniu-js@1.0.22
# 1.x版本的七牛初始化后是大写Qiniu
# 注意这里的1.x版本还需要引入Plupload 
cnpm i plupload@2.3.2
```

[官方demo](http://jssdk.demo.qiniu.io/#)

```
var uploader = Qiniu.uploader({
    runtimes: 'html5,flash,html4',    //上传模式,依次退化
    browse_button: 'pickfiles',       //上传选择的点选按钮，**必需**
    uptoken_url: '/token',            //Ajax请求upToken的Url，**强烈建议设置**（服务端提供）
    // uptoken : '', //若未指定uptoken_url,则必须指定 uptoken ,uptoken由其他程序生成
    // unique_names: true, // 默认 false，key为文件名。若开启该选项，SDK为自动生成上传成功后的key（文件名）。
    // save_key: true,   // 默认 false。若在服务端生成uptoken的上传策略中指定了 `sava_key`，则开启，SDK会忽略对key的处理
    domain: 'http://qiniu-plupload.qiniudn.com/',   //bucket 域名，下载资源时用到，**必需**
    get_new_uptoken: false,  //设置上传文件的时候是否每次都重新获取新的token
    container: 'container',           //上传区域DOM ID，默认是browser_button的父元素，
    max_file_size: '100mb',           //最大文件体积限制
    flash_swf_url: 'js/plupload/Moxie.swf',  //引入flash,相对路径
    max_retries: 3,                   //上传失败最大重试次数
    dragdrop: true,                   //开启可拖曳上传
    drop_element: 'container',        //拖曳上传区域元素的ID，拖曳文件或文件夹后可触发上传
    chunk_size: '4mb',                //分块上传时，每片的体积
    auto_start: true,                 //选择文件后自动上传，若关闭需要自己绑定事件触发上传
    init: {
        'FilesAdded': function(up, files) {
            plupload.each(files, function(file) {
                // 文件添加进队列后,处理相关的事情
            });
        },
        'BeforeUpload': function(up, file) {
                // 每个文件上传前,处理相关的事情
        },
        'UploadProgress': function(up, file) {
                // 每个文件上传时,处理相关的事情
        },
        'FileUploaded': function(up, file, info) {
                // 每个文件上传成功后,处理相关的事情
                // 其中 info.response 是文件上传成功后，服务端返回的json，形式如
                // {
                //    "hash": "Fh8xVqod2MQ1mocfI4S4KpRL6D98",
                //    "key": "gogopher.jpg"
                //  }
                // 参考http://developer.qiniu.com/docs/v6/api/overview/up/response/simple-response.html

                // var domain = up.getOption('domain');
                // var res = parseJSON(info.response);
                // var sourceLink = domain + res.key; 获取上传成功后的文件的Url
        },
        'Error': function(up, err, errTip) {
                //上传出错时,处理相关的事情
        },
        'UploadComplete': function() {
                //队列文件处理完毕后,处理相关的事情
        },
        'Key': function(up, file) {
            // 若想在前端对每个文件的key进行个性化处理，可以配置该函数
            // 该配置必须要在 unique_names: false , save_key: false 时才生效

            var key = "";
            // do something with key here
            return key
        }
    }
});
// domain 为七牛空间（bucket)对应的域名，选择某个空间后，可通过"空间设置->基本设置->域名设置"查看获取
// uploader 为一个plupload对象，继承了所有plupload的方法，参考http://plupload.com/docs
```

> 至此我们依然无法上传，因为需要一个uptoken这个东西需要服务器返回

我们必须开一个node.js的服务器返回token才可以继续。。。
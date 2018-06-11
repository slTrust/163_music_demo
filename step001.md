### 本地node服务器返回token

#### 查看七牛node sdk

```
npm install qiniu

//七牛账号 --> 个人中心--> 秘钥管理 就能看到你的AK和 SK了
var accessKey = 'your access key';
var secretKey = 'your secret key';
var mac = new qiniu.auth.digest.Mac(accessKey, secretKey);

// 上传凭证  有了它你就能上传了
var options = {
  scope: bucket,
};
var putPolicy = new qiniu.rs.PutPolicy(options);
var uploadToken=putPolicy.uploadToken(mac);
```

创建server.js文件
```
//读文件   这个文件保存着七牛的key千万不要上传这个文件
let json = fs.readFileSync('./qiniu-config.json')

let {accessKey,secretKey} = JSON.parse(json);
var mac = new qiniu.auth.digest.Mac(accessKey, secretKey);
var options = {
    // 你的bucket
    scope: '163-music-demo-1',
};
var putPolicy = new qiniu.rs.PutPolicy(options);
var uploadToken=putPolicy.uploadToken(mac);
response.write(`{
    "uptoken":"${uploadToken}"
}`)
response.end()

```

#### 千万不要上传你的key
#### 千万不要上传你的key
#### 千万不要上传你的key

在.gitignore里添加忽略提交的七牛配置文件

```
./qiniu-config.json
```
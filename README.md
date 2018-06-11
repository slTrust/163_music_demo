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
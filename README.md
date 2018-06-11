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
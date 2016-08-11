---
title: MongoDB从入门到放弃_02
date: 2016年8月11日
categories: 
     - MongoDB   
toc: true
tags: 
     - MongoDB
---

#### Mongo命令和配置

**1.启动项**
```js
--dbpath 指定数据库的目录
--port 端口 默认是27017
--fork 以后台守护的方式进行启动
--logpath 制定日志文件输出路径
--config 指定一个配置文件
--auth  以安全的方式启动数据库
--rest   会启动一个帮助页面
```
<!-- more -->
**2.关闭数据库**
`db.shutdownServer();`
**3.导入导出**
  导出 mongoexport
   ```js
   -d 指定导出的数据库
   -c 制定导出的集合
   -o 导出的文件路径
   -q 进行过滤
   mongoexport -d local -c person -o bak.json
   ```
  导入 mongoimport
   `mongoimport -d local --collections person --file bak.json`
   导入整个库
   `mongorestore --directoryperdb bak.dmp`
**4.锁住写入**
`db.runCommand({fstnc:1,lock:1});`	
**5.解锁**
`db.fsyncUnlock();  //备份的时候要先锁住写入,备份完之后再解锁可以保证数据完整性`
**6.增加角色**
`db.addUser({'chuhan':'123'});    //已经废弃不建议使用`
`db.createUser({user:'chuhan',pwd:'123',roles:[{role:'userAdmin',db:'admin'}]})`
**7.显示所有的角色**
`db.showRoles`
**8.修改用户密码**
`db.changeUserPassword('chuhan','123');`
**9.授权登录**
`db.auth('chuhan','123');   //如果显示1,表示成功,否则授权失败`
**10.查看用户权限**
```js
db.runCommand({
	usersInfo:'chuhan',
	showPrivileges:true
});
```
**11.修改用户权限**
```js
db.runCommand({
	updateUser:'chuhan',
	pwd:'789',
	customData:{title:'manager',age:12}
})
```
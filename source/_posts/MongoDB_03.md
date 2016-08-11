---
title: MongoDB从入门到放弃_03
date: 2016年8月11日
categories: 
     - MongoDB   
toc: true
tags: 
     - MongoDB
---
### MongoDB常用的指令

**1.去重**
	```js
	db.runCommand({
		distinct:'person',
		key:'home'
	});
	```
<!-- more -->
**2.分组**
	```js
	db.runCommand({
		group:{
			ns:集合名称,
			key:分组的键,
			initial:初始化,
			$reduce:分解器,
			condition:条件,
			finalize:完成时的处理器
		}
	});
	db.runCommand({
		group:{
			ns:'person',
			key:'email',
			initial:{age:0},
			$reduce:function(doc,acc){
				if(doc.age>acc.age){
					acc.age = doc.age;
					acc.email = doc.email;
				}
			},
			condition:{age:{$gt:10}},
			finalize:function(acc){
				print(acc.age);
			}
		}
	});
	```
**3.删除集合**
	```js
	db.runCommand({
		drop:'person'
	})
	```
**4.查看数据库信息**
	```js
	db.runCommand({
		buildInfo:1
	})
	```

**固定集合**

> 建立集合的时候指定大小和文档的数量,如果满了,会把最后的元素抛弃掉,把新的元素加进去
    特性
        没有索引
	插入和查询的速度非常快
	适合写日志
**创建集合**
	`db.createCollection('lessons',{size:50,max:5,capped:true})`
	`//当插入第六条数据的时候,会覆盖第一条数据,是一种队列的数据结构`
**修改集合信息**
	```js
	db.runCommand({
		convertToCapped:'courses',
		size:6
	});
	//把一个非固定集合转成固定集合
	```

**Gridfs**

> 是mongodb自带的文件系统,使用二进制存储文件.可以以BSON格式保存二进制对象.
但是BSON对象的体积不能超过4M.所以mongodb提供了gridfs.可以把大文件透明的
分割成小文件(256K)
上传文件
	`mongofiles -d files -l "E:\test.txt" put "test.txt"`
**查看gridfs有多少文件**
	`mongofiles -d files list`
**删除文件**
	`mongofiles -d files delete "test.txt"`

**服务器端脚本 eval**
	> db.eval("1+1");
	  db.system.js.insert({_id:'x',value:'234'});
	  db.eval('return x');  //234

### Mongo索引
索引是特殊的数据结构,按顺序保存文档中的一个或者多个指定
使用B-Tree索引,方便范围查询和匹配查询
`db.users.find({ score : { $lt:10 }}).sort({ _id:-1 });`
`db.persons.find({name:100000}).explain();`
//查看此次查询所执行的步骤
	 建立单键索引(命名索引,唯一索引)
`db.persons.ensureIndex({name:1},{name:'nameIndex',unique:true});`
	 对数组进行索引
> db.persons.insert({hobby:["football","basketball"]});
  db.persons.ensureIndex({hobby:1});
  db.persons.find({hobby:'football'},{hobby:1,_id:0});
	 复合索引
db.persons.ensureIndex({a:1,b:1});
db.persons.find({a:1,b:2},{a:1,_id:0}).explain();
	 过期索引
在一定时间后会过期,过期后相应的数据被删除
session 日志 缓存 临时文件
> db.p2.insert({time:new Date()});
  db.p2.ensureIndex({time:1},{expireAfterSeconds:11});
1.索引字段的值必须是Date对象,不能是其它类型比如时间戳
2.删除时间不精确
	 二维索引
空间索引
```js
for(var i=1;i<=10;i++){
	for(var j=1;j<=10;j++){
		db.map.insert({gis:{x:i,y:j}});
	}
}
```
建立二维索引
`db.map.ensureIndex({gis:'2d'});`
去最近的3个点
`db.map.find({gis:{$near:[1,1]}},{gis:1,_id:0}).limit(3);`
查询以[1,1],[3,3]为对象矩形里面所有的点
`db.map.find({gis:{$within:{$box:[[1,1],[3,3]]}}},{gis:1,_id:0});`
查询圆心[1,1],半径为1的圆形中所有的点
`db.map.find({gis:{$within:{$center:[[1,1],1]}}},{gis:1,_id:0});`
查询多边形
`db.map.find({gis:{$within:{$polygon:[[2,1],[1,3],[3,3]]}}},{gis:1,_id:0});`
> 索引的使用注意事项
  1.  1为正 -1为倒序
  2.  索引虽然可以提升查询性能,但会降低插件的效率
  3.  建的合理

        主从复制		
> 简单的数据库同步备份的集群技术
  1.集群需要知道主服务器
  2.还需要从服务器
  启动主服务器
`mongod --dbpath=./master --port=8888 --master`
> 启动从服务器
`mongod --dbpath=./slave --port=8889 --slave --source localhost:8888`
> 从服务器需要指定的
  only: 指定复制的数据库
  slavedelay 主库向从库同步时延迟时间
  oplogsize :主节点操作记录存储到local的oplog中
  autoresync
  `db.resoures.find();`
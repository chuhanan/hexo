---
title: MongoDB从入门到放弃_01
date: 2016年8月11日
categories: 
     - MongoDB   
toc: true
tags: 
     - MongoDB
---

#### MongoDB数据库下载地址:http://pan.baidu.com/s/1c2yrmQ
#### MongoDB可视化工具下载:http://pan.baidu.com/s/1bpaBUG3

**1.启动mongo**
	`mongod --dbpath ./data      //data存在`
**2.创建mongo服务**
        ```js
	mongod.exe --dbpath E:\DataTools\mongodata --logpath E:\DataTools\mongolog
	--logappend --directoryperdb --serviceName MongoDB --install
  删除一个服务
	在管理员cmd模式下:sc delete MongoDB(服务名)
	```
<!-- more -->
**3.查看当前的数据库**
	`show dbs`
**4.切换数据库**
	`use local`
**5.查看当前数据库下的所有的collections**
	`show collections`
**6.查看数据**
	```js
	db.person.find();
	db.person.findOne();
	```
**7.插入数据**
	```js
	db.person.insert({name:'楚寒',age:20});
	db.person.save({name:'123',age:12});   //不支持批量插入
	save相同的__id时不会报错,insert则会报错 
	```
  批量插入数据(可以用脚本插入)
	```js
	for(var i=0;i<10;i++){
		db.person.insert({name:'张三'+i,age:10+i});
	}
	```
**8.更新数据**
	>db.person.update({name:'楚寒'},{$set:{email:'384840951@qq.com'}})
	>db.person.update({name:'楚寒'},{address:'hello'}); //这种更新会把之前的数据替换
	>//如果更新的键不存在 那么这个键就会被增加上去
	>db.collection.update(criteria,objNew,upsert,multi)
	>参数说明：
	>criteria：查询条件
	>objNew：update对象和一些更新操作符
	>upsert：如果不存在update的记录，是否插入objNew这个新的文档，true为插入，默认为false，不插入。
	>multi：默认是false，只更新找到的第一条记录。如果为true，把按条件查询出来的记录全部更新。
  ```js
  修改器    
	$inc    针对数字类型
		db.courses.update({},{$inc:{price:500}});  给所有数据的price字段加上500;
	$unset  删除所有的xx字段
		db.courses.update({},{$unset:{price:1}},true,true);
	$push   给collections中添加一个数组字段
		db.person.update({},{$push:{lessons:'js'}},true,true);
	$pushAll   给collections中批量添加元素
		db.person.update({},{$pushAll:{lessons:'js'}},true,true);
	$addToSet   给collections中添加元素   元素不重复
		db.person.update({},{$addToSet:{lessons:'js'}},true,true);
	$pop    删除数组字段中的最后一个元素
		db.person.update({},{$pop:{lessons:1}},true,true);
	$pull   删除数组字段中的指定的一个元素
		db.person.update({},{$pull:{lessons:'js'}},true,true);
	$pullAll   删除数组字段中的指定的元素
		db.person.update({},{$pullAll:{lessons:['js','node']}},true,true);
  ```
**9.删除数据**
	`db.person.remove({name:'楚寒'});`
**10.删除集合**
	`db.person.drop();`
**11.删除数据库**
	`db.dropDataBase();`
**12.查看数据库的名称**
	`db.getName();`
**13.查看数据库的状态**
	`db.stats();`
**14.查看帮助**
	`db.help();`
**15.改变id的显示方式**
	`db.person.find({},{_id:0});   //隐藏id列`
**16.runCommand findAndModify**
	```js
	var result =runCommand({
		findAndModify:'person',      //更新的集合名称
		query:{name:'张三'},	   //更新条件
		update:{$set:{age:33}},   //需要更新的值
		new:true      //是否返回更新后的值
	});
	```


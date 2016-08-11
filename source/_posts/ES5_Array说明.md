---
title: ES5 Array详细 
date: 2016年8月11日
categories: 
     - JS   
toc: true
tags: 
     - ES5
---

#### 1. JS 你不知道的Array
1.检测数组
``` js
if(value instanceof Array){
	//对数组执行操作
}
//上面这种写法是ECMAScript3时候的
if(Array.isArray(value)){
	//对数组执行操作
}
//这种是ES5中的写法
```
为什么要提这个呢?因为这个ES5的写法只支持IE9+ ff4+ Safari5+Chrome,所以在涉及要浏览器兼容的时候就要注意了。
<!-- more -->
2.数组的栈方法
``` js
var colors =[];
var count = colors.push("red","black");
alert(count);   //2,push方法有一个返回值,返回当前的数组的长度

count = colors.push("yellow");
alert(count);   //3

var item = colors.pop(); //pop方法删除数组的最后一项
alert(item);             //yellow ,说明之后返回值是删除的那一项
```
- 3.数组的队列方法
``` js
var  colors = ["red","black"];
var item =  colors.unshift("white");  //white,并且数组的长度加1

```
`数组的unshift方法是为数组添加第一项并返回该项` `数组的shift方法是为数组删除第一项并返回该项`
4.数组的操作方法
```js
//concat  合并数组
var colors = ["red","blue"];
var colors2 = colors .concat("green");  //注意:此时的colors还是原来的数组,没有改变
/*slice 分割数组并创建新的数组
  当传入1个参数的情况下:该方法返回的是从参数位置开始到数组末尾的所有项
  当传入2个参数的情况下:该方法返回的是从参数位置开始到第二个参数结束的的所有项,并不包括结束的位置项*/
var colors3 = colors2.slice(1);  //blue,green
var arr =  [1,2,3,4,5];
var arr2 = arr.slice(1,3);   
//[ 2, 3 ]
```
如果slice()方法的参数中有一个负数的情况下,就用数组的长度加上该数来确定相对的位置,如下
```js
var arr =  [1,2,3,4,5];
var arr_re = arr.slice(-2,arr.length);
//[ 4, 5 ] ,  实际上的参数是 arr.silice(5-2,5);
```
-------

数组的最强大的方法splice(),注意和上面的slice区别开;根据传入的参数的不同,它有很多种用法:
>删除:可以删除任意数量的项,需要传入2个参数,要删除的第一项的位置和要删除的项数
>如 splice(0,2)会删除数组中的前两项
>插入:可以向制定的位置插入任意数量的项,需要提供3个参数:起始位置,0(固定,表示是要删除的项,要插入的项),如果要传入多项,可以继续往后面传入更多的参数(项),
>如splice(1,0,"red","blue")   从第一个位置开始插入
>替换:可以向制定位置插入任意数量的项,且同事删除任意数量的项,只需要指定3个参数(起始位置,要删除的项,和要删除的项数)
>如splice(2,1,'red','green')   从第二个位置开始删除1项,然后在2的位置上插入"red"和"green"
`注意:splice()方法插入的时候是修改原数组,并不会返回新数组(它是没有返回值的)`
```js
var arr =  ["aaa",'bbb','ccc'];
var arr_re = arr.splice(2,0,"ddd","eee");

console.log(arr);  //[ 'aaa', 'bbb', 'ddd', 'eee', 'ccc' ]
console.log(arr_re);  // [];
```

5.数组的迭代方法
>ES5中数组有5个迭代方法
>every(): 如果函数对每一项都返回true,则返回true
>some(); 如果函数的任意一项返回true,则返回true
>filter(); 如果符合逻辑处理中的项返回true,那么这些项将会组成新的数组返回
>map(); 返回每次调用完成之后重新组成数组
>forEach(); 对数组中的每一项运行给定的函数,该方法没有返回值

```js
//every
var arr_numbers = [1,2,3,4,5,4,3,2,1];
var result = arr_numbers.every(function(item,index,arr){
	return item >2;
})
console.log(result)  //false 
//some
var arr_numbers = [1,2,3,4,5,4,3,2,1];
var result = arr_numbers.some(function(item,index,arr){
	return item >2;
})
console.log(result)  //true
//filter
var arr_numbers = [1,2,3,4,5,4,3,2,1];
var result = arr_numbers.filter(function(item,index,arr){
	return item >2;
})
console.log(result)  //[ 3, 4, 5, 4, 3 ]
//forEach
var arr_numbers = [1,2,3,4,5,4,3,2,1];
arr_numbers.forEach(function(item,index,arr){
    if(item ==2){
        console.log(item);
    }
})
console.log(arr_numbers );
/*
2
2
[ 1, 2, 3, 4, 5, 4, 3, 2, 1 ]
*/
//map
var arr_numbers = [1,2,3,4,5,4,3,2,1];
var result = arr_numbers.map(function(item,index,arr){
	return item *2;
})
console.log(result)  //[ 2, 4, 6, 8, 10, 8, 6, 4, 2 ]
```
`以上方法都是ES5的所以可能也会涉及到浏览器兼容问题哦~`

6.数组的缩小方法
ES5新增了2个缩小数组的方法:reduce()和reduceRight(),这两个方法都会迭代数组的所有项,然后构建一个最终的返回值,reduce()会从数组的第一项开始迭代,而reduceRight()会从最后一项开始迭代并且向前遍历.
```js
/*
	这两个方法都是接受4个参数(前一个值,当前值,当前项的索引,当前对象)
*/
var arr = [1,2,3,4,5,6];
var sum = arr.reduce(function(pre,cur,index,arr){
	return pre+cur;
}) 
console.log(sum);   //21
//reduceRight();也是如此所以就不用细说了,这次数组说明就到这里,以后可能会添加es6的数组的扩展哦~~欢迎阅读
```

---
title: JAVA集合总结
date: 2016年8月13日
categories: 
     - JAVA  
toc: true
tags: 
     - JAVA
---

学完了javase之后对自己所理解的集合做一个小结,如有出入欢迎吐槽 :)
下面是我自己画的一张图,简单的对集合做一个分类
![](http://obqi5r9i0.bkt.clouddn.com/java_01_%E9%9B%86%E5%90%88%E6%80%BB%E7%BB%93.png)

<!-- more -->

### 集合类接口的公共的成员方法: 
* boolean add(E e)     //添加元素
* boolean remove(Object o)    //删除元素
* void clear()             //删除整个集合的元素
* boolean contains(Object o)    //判断集合内是否包含该元素
* boolean isEmpty()            //判断集合是否为空
* int size()                  //定义集合的大小
* boolean retainAll(Collection c)     //判断是否与集合有交集如果有 则返回true 并且把2个集合中相同的元素赋值给原集合
   
``` python
public static void main(String[] args) {          
               //测试retainAll方法</span>
                Collection<String> c =new ArrayList<String>();
                c.add("1");
                c.add("2");
                c.add("3");
                c.add("4");
                Collection<String> c2 =new ArrayList<String>();
                c2.add("4");
                c2.add("5");
                c2.add("6");
                c2.add("7");
                boolean b = c.retainAll(c2);
                System.out.println(b);
                System.out.println(c);
                //程序的输出结果是
               
### Vector的特有功能

* public void addElement(E obj)    //将指定的类型对象添加到此集合的末尾，将其大小增加 1
* public E elementAt(int index)        //根据索引找元素
* public Enumeration elements()     //返回集合中的所有的元素

### LinkedList类特有功能

* public void addFirst(E e)及addLast(E e)    //将指定元素插入此列表的开头或最后。
* public E getFirst()及getLast()                    //返回此列表的第一个元素或最后元素。
* public E removeFirst()及public E removeLast()     //移除并返回此列表的第一个元素或最后一个元素

集合的遍历: Itrator
                          
public static void main(String[] args) {
     Collection<Integer> c =new ArrayList<Integer>();
                c.add(1);
                c.add(2);
                c.add(3);
                c.add(4);
                Iterator<Integer> it = c.iterator();
                while(true){
                        if(it.hasNext()){
                                System.out.println(it.next());
                        }
                }
        }
注意集合修改元素的时候有2种方式,1,迭代器遍历元素,迭代器修改元素,2 集合遍历元素,集合修改元素 否则修改元素则会报一个并发修改的异常.
              解决方案
                        a:迭代器遍历，迭代器修改(ListIterator)
                                元素添加在刚才迭代的位置
                        b:集合遍历，集合修改(size()和get())
                                元素添加在集合的末尾
public static void main(String[] args) {
                //迭代器遍历  迭代器修改
                ArrayList<Integer> arr =new ArrayList<Integer>();
                arr.add(1);
                arr.add(2);
                arr.add(3);
                arr.add(4);
                ListIterator<Integer> it = arr.listIterator();
                while(it.hasNext()){        
                        Integer i = it.next();
                                        if(i==2){
                                                it.add(100);
                                        }                                        
                }
                System.out.println(arr);
        }

public static void main(String[] args) {
                //集合遍历  集合修改
                ArrayList<Integer> arr =new ArrayList<Integer>();
                arr.add(1);
                arr.add(2);
                arr.add(3);
                arr.add(4);
                for(int i=0;i<arr.size();i++){
                        Integer x = arr.get(i);
                        if(x==2){
                                arr.add(100);
                        }
                }
                System.out.println(arr);
        }


           3,Map接口成员方法
                   V put(K key,V value)          //添加键值                                      V remove(Object key)           //删除
                   void clear()                                 //清除
                   boolean containsKey(Object key)      //根据键判断值是否存在
                   boolean containsValue(Object value)   //根据值判断键是否存在
                   Set<K> keySet()                            //获取map中的所有的元素
                   boolean isEmpty()                     //判断是否为空
                   int size()                                            //定义map的大小
                   Map集合遍历:
                              方式1：根据键找值
                                          获取所有键的集合
                                          遍历键的集合，获取到每一个键
                                          根据键找值
                             方式2：根据键值对对象找键和值
                                         获取所有键值对对象的集合
                                         遍历键值对对象的集合，获取到每一个键值对对象
                                         根据键值对对象找键和值
//方式1
        public static void main(String[] args) {
                Map<Integer, String>  map =new HashMap<Integer, String>();
                map.put(1, "hello");
                map.put(2, "world");
                map.put(3, "java");
                Set<Integer> set =map.keySet();
                for(Integer key :set){
                        String s = map.get(key);
                        System.out.println(key+"---"+s);
                }
        }       


<div class="blockcode"><blockquote>//方式二
        public static void main(String[] args) {
                Map<Integer, String>  map =new HashMap<Integer, String>();
                map.put(1, "hello");
                map.put(2, "world");
                map.put(3, "java");
                Set<Entry<Integer, String>> set = map.entrySet();
                for(Entry<Integer, String> en :set){
                        Integer i = en.getKey();
                        String s = en.getValue();
                        System.out.println(i+"---"+s);
                }
        }       
```

**关于集合的基础部分就写这些吧,正在整理集合的高级部分.**








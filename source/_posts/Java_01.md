---
title: 一道JAVA笔试题
date: 2016年8月13日
categories: 
     - JAVA  
toc: true
tags: 
     - JAVA
---

```python
package com.zsn.test1007;

import java.util.Scanner;

/*
 * 我们认为2是第一个素数，3是第二个素数，5是第三个素数，依次类推。
                         现在，给定两个整数n和m，0<n<=m<=200，
                         你的程序要计算第n个素数到第m个素数之间所有的素数的和，包括第n个素数和第m个素数。

        输入格式:
        两个整数，第一个表示n，第二个表示m。

        输出格式：
        一个整数，表示第n个素数到第m个素数之间所有的素数的和，包括第n个素数和第m个素数。

        输入样例：
        2 4 

        输出样例：
        15
 * 
 * 
 */
public class CalcZhiShu {
        public static void main(String[] args) {
                int sum=0;   //所求的素数的和
                int n,m;     //输入的2个数(n是第一个   m是第二个)
                int mark =1;   //记录第几个素数
                System.out.println("请输入第一个数:");
                Scanner sc =new Scanner(System.in);
                n =sc.nextInt();
                System.out.println("请输入第二个数:");
                Scanner sc1 =new Scanner(System.in);
                m =sc.nextInt();
                if(n==1) sum+=2;    
                boolean IsZhiShu =false;
                for(int i=2;;i++){
                IsZhiShu=true;
                                if(mark>m){
                                        break;
                                }
                                for (int j = 2;j<i; j++) {
                                        if(i%j==0){
                                                IsZhiShu=false;
                                                break;
                                        }
                                        
                                }
                                if(IsZhiShu==true){
                                        
                                        if(mark>=n){
                                                sum+=i;
                                                
                                        }
                                        mark++;
                            }
                        
                        }
                        System.out.println(n+"到"+m+"之间的素数和是:"+sum);

        }
}
```
下面是输出的结果:
![chuhan](http://obqi5r9i0.bkt.clouddn.com/java_01_01.jpg)
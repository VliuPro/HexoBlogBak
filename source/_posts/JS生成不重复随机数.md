title: JS通过打乱数组生成不重复随机数
date: 2015-12-01 20:47:48
updated:
tags: 
- JavaScript
categories: 
- JavaScript
permalink: js-random-no-repeat
---

> Js生成随机数的方法跟Java很像，或者说很多的编程语言的随机数生成方法都是类似的，只不过生成随机数的函数的实现思想是不是一致我就不知道了～


* 通过数组生成不重复随机数的一般思路是：创建一个数组，每次取出一个值之后置为null，但是这种方法的所需要产生的随机数的数目越大就越慢，一是因为越大所需要创建的数组空间越大，二是因为越到后面取到null的可能性越大，计算的重复次数越多。
比如： 
    ```javascript
        var list = new Array();
        var count = 100;
        for (var i = 0 ; i < count ; i ++){
            list[i] = i + 1;
        }
        for (var num,i = 0 ; i < count ; i ++){
            do{
                num = Math.floor(Math.random() * count);
            }while(list[num] == null);
            document.write(list[num] + ",");
            list[num] = null;
        }
    ```
    
---

* 现在换个思路，创建数组之后直接把数组打散，然后依次取出的就是不重复的随机数～
    ```javascript
        var list = new Array();
        var count = 100;
        for (var i = 0 ; i < count ; i ++){
            list[i] = i + 1 ;
        }
        list.sort(function(){
            return 0.5 - Math.random();
        });
        for (var i = 0 ; i < count ; i ++){
            document.write(list[i] + ",");
        }
    ```
    
---

两者的思路其实也差不多，只不过主要区别是取随机数的方法不一样！
##位置方法
>这两个方法都接收两个参数：要查找的项和表示起点位置的索引(可选)，`indexOf()`从数组开头开始向后查，相反的,`lastIndexOf()`从数组的末尾开始向前查找,返回查找的项在数组中的位置，没有找到返回-1。

**1.indexOf()**
```
var array = [1,2,3,2,1];
console.log(array.indexOf(2));//1
```
**2.lastIndexOf**
```
console.log(array.lastIndexOf(2));//3
```

##迭代方法
>ECMASCript5为数组定义了5个迭代方法。每个方法接收两个参数：要在每一项上运行的函数和运行该函数的作用域对象(可选)---影响this的值，传入这些方法中的函数接收三个参数：数组项的值，该项在数组中的位置，数组本身。这5种方法都会对数组的每一项运行给定函数但是不同的方法的返回值不同。

+ **every()**:若该函数对每一项都返回true，则返回true
```
var num = [1,2,3,2,1];
var result = num.every(function(item,index,array){
    return (item > 2);
    })
    console.log(result);//true
```
+ **filter()**:返回该函数会返回true的项组成的数组
```
var num = [1,2,3,2,1];
var result = num.filter(function(item,index,array){
    return (item > 2);
    })
    console.log(result);//[3]
```
+ **forEach()**:无返回值
```
var num = [1,2,3,2,1];
var result = num.forEach(function(item,index,array){
    return (item > 2);
    })
    console.log(result);//undefined
```
+ **map()**:返回每次调用结果组成的数组
```
var num = [1,2,3,2,1];
var result = num.map(function(item,index,array){
    return (item > 2);
    })
    console.log(result);// [false,false,true,false,false]
```
+ **some()**:如果该函数中有一项返回true，则返回ture
```
var num = [1,2,3,2,1];
var result = num.some(function(item,index,array){
    return (item > 2);
    })
    console.log(result);//true
```

`声明`:以上方法均不会修改数组本身
##缩小方法
>ECMAScript增加了两个缩小数组的方法,这两个方法都会迭代数组中的所有项，然后构建一个最终返回的值。这两个方法都接收两个参数,一个在每一项上调用的函数，一个是作为缩小基础的初始值(可选)。函数接收4个参数，前一个值，当前值，项的索引，数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项，因此第一个参数是是数组的第一项，第二个参数是数组的第二项，返回函数结果。

**1.reduce():**迭代从数组的第一项到最后一项
```
var val = [1,2,3]
var sum = val.reduce(function(pre,cur,index,array){
    return pre + cur;
    })
console.log(sum);//6
```
**2.reduceRight():**迭代从数组的最后一项到第一项
同上面结果一样。

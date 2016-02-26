Date类型
------------
>ECMASCript中的Date类型是在早期中Java中的java.util.Date类基础上构建的。为此Date类型使用自UTC(国际协调时间)1970年1月1日午夜(0时)开始经过的毫秒数来保存日期。

###创建日期对象
>1. 创建当前日期。不需要传入参数
 2. 创建指定日期。需要传入参数，必须传入表示该日期的毫秒数(即从1970年1月1日午夜起至该日期止经过的毫秒数)。为了简化这一计算过程,ECMAScript提供了两个方法:Date.parse()和Date.UTC()。

```
var now = new Date()//新创建的对象自动获得当前日期和时间
var someDate = new Date('May 25, 2004')
var allFives = new Date(2015, 4, 4, 17, 55, 55)
```

####Date.parse()和Date.UTC()

#####Date.parse()
>接收一个表示日期的`字符串`参数，然后尝试根据这个字符串返回相应日期的毫秒数

`var someDate = new Date(Date.parse('May 25,2015'))`
**Note:**ECMA-262没有定义Date.parse()应该支持那种日期格式，因此这个方法因实现而异，而且通常因地区而异。而实际上将表示日期的字符串传递给Date构造函数，也会在后台调用Date.parse()。

#####Date.UTC()
>参数分别是:年份，基于0的月份，日，小时，分钟，秒，以 及毫秒数。只有前两个参数是必须的。如果省略其他参数，则统统假设为0.

```
// GMT时间2016年1月1日午夜0时
var M = new Date(Date.UTC(2016, 0));

// GMT时间2015年5月5日下午5:55:55
var allFives = new Date(Date.UTC(2015, 4, 4, 17, 55, 55));
```

**Note:**Date构造函数也会模仿Date.UTC()，但有一点明显不同:日期和时间都基于本地时区而非GMT创建。不过Date构造函数仍与Date.UTCf()接收的参数相同。

####Date.now()
>返回调用这个方法时的日期和时间的毫秒数。

```
// 取得开始时间
var start = Date.now();

// 调用函数
doSomthing();

// 取得停止时间
var stop = Date.now();

result = stop - start;
```

**兼容性:**IE9+，Firfox3+,Safari3+,Opera10.5,Chrome。在不支持它的浏览器中，使用+操作符把Date对象转换成字符串，也可以达到同样目的

####日期格式化方法
>将日期格式化为字符串的方法

+ toDateString()
+ toTimeString()
+ toLocalDateString()
+ toLocalTimeString()
+ toUTCString()

**推荐:**`toUTCString()`
**Note:**UTC日期指的是没有时区偏差的情况下(将日期转换为GMT时间)的日期值。

事件处理程序
------------
 
###DOM0级事件处理程序
>通过Javascript指定事件处理程序的传统方式，就是将一个函数赋值给一个事件处理程序属性。
每个元素都有自己的事件处理程序属性，这些属性通常全部小写，例如onclick。将这种属性的值设置为一个函数，就可以指定事件处理程序。

```
var btn = document.getElementById('myBtn');
// 添加事件处理程序
btn.onclick = function () {
    alert( this );//为DOM元素btn
};
// 移除事件处理程序
btn.onclick = null;
```

**优点**:1.简单2.具有跨浏览器的优势
**缺点**:在代码运行之前不会指定事件处理程序，因此这些代码在页面中位于按钮后面，就有可能在一段时间怎么点击都没反应，用户体验变差。 

###DOM2级事件处理程序
>定义了两个方法，用于处理指定和删除事件处理程序的操作:addEventListener()和removeEventListener()。三个参数，1.要处理的事件名。2.作为事件处理程序的函数3.一个布尔值。最后这个布尔值为true,表示在捕获阶段调用事件处理程序，false表示在冒泡阶段调用事件处理程序。

```
// 添加多个事件处理程序
var btn = document.getElementById('myBtn');
btn.addEventListener('click',function (){
    alert( this );// 为DOM元素btn
},false );
btn.addEventListener('click',function () {
    alert('Hello World');
},false);

// 移除事件处理程序
btn.removeEventListener('click',function () {
    // 匿名函数无法被移除,移除失败
},false);
   // 改写
   var handler = function () {
    alert(this.id);
   };
   btn.addEventListener('click',handler,false);
   // 再次移除事件处理程序
   btn.removeEventListener('click',handler,false);// 移除成功
```
这两个事件处理程序会按照添加他们的顺序触发。大多数情况，都是将事件处理程序添加到事件流的冒泡阶段，这样可以最大限度的兼容各种版本的浏览器。

**优点**: 一个元素可以添加多个事件处理程序
**缺点**: IE8及以下浏览器不支持DOM2级事件处理程序。(包括IE8)

###IE事件处理程序
>定义了两个方法，与上类似:attachEvent(),detachEvent()。这两个方法接收相同的两个参数:事件处理程序名称和事件处理程序函数。由于IE8以及更早版本的浏览器只支持事件冒泡，所以通过detachEvent()添加的事件处理程序会被添加到冒泡阶段。

```
var btn = document.getElementById('myBtn');
btn.attachEvent('onclick', function(){
    alert( this );// window
});
btn.attachEvent('onclick', funciton(){
    alert("HELLO, WORLD");
});
```
点击按钮，这两个事件处理程序的触发顺序与上述刚好相反。不是按照添加事件处理程序的顺序触发，刚好相反。

**优点**:一个元素可以添加多个事件处理程序
**缺点**:只支持IE。

###跨浏览器的事件处理程序
```
var EventUtil = {
    addHandler : function ( ele, type, handler ) {
        if ( ele.addEventListener ) {
            ele.addEventListener( type, handler, false );
        } else if ( ele.attachEvent ) {
            ele.attachEvent( 'on' + type, handler );
        } else {
            ele['on' + type] = handler
        }
    },
    removeHandler: function ( ele, type, handler ) {
        if ( ele.removeEventListener ) {
            ele.removeEventListener( type, handler, false );
        } else if ( ele.detachEvent ) {
            ele.detachEvent( 'on' + type, handler );
        } else {
            ele[ 'on' + type ] = null;
        }
    }
}
```

事件对象
------------

###DOM中的事件对象
>兼容DOM的浏览器会将一个event对象传入到事件处理程序中。无论指定事件处理程序时使用什么方法(DOM0级或DOM2)级，都会传入event对象。event对象包含与创建它的特定事件有关的属性和方法。触发的事件类型不一样，可用的属性和方法也不一样。不过，所有事件都有下表列出的成员

|   属性/方法     |      类型       |   说明   
| ---------—---  | -------------  |  ------------
|  bubbles
| cancebles 
| currentTarget
| defaultPrevented
| detail 
| evnetPhase
| preventDefault
| stopImmediatePropagation()
| stopPropagation()
| target 
| trusted 
| type
| view

**1.currentTarget与target**

> target       :事件的目标对象
> currenTarget : 其当前事件处理程序正在处理事件的那个元素

```
var btn = document.getElementById('myBtn');

btn.onclick = function ( e ) {
    console.log( this === e.currentTarget );// true
    console.log( this === e.target );// true
};

document.body.onclick = function ( e ) {
    console.log( this === e.currentTarget );// true
    console.log( this === e.target );// false
};
```
点击按钮，结果如上。

**2.preventDefault()与cancelable**

>cancelable     :只读属性，表明是否可以取消事件的默认行为,值为true便可以，反之不行。
>preventDefault :阻止特定事件的默认行为。例如，链接的默认行为就是在被单击时会导航到其href特性指定的URL。

```
var link = document.getElementById( 'myLink' );
link.onclick = function ( e ) {
    // 阻止默认行为
    e.preventDefault();// 只有cancelable设置为true的事件，才可以使用
}
```

**3.stopPropagation()与eventPhase**

>stopPropagation() : 取消事件进一步捕获或冒泡，如果bubbles为true，则可以使用这个方法
>eventPhase        : 调用事件处理程序的阶段:1.表示捕获阶段2.表示'处于目标阶段'3.表示冒泡阶段。

```
var btn = document.getElementById('myBtn');

document.body.addEventListener( 'click', function ( e ) {
    alert( e.eventPhase );
}, true );// 1

btn.addEventListener( 'click',function ( e ) {
    alert( e.eventPhase );
}, false );// 2

document.body.addEventListener( 'click', function ( e ) {
    alert( e.eventPhase );
}, false );// 3
```
点击btn按钮弹出1，2，3。

```
var btn = document.getElementById('myBtn');

document.body.addEventListener( 'click', function ( e ) {
    alert( e.eventPhase );// 1
}, true );

btn.addEventListener( 'click',function ( e ) {
    // 阻止进一步的事件冒泡
    e.stopPropagation();
    alert( e.eventPhase );// 2
}, false );

document.body.addEventListener( 'click', function ( e ) {
    alert( e.eventPhase );// 点击按钮时，事件处理程序被阻止，无反应
}, false );
```
点击btn按钮弹出1，2。

```
var btn = document.getElementById('myBtn');

document.body.addEventListener( 'click', function ( e ) {
    // 阻止进一步的事件冒泡
    e.stopPropagation();
    alert( e.eventPhase );// 1
}, true );

btn.addEventListener( 'click',function ( e ) {
    alert( e.eventPhase );// 点击按钮时，事件处理程序被阻止，无反应
}, false ); 

document.body.addEventListener( 'click', function ( e ) {
    alert( e.eventPhase );// 点击按钮时，事件处理程序被阻止，无反应
}, false );
```
点击btn按钮弹出1。

###IE中的事件对象
>访问IE中的event对象有几种不同的方式，取决于指定事件处理程序的方法。IE的event对象同样也包含与创建它的事件相关的属性和方法。与DOM中的event对象一样，这些属性和方法也会因为事件类型的不同而不同,但所有事件对象都会包含下表所列的属性和方法。

|   属性/方法     |      类型       |   说明   
| ---------—---  | -------------  |  ------------
| cancelBubble
| returnValue 
| srcElement
| type

**1.srcElement**
>事件的目标(与DOM中的target属性相同)

```
var btn = document.getElementById('myBtn');

btn.onclick = function () {
    alert( window.event.srcElement === this )// true
};
btn.attachEvent('onclick', function (e) {
    alert( e.srcElement === this ) // false, IE事件处理程序绑定this的值为window
});
```

**2.returnValue**
> 默认值为true，但将其设置为false就可以取消事件的默认行为(与DOM中的preventDefault()作用相同)

```
var link = document.getElementById( 'myLink' );
link.onclick = function () {
    // 阻止默认行为
    window.event.returnValue = false;
}
```
与DOM不同的是，在此没有办法确定事件是否能被取消。

**3.cancelBubble**
>默认值为false，但将其设置为true就可以取消事件冒泡(与DOM中的stopPropagation()作用相同)

```
var btn = document.getElementById('myBtn');

btn.onclick = function () {
    alert( 'Clicked' );
    window.event.cancelBubble = true;
};

document.body.onclick = function () {
    alert( 'Body Clicked' )
};
```

内存和性能
------------
###事件处理程序对性能的影响
>在javascript中，添加到页面上的事件处理程序数量将直接关系到页面的整体运行性能。
1.每个函数(**事件处理程序**)都是对象，都会占用内存。内存中的对象越多，性能就越差。
2.必须事先指定所有 **事件处理程序**而导致的DOM访问次数，会延迟整个页面的交互就绪时间。
3.每当将 **事件处理程序**指定给元素时，运行中的浏览器代码与支持页面交互的Javascript代码之间就会建立一个连接。这种连接越多，页面执行起来越慢。

```
<div id="love-wrapper">
    <div id="love-l">L</div>
    <div id="love-o">O</div>
    <div id="love-v">V</div>
    <div id="love-e">E</div>
</div>

var l = document.getElementById( 'love-l' );
var o = document.getElementById( 'love-o' );
var v = document.getElementById( 'love-v' );
var e = document.getElementById( 'love-e' );

l.addEventListener('click', function () {
    alert(' L ')
}, false);
o.addEventListener('click', function () {
    alert( 'O' )
}, false);
v.addEventListener('click', function () {
    alert( 'V' )
}, false);
e.addEventListener('click', function () {
    alert( 'E' )
}, false);
```

在上面的示例中，为每个元素的点击事件都绑定了不同事件处理程序。

+ 取得了4个DOM元素。==>DOM访问次数为4次
+ 添加了4个事件处里程序。==>内存中的对象多增加了4个
+ 相应的事件处理程序与指定元素连接了4次==》连接数4

如果页面复杂，那么就会有数不清的代码用于添加事件处理程序。影响页面性能。

###事件委托
>对事件处理程序过多问题的解决方案就是事件委托。
事件委托利用了事件冒泡，只指定一个事件处理程序，就可以`管理` **某一类型(例如:点击事件)**的所有事件。
如果可行的话，可以考虑为document对象添加一个事件处理程序，用以处理页面上特定类型的事件。有点如下
1. document对象很快就可以访问，而且可以在页面生命周期中的任何时点上为它添加事件处理程序(无需等待load和DOMContentLoaded事件)。换句话说，只要可点击的元素呈现在页面上，就可以立即具备适当的功能
2. 在页面中设置事件处理程序所需的时间更少。只添加一个事件处理程序所需的DOM引用更少，所花的时间更少。 整个页面所占得内存空间更少

```
var love = document.getElementById( 'love-wrapper' );
love.addEventListener('click', function ( e ) {
    switch ( e.target.id ) {
        case 'love-l' : 
            alert('L');
            break;
        case 'love-o' :
            alert('O');
            break;
        case 'love-v' :
            alert('V');
            break;
        case 'love-e' :
            alert('E');
            break;
    }
},false)
```

在上面的示例中

+ 取得了1个DOM元素。==>DOM访问次数减少到了1次
+ 添加了1个事件处理程序。==>内存中的对象只增加了1个
+ 相应的事件处理程序与指定元素连接了4次==>连接数1

实现了和上一个例子中一样的效果。但是却有效的控制了DOM访问次数，事件处理程序的添加个数，以及DOM元素与相应的事件处理程序的连接次数。

###移除事件处理程序
>利用事件委托我们可以有效的控制相应的事件处理程序与指定元素的连接次数。另外，在不需要的时候移除事件处理程序，也是解决这个问题的一种方案。内存中留有那些过时不用的"空事件处理程序，也是造成Web应用程序内存和性能问题的主要原因"。

有两种情况可能导致上述问题
1. 带有事件处理程序的 **指定元素** 通过DOM操作被删除了，页面中的某一部分被替换了，导致带有事件处理程序的 **指定元素** 被删除了。
2. 卸载页面的时候。
 
**第一种情况:**本质上来讲都是一种情况，就是带有事件处理程序的 **指定元素**被删除了，但是其事件处理程序仍然和 **指定元素**保持着引用关系，导致其事件处理程序无法被当做垃圾回收。(尤其是IE)会做出这种处理

```
<div id="myDiv">
    <div id="myBtn"></div>
</div>
var handler = function () {
    // 删除指定元素
    document.getElementById( 'myDiv' ).removeChild();
}

var btn = document.getElementById('myBtn');

btn.addEventListener('click', handler, false);
```

点击按钮时，按钮被删除，但是其事件处理程序却还和其保持了引用关系,导致内存增加。因此在知道 **指定元素**可能被删除的情况下，先解除他们之间的引用关系。如下

```
<div id="myDiv">
    <div id="myBtn"></div>
</div>
var handler = function () {
    // 解除连接引用关系
    btn.removeEventListener( 'click', handler, false );

    // 删除指定元素
    document.getElementById( 'myDiv' ).removeChild();
}

var btn = document.getElementById('myBtn');

btn.addEventListener('click', handler, false);
```

这样通过解除引用连接关系，也可以提升页面性能。

**第二种情况:**IE8及更早浏览器在这种情况下仍然是问题最多的浏览器。如果在页面卸载之前，没有清理干净事件处理程序，那它们就会滞留在事件处理程序中。

一般来说，最好的做法是在页面卸载之前，先通过unload事件处理程序移除所有事件处理程序。





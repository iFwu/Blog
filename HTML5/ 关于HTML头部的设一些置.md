关于`DOCTYPE`
------------------
###作用
**表明文档类型**
>每个HTML版本都有一个对应的DTD（Document Type Definition），它解释了哪些标签、属性或值对于HTML的某个特定类型是有效的。是它告诉web浏览器你正在使用哪个版本的HTML，保证网页能在web浏览器上准确无误的显示出来。

1. 给浏览器看。让浏览器知道如何解释你的html。
2. 给人看。让人知道你的文档是用什么格式写的，符不符合规则，有没有错。
###种类
>DOCTYPE有3种DTD模式：strict tranditional Frameset
####名称格式：  `注册//组织//类型 标签//语言`,`注册`指组织是否由国际标准化组织(ISO)注册，`+`表示是，`-`表示不是
#####HTML 4.01 strict
`<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">`
#####HTML 4.01 Transitional
`<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">`
#####HTML 4.01 Frameset
`<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">`
#####最新 HTML5 推出更加简洁的书写，它向前向后兼容，推荐使用。
`<!doctype html>`

关于HEAD内的标签
---------------------
###兼容性相关

`<meta http-equiv="X-UA-Compatible" content="IE-edge,chrome=1">`
- 如果支持Google Chrome Frame：GCF，则使用GCF渲染；
- 如果系统安装ie8或以上版本，则使用最高版本ie渲染；
- 否则，这个设定可以忽略。

###显示相关
`<meta name="viewport" content="width=device-width,initial-scale=1.0">`
+ 设定宽度等于`device-width`,控制视口的宽度
+ 也可以设定成一个固定的值`content="width=500"`,记住,没有必要在meta标签里加px单位,这样写就会自动识别为px单位
+ `initial-scale`,控制页面最初加载时的缩放等级
+ `maximum-scale`,`minimum-scale`以及`user-scalable`属性控制允许用户以怎样的方式放大或者缩小页面
**注**:高度同理

`<meta charser="utf-8">`
为了正确显示HTML页面，浏览器必须知道页面使用的是那种语言。我们可以通过字符集来说明网页使用的是何种语言。
- utf-8:世界通用语言编码
- GB2312:简体中文编码

###搜索引擎相关
`<title></title>`
`<meta name="description" content="">`
`<meta name="keyword" content="">`
`<meta name="author" content="">`
`<html lang="en">`
 lang:对文章的主要语言进行声明。(对搜索引擎有用）
 
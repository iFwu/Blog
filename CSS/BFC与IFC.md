BFC与IFC
==========

###BFC与IFC的参与者
>Boxes in the normal flow belong to a formatting context, which may be block or inline, but not both simultaneously. Block-level boxes participate in a block formatting context. Inline-level boxes participate in an inline formatting context.
在`普通流`中的盒子会参与一种格式上下文,这个盒子可能是块盒也可能是行内盒,但不可能同时是块盒又是行内盒。块级盒参与块级格式上下文(`BFC`),行内级盒参与行级格式上下文(`IFC`)。 
 Block formatting contexts(BFC)

BFC
-------
###BFC的形成
>Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with 'overflow' other than 'visible' (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents.
浮动,绝对定位元素,和display属性为inline-boxs、table-cells、table-captions的不是块盒的块容器(除非这个值已经被传播到视口),以及当overflow不为visible的块盒才会为它的内容创建新的BFC。

为了看得更清晰,梳理如下

 - float 的值不为 none
 - position 的值不为 static 或 relative
 - display 的值为 table-cell、table-caption、inline-block、flex 或 inline-flex
 - overflow 的值不为 visibility

###BFC的特性

>In a block formatting context, boxes are laid out one after the other, vertically, beginning at the top of a containing block. The vertical distance between two sibling boxes is determined by the 'margin' properties. Vertical margins between adjacent block-level boxes in a block formatting context [collapse](https://www.w3.org/TR/2011/REC-CSS2-20110607/box.html#collapsing-margins).

1.在BFC中,盒子都是从它的包含块的顶部一个一个的垂直放置的。两个相邻盒子的垂直间距决定于margin属性。在BFC中,两个相邻块级盒子之间垂直方向上的外边距是会塌陷的。

>In a block formatting context, each box's left outer edge touches the left edge of the containing block (for right-to-left formatting, right edges touch). This is true even in the presence of floats (although a box's line boxes may shrink due to the floats), unless the box establishes a new block formatting context (in which case the box itself [may become narrower](https://www.w3.org/TR/2011/REC-CSS2-20110607/visuren.html#bfc-next-to-float) due to the floats).

2.在BFC中,每个盒子的左外边界挨着包含块的左边界(对于从右到左的格式化,则为右边界互相挨着)。即使是存在浮动元素也是如此(即使一个盒子的行盒是因为浮动而收缩了的),除非这个盒子建立了一个新的BFC(在某些情况下这个盒子自身会因为浮动而变窄)。

>For information about page breaks in paged media, please consult the section on [allowed page breaks](https://www.w3.org/TR/2011/REC-CSS2-20110607/page.html#allowed-page-breaks).

IFC
-------

>In an inline formatting context, boxes are laid out horizontally, one after the other, beginning at the top of a containing block. Horizontal margins, borders, and padding are respected between these boxes. The boxes may be aligned vertically in different ways: their bottoms or tops may be aligned, or the baselines of text within them may be aligned. The rectangular area that contains the boxes that form a line is called a line box.

在IFC中，盒子水平放置，一个接着一个，从包含块的顶部开始。水平margins,borders,和padding在这些盒子中被平分。这些盒子也许通过不同的方式进行对齐:他们的地步和顶部也许被对齐，或者通过文字的基线进行对齐。矩形区域包含着来自一行的盒子叫做line box。

>The width of a line box is determined by a containing block and the presence of floats. The height of a line box is determined by the rules given in the section on [line height calculations](https://www.w3.org/TR/2011/REC-CSS2-20110607/visudet.html#line-height).

line box的宽度和浮动情况由它的包含块决定。line box的高度由`line-height`的计算结果决定。

>A line box is always tall enough for all of the boxes it contains. However, it may be taller than the tallest box it contains (if, for example, boxes are aligned so that baselines line up). When the height of a box B is less than the height of the line box containing it, the vertical alignment of B within the line box is determined by the ['vertical-align' property](https://www.w3.org/TR/2011/REC-CSS2-20110607/visudet.html#propdef-vertical-align). When several inline-level boxes cannot fit horizontally within a single line box, they are distributed among two or more vertically-stacked line boxes. Thus, a paragraph is a vertical stack of line boxes. Line boxes are stacked with no vertical separation (except as specified elsewhere) and they never overlap.

一个line box总是足够高对于包含在它内的所有盒子。然后，它也许比包含在它内最高的盒子高。(比如，盒子对齐导致基线提高了)。当盒子B的高度比包含它的line box的高度低，在line box内的B的垂值对齐线通过`'vertical align'`属性决定。当几个行内级盒子在一个单独的line box内不能很好的水平放置，则他们被分配成了2个或者更多的垂直重叠的line boxs.因此,一个段落是很多个line boxs的垂直叠加。Line boxs被叠加没有垂直方向上的分离(特殊情况除外)，并且他们也不重叠。

>In general, the left edge of a line box touches the left edge of its containing block and the right edge touches the right edge of its containing block. However, floating boxes may come between the containing block edge and the line box edge. Thus, although line boxes in the same inline formatting context generally have the same width (that of the containing block), they may vary in width if available horizontal space is reduced due to floats. Line boxes in the same inline formatting context generally vary in height (e.g., one line might contain a tall image while the others contain only text).

通常，line box的左边缘挨着它的包含块的左边缘，右边缘挨着它的包含块的右边缘。然而，浮动盒子也许会在包含块边缘和line box边缘之间。因此，尽管line boxs在同样的行内格式上下文中通常都有相同的宽度(就是他的包含块的宽度)，但是水平方向上的空间因为浮动被减少了，它的宽度也会变得复杂。Line boxs在同样的行内格式上下文中通常在高度上是多样的(比如，一行也许包含了一个最高的图片然后其他的也可以仅仅只包含文字)

>When the total width of the inline-level boxes on a line is less than the width of the line box containing them, their horizontal distribution within the line box is determined by the ['text-align'](https://www.w3.org/TR/2011/REC-CSS2-20110607/text.html#propdef-text-align) property. If that property has the value 'justify', the user agent may stretch spaces and words in inline boxes (but not inline-table and inline-block boxes) as well.

当在一行中行内级盒子的总宽度比包含他们的line box的宽度小，他们的在line box中的水平放置位置由`'text align'`属性决定。如果属性是'justify'，用户代理可能会拉伸空间和文字在inline boxs内。

>When an inline box exceeds the width of a line box, it is split into several boxes and these boxes are distributed across several line boxes. If an inline box cannot be split (e.g., if the inline box contains a single character, or language specific word breaking rules disallow a break within the inline box, or if the inline box is affected by a white-space value of nowrap or pre), then the inline box overflows the line box.

当一个行内盒子超过了line box的宽度，则它被分割成几个盒子并且这些盒子被分配成几个横穿过的line boxs。如果一个行内盒子不能被分割。则行内盒子溢出line box。

>When an inline box is split, margins, borders, and padding have no visual effect where the split occurs (or at any split, when there are several).

当一个行内盒子被分割，分割发生则margins,borders,和padding便没有了视觉效果。

>Inline boxes may also be split into several boxes within the same line box due to [bidirectional text processing](https://www.w3.org/TR/2011/REC-CSS2-20110607/visuren.html#direction).

在同样的line box内的行内盒子也许会被分割成几个盒子因为双向的文字。

>Line boxes are created as needed to hold inline-level content within an inline formatting context. Line boxes that contain no text, no preserved white space, no inline elements with non-zero margins, padding, or borders, and no other in-flow content (such as images, inline blocks or inline tables), and do not end with a preserved newline must be treated as zero-height line boxes for the purposes of determining the positions of any elements inside of them, and must be treated as not existing for any other purpose.

Line boxs在行内格式上下文中档需要包含行内级内容时被创造。Line boxs包含没有文字，没有空格，没有带着margins,padding和borders，以及没有其他在流中的内容(比如图片，行内盒子和行内表格)，也不会以新起一行结尾。对于在他们内的任何盒子的位置都以他们决定并且必须将他们视作没有高度的line boxs。

###栗子

**栗1**
>Here is an example of inline box construction. The following paragraph (created by the HTML block-level element P) contains anonymous text interspersed with the elements EM and STRONG:
这里是一个inline box结构的栗子。下面的段落包含着散布在EM和STRONG元素的匿名文字。

```
<P>Several <EM>emphasized words</EM> appear
<STRONG>in this</STRONG> sentence, dear.</P>
```
The P element generates a block box that contains five inline boxes, three of which are anonymous:

+ Anonymous: "Several"
+ EM: "emphasized words"
+ Anonymous: "appear"
+ STRONG: "in this"
+ Anonymous: "sentence, dear."

P元素生成了一个块级盒子，它包含五个inline boxs，其中有3个是匿名的。如下。

+ 匿名 : 'Several'
+ EM : 'emphasized words'
+ 匿名 : 'appear'
+ STRONG : 'in this'
+ 匿名 : 'sentence, dear'

>To format the paragraph, the user agent flows the five boxes into line boxes. In this example, the box generated for the P element establishes the containing block for the line boxes. If the containing block is sufficiently wide, all the inline boxes will fit into a single line box:
为了格式化段落，用户代理将5个盒子都汇流入line boxs，在这个栗子中，由P元素生成的盒子为line boxs的包含块。如果包含块足够的宽，所有的inline boxs都将刚好放在一个单行的line box中，如下:

Several emphasized words appear in this sentence, dear.

>If not, the inline boxes will be split up and distributed across several line boxes. The previous paragraph might be split as follows:
如果没有，inline boxes将被分离并被分别分配到不同的line boxs中。上面的段落可能会被分割成下面这样:

Several emphasized words appear
in this sentence, dear.
或者，这样
Several emphasized  
words appear in this 
sentence, dear.

>In the previous example, the EM box was split into two EM boxes (call them "split1" and "split2"). Margins, borders, padding, or text decorations have no visible effect after split1 or before split2.
在前面的栗子中，EM box是被分割成了2个EM boxs(叫他们'split1'和'split2')。外边距，边界，内边距和文字修饰效果作用在'split1'之后或者'split2'之前都没有任何效果。

**栗2**
考虑下面的栗子。
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">
<HTML>
  <HEAD>
    <TITLE>Example of inline flow on several lines</TITLE>
    <STYLE type="text/css">
      EM {
        padding: 2px; 
        margin: 1em;
        border-width: medium;
        border-style: dashed;
        line-height: 2.4em;
      }
    </STYLE>
  </HEAD>
  <BODY>
    <P>Several <EM>emphasized words</EM> appear here.</P>
  </BODY>
</HTML>
```

Depending on the width of the P, the boxes may be distributed as follows:
依赖P 的宽度，盒子可能会被分割成下面这样。

![](../img/inline-layout.png)

>+ The margin is inserted before "emphasized" and after "words".
 + The padding is inserted before, above, and below "emphasized" and after, above, and below "words". A dashed border is rendered on three sides in each case.

+ 外边距作用在了'emphasized'之前和'words'单词之后
+ 内边距作用在了'emphasized'的前面，上面，和下面，和'words'单词的后面，上面和下面。单独的虚线框分别被渲染在了每个事例的三个边上。


>参考
[visual-formatting](https://www.w3.org/TR/2011/REC-CSS2-20110607/visuren.html#block-formatting)





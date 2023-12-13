### 工作中总结的css技巧，方便自己以后查询

#### 1.设置 position:relative; 加上zoom:1解决ie下的定位位置。

#### 2.IE67中定位溢出不动，只需要在有滚动条的容器上也设置相对定位即可。

#### 3.IE6下参照物宽高为奇数时候，绝对定位元素设置了位置为0或100%时，仍有1px的空隙，所以需设置偶数。

#### 4.IE6中浮动块级元素的margin双倍，需设置display:inline;_display:inline;解决

#### 5.IE6中浮动引起的3px的bug，采用负变距的方法

#### 6.让box-sizing继承html
	html {
	  box-sizing: border-box;
	}

	*, *:before, *:after {
	  box-sizing: inherit;
	}

#### 7.不建议使用‘_'这个符号，因为ie6有时候，不识别此符号

#### 8.设置font-size:100%的作用
	如当body设置font-size:12px 的时候,h1-h6是不继承这个属性的，所以要设置这个属性

#### 9.line-height:最好使用复数

#### 10.样式截字，需要出现省略号：但是这种截字的方法，必须是针对单行文本起作用
	width：***；text-overflow:ellipsis;overflow:hidden;white-space:nowrap;

#### 11.使用display:inline-block要注意的事项
display:block就是将元素显示为块级元素，display:inline则将元素显示为行内元素。
display：inline-block将元素显示为行内元素，但是元素的内容作为块元素处理。旁边的内联元素和该对象显示在同一行，并且允许空格，但是该元素具有块元素的特性，可以设置其高度，宽度等属性。在同一行内有不同高度内容的元素时，通常要设置对齐方式如vertical-align: top;来使元素顶部对齐。

#### 12.使用zoom:1的作用
触发IE浏览器的haslayout，解决ie下的浮动等一些问题，zoom属性是IE浏览器的专有属性，firefox等浏览器不支持，它可以设置或检索对象的缩放比例

#### 13.rgba透明在ie9下的问题（ie9的滤镜）
	filter:none;//处理ie9浏览器中的滤镜效果
	background-color:rgba(0,0,0,0.8);

#### 14.浏览器中判断ie6、ie7、ie8
	ie6 识别*和_
	ie7 识别*和!important
	firefox识别!important不能识别*
	color:#f00\0;       ie8-ie10 
	color:#f00\9\0;   ie9-ie10 
	color:#f00\9;    ie6-ie10 

	如果专门设置ie8的话，可以用下面的方法：
	color:#f00\0;
	color;#000\9\0;

#### 15.css清除浮动的三种方法
	1//个人推荐
	.clearfix:before,.clearfix:after{display:table;content:"",line-height:0;}
	.clearfix:after{clear:both;}

	2//（有时候会导致没有换行的内容隐藏到，或者拖动的时候，要拖动的内容在盒子里拖不出去）
	overflow:hidden;zoom:1;
	3//不推荐，要添加多余的标签
	添加div设置：clear:both;

### 16.ie8下图片带边框的问题
	给img设置以下属性 img{outline:none;border:none;}

### 17.图片自适应问题
	可以给图片外层添加一个盒子，设置宽高，然后设置图片为100%（兼容性好）
	还可以把图片当做背景来实现，用position-size,控制图片的自适应，但是兼容ie9+

### 18.对于"text-indent:2em;"属性，只能加在块状元素上面，内联元素是不起作用的。    

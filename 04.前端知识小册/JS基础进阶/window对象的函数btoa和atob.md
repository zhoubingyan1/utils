### 1、前言
btoa 和 atob 是window对象的两个函数，其中：

btoa ：是binary to ascii，用于将binary的数据用ascii码表示，即Base64的编码过程
atob：是ascii to binary，用于将ascii码解析成binary数据，即Base64的解码过程。


### 2、btoa
binary to ascii，用于将ascii字符串或二进制数据转换成一个base64编码过的字符串表示，即Base64的编码过程，常用于编码字符串。

```
var str = "hello world";

var demo = btoa(str);

console.log(demo);// aGVsbG8gd29ybGQ=

```

【注意】btoa不能编辑Unicode字符

```
var str = "好好学习";

var demo = btoa(str);

console.log(demo);

```

上面操作，此时会报错，翻译过来就是：无法在“Window”上执行“btoa”：要编码的字符串包含超出 Latin1 范围的字符。

### 3、atob
```
var demo = "aGVsbG8gd29ybGQ=";

var str = atob(demo);

console.log(str);//hello world

```
### 4、Unicode字符编码
刚才上面的例子中我们想对Unicode字符编码进行编码，直接使用btoa不行的，所以我们使用encodeURIComponent和decodeURIComponent方法。

因为 btoa 仅支持 ASCII 字符序列，如所以我们要先使用 encodeURIComponent ，将中文字符编码转变成ASCII字符序列


>>编码时，先用encodeURIComponent对字符串进行编码，再用btoa进行Base64编码；
>>解码时，先用atob对Base64编码的串进行解码，再用decodeURIComponent对字符串进行解码

注意：

编码的过程：Unicode字符 -------> 先encodeURI -------> 再btoa编码
解码的过程：先atob解码 -------> 再decodeURI -------> Unicode字符
```
var str = "好好学习";

var demo = btoa(encodeURIComponent(str));

console.log(demo);//JUU1JUE1JUJEJUU1JUE1JUJEJUU1JUFEJUE2JUU0JUI5JUEw


```
```
var demo = 'JUU1JUE1JUJEJUU1JUE1JUJEJUU1JUFEJUE2JUU0JUI5JUEw'

var str1 = decodeURIComponent(atob(demo))

console.log(str1)//好好学习

```

不过虽然到达了曲线救国的目的，但是由于 encodeURIComponent 和 decodeURIComponent 已经达到了转义控制字符的目的，使用 atob 和 btoa 感觉是多此一举。

5、IE低版本处理方式
在使用atob和btoa这两个函数时需要注意的是，IE9是不支持的（虽然现在基本都是面向Chrome浏览器编程😊）

我们使用第三方Base64工具：webtoolkit.base64是一个第三方实现的 Base64 编码工具，完美的支持 unicode 编码的字符串。
```
Base64.encode('中文')//"5Lit5paH"

Base64.decode('5Lit5paH');//"中文"

```
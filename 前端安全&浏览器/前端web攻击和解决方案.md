### 什么是web攻击
- Web攻击是指利用Web应用程序的漏洞或者设计缺陷，对Web服务器或者Web用户进行恶意的攻击行为。Web攻击可以导致网站数据泄露、网站功能破坏、网站服务中断、用户隐私泄露等严重的后果。

### 常见web攻击

- 1.SQL注入攻击：指利用Web应用程序对数据库的查询语句拼接不严格，将恶意的SQL语句注入到正常的查询语句中，从而执行非法的数据库操作，如读取、修改、删除数据等。
- 2.XSS（跨站脚本）攻击：指利用Web应用程序对用户输入的内容没有进行充分的过滤或者转义，导致恶意的脚本代码被嵌入到网页中，从而在用户浏览器上执行，实现窃取用户信息、篡改网页内容、劫持用户会话等目的。
- 3.CSRF（跨站请求伪造）攻击：指利用用户已经登录的Web应用程序的身份认证信息，构造一个伪造的请求，诱导或者强制用户在不知情的情况下访问该请求，从而实现非法的操作，如转账、修改密码、删除数据等。
- 4.文件上传攻击：指利用Web应用程序对用户上传的文件没有进行充分的检查或者限制，导致恶意的文件被上传到服务器上，从而实现执行恶意代码、覆盖正常文件、占用服务器资源等目的。
- 5.目录遍历攻击：指利用Web应用程序对用户输入的路径没有进行充分的过滤或者限制，导致恶意的路径被拼接到正常的路径中，从而实现访问服务器上不应该被访问的文件或者目录，如配置文件、源代码、日志文件等。

### 如何预防

- 对用户输入的内容进行严格的过滤和转义，避免将恶意的代码注入到网页或者数据库中。
    可以使用xss、he、validator等库来对用户输入的内容进行过滤和转义。

```js
const xss = require('xss');
const he = require('he');
const validator = require('validator');

// 对输入的字符串进行xss过滤和html entity编码
const filteredInput = xss(he.encode(input));

// 对输入的字符串进行特定类型的验证
if (validator.isEmail(input)) {
  // 处理输入合法的情况
} else {
  // 处理输入不合法的情况
}

```

- 对用户上传的文件进行严格的检查和限制，避免将恶意的文件上传到服务器上。

可以使用一些库来对用户上传的文件进行检查和限制，比如：file-type。

```js
const fileType = require('file-type');
const MAX_FILE_SIZE = 1024 * 1024 * 10; // 10MB

// 检查上传文件类型
if (fileType(buffer) === null) {
  // 处理上传的文件类型不合法的情况
}

// 检查上传文件大小
if (buffer.length > MAX_FILE_SIZE) {
  // 处理上传的文件大小超过限制的情况
}

```

- 对用户请求的路径进行严格的过滤和限制，避免将恶意的路径拼接到正常的路径中。

使用path-to-regexp、joi等库来对用户请求的路径进行过滤和限制

```js
const pathToRegexp = require('path-to-regexp');
const Joi = require('joi');

const path = '/users/:id';
const id = '123';

// 将路径模式转换成正则表达式
const regexp = pathToRegexp(path);

// 判断id是否符合要求
if (!Joi.string().regex(/^\d+$/).validate(id).error) {
  // 处理id合法的情况
}

// 检查请求路径是否符合要求
if (regexp.test(req.path)) {
  // 处理请求路径合法的情况
}

```

- 使用HTTPS协议来加密通信内容，避免被中间人截取或者篡改。


- 使用CSRF Token或者验证码等机制来防止CSRF攻击，避免用户在不知情的情况下执行非法的操作。实现CSRF Token或者验证码等机制的支持


- 使用HTTP头部中的一些安全选项来增强浏览器对XSS攻击和点击劫持等攻击的防护能力，Content-Security-Policy、X-XSS-Protection、X-Frame-Options等。

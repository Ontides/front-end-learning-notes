# 第1~2章 了解JavaScript

## 一、JavaScript的实现

### ECMAScript

ECMAScript，即 ECMA-262 定义的语言，并不局限于 Web 浏览器，ECMA-262 将这门语言作为一个基准来定义，以便在它之上再构建更稳健的脚本语言。Web 浏览器只是 ECMAScript 实现可能存在的一种宿主环境。

### DOM

文档对象模型（DOM， Document Object Model）是一个应用编程接口（API），用于在 HTML 中使
用扩展的 XML。 DOM 将整个页面抽象为一组分层节点。 HTML 或 XML 页面的每个组成部分都是一种
节点，包含不同的数据。

#### DOM Level

- DOM Level 1

  由DOM Core 和 DOM HTML组成，前者提供了一种映射 XML 文档，从而方便访问和操作文档任意部分的方式；后者扩展了前者，并增加了特定于 HTML 的对象和方法。 DOM Level 1 的目标是映射文档结构 。

- DOM Level 2

  DOM Level 2增加了对鼠标和用户界面事件、范围、遍历（迭代 DOM 节点的方法）的支持，而且通过对象接口支持了层叠样式表（CSS）。

  新增模块：

  - DOM 视图：描述追踪文档不同视图（如应用 CSS 样式前后的文档）的接口。
  - DOM 事件：描述事件及事件处理的接口。
  - DOM 样式：描述处理元素 CSS 样式的接口。
  - DOM 遍历和范围：描述遍历和操作 DOM 树的接口。

- DOM Level 3

  DOM Level 3 进一步扩展了 DOM，增加了以统一的方式加载和保存文档的方法（包含在一个叫DOM Load and Save 的新模块中），还有验证文档的方法（DOM Validation）。在 Level 3 中， DOM Core 经过扩展支持了所有 XML 1.0 的特性，包括 XML Infoset、 XPath 和 XML Base 。

- DOM4

  目前， W3C 不再按照 Level 来维护 DOM 了，而是作为 DOM Living Standard 来维护，其快照称为DOM4。 DOM4 新增的内容包括替代 Mutation Events 的 Mutation Observers。

### BOM

浏览器对象模型（BOM）API，用于支持访问和操作浏览器的窗口。使用 BOM，开发者可以操控浏览器显示页面之外的部分。而 BOM 真正独一无二的地方，当然也是问题最多的地方，就是它是唯一一个没有相关标准的 JavaScript 实现。 HTML5 改变了这个局面，这个版本的 HTML 以正式规范的形式涵盖了尽可能多的 BOM 特性。

## 二、HTML中的JavaScript

### `<script>`元素

将 JavaScript 插入 HTML 的主要方法是使用`<script>`元素。

`<script>`元素有下列 8 个属性：

- async

  可选。表示应该立即开始下载脚本，但不能阻止其他页面动作，比如下载资源或等待其他脚本加载。只对外部脚本文件有效。

- charset

  可选。使用 src 属性指定的代码字符集。这个属性很少使用，因为大多数浏览器不在乎它的值。

- crossorigin

  可选。配置相关请求的 CORS（跨源资源共享）设置。默认不使用 CORS。crossorigin="anonymous"配置文件请求不必设置凭据标志。 crossorigin="use-credentials"设置凭据标志，意味着出站请求会包含凭据。

- defer

  可选。 表示脚本可以延迟到文档完全被解析和显示之后再执行。 只对外部脚本文件有效。在 IE7 及更早的版本中，对行内脚本也可以指定这个属性。

- integrity

  可选。允许比对接收到的资源和指定的加密签名以验证子资源完整性（SRI，Subresource Integrity）。如果接收到的资源的签名与这个属性指定的签名不匹配，则页面会报错，脚本不会执行。这个属性可以用于确保内容分发网络（CDN， Content Delivery Network）不会提供恶意内容。

- language

  废弃。最初用于表示代码块中的脚本语言（如"JavaScript"、 "JavaScript 1.2"或"VBScript"）。大多数浏览器都会忽略这个属性，不应该再使用它。

- src

  可选。表示包含要执行的代码的外部文件

- type

  可选。代替 language，表示代码块中脚本语言的内容类型（也称 MIME 类型）。按照惯例，这个值始终都是"text/javascript"，尽管"text/javascript"和"text/ecmascript"都已经废弃了。 如果这个值是 module，则代码会被当成 ES6 模块，而且只有这时候代码中才能出现 import 和 export 关键字。

使用了 src 属性的`<script>`元素不应该再在`<script>`和`</script>`标签中再包含其他 JavaScript 代码。如果两者都提供的话，则浏览器只会下载并执行脚本文件，从而忽略行内代码。

`<script>`元素的一个最为强大、同时也备受争议的特性是，它可以包含来自外部域的 JavaScript 文件。跟`<img>`元素很像， `<script>`元素的 src 属性可以是一个完整的 URL，而且这个 URL 指向的资源可以跟包含它的 HTML 页面不在同一个域中。浏览器在解析这个资源时，会向 src 属性指定的路径发送一个 GET 请求，以取得相应资源，假定是一个 JavaScript 文件。这个初始的请求不受浏览器同源策略限制，但返回并被执行的 JavaScript 则受限制。当然，这个请求仍然受父页面 HTTP/HTTPS 协议的限制。

不管包含的是什么代码，在没有使用 defer 和 async 属性的情况下，浏览器都会按照`<script>`在页面中出现的顺序依次解释它们。

#### 标签位置

如果把所有 JavaScript 文件都放在<head>里，也就意味着必须把所有 JavaScript 代码都下载、解析和解释完成后，才能开始渲染页面（页面在浏览器解析到<body>的起始标签时开始渲染）。这会导致页面渲染存在延迟，白屏时间长。现在通常程序中的JavaScript引用都放在<body>元素后以减少加载白屏时间。

#### 推迟执行脚本

HTML 4.01 为<script>元素定义了一个叫 defer 的属性。这个属性表示脚本在执行的时候不会改变页面的结构。也就是说，脚本会被延迟到整个页面都解析完毕后再运行。因此，在<script>元素上设置 defer 属性，相当于告诉浏览器**立即下载，但延迟执行**。

```html
<script defer src="example1.js"></script>
<script defer src="example2.js"></script>
```

上面例子，第一个推迟的脚 本会在第二个推迟的脚本之前执行，而且两者都会在 DOMContentLoaded 事件之前执行。不过在实际当中，推迟执行的脚本不一定总会按顺序执行或者在 DOMContentLoaded 事件之前执行，因此最好只包含一个这样的脚本。

#### 异步执行脚本

HTML5 为<script>元素定义了 async 属性，与 defer 类似，它只适用于外部脚本，会告诉浏览器立即开始下载。不过，标记为 async 的脚本不保证能按照它们出现的次序执行。

async 属性告诉浏览器，不必等脚本下载和执行完后再加载页面，同样也不必等到该异步脚本下载和执行后再加载其他脚本。因此，异步脚本不应该在加载期间修改 DOM。

异步脚本保证会在页面的 load 事件前执行，但可能会在 DOMContentLoaded 之前或之后。

#### 动态加载脚本

除了静态在HTML中引入，通过向 DOM 中动态添加 script 元素同样可以加载指定的脚本。只要创建一个 script 元素并将其添加到 DOM 即可。

默认情况下， 以这种方式创建的<script>元素是以异步方式加载的，相当于添加了 async 属性。不过这样做可能会 有问题，因为所有浏览器都支持 createElement()方法，但不是所有浏览器都支持 async 属性。因此，如果要统一动态脚本的加载行为，可以明确将其设置为同步加载。

```javascript
let script = document.createElement('script');
script.src = 'gibberish.js';
script.async = false; 
document.head.appendChild(script);
```

以这种方式获取的资源对浏览器预加载器是不可见的。这会严重影响它们在资源获取队列中的优先级。根据应用程序的工作方式以及怎么使用，这种方式可能会严重影响性能。要想让预加载器知道这些动态请求文件的存在，可以在文档头部显式声明它们：

```html
<link rel="preload" href="gibberish.js">
```

### 行内代码和外部文件

通常认为最佳实践是尽可能将 JavaScript 代 码放在外部文件中。使用外部文件的优点：

* 可维护性

  JavaScript 代码如果分散到很多 HTML 页面，会导致维护困难。而用一个目录保存 所有 JavaScript 文件，则更容易维护，这样开发者就可以独立于使用它们的 HTML 页面来编辑代码。

* 缓存

  浏览器会根据特定的设置缓存所有外部链接的 JavaScript 文件，这意味着如果两个页面都用到同一个文件，则该文件只需下载一次。这最终意味着页面加载更快。

* 适应未来

  通过把 JavaScript 放到外部文件中，就不必考虑用 XHTML 或前面提到的注释黑科技。 包含外部 JavaScript 文件的语法在 HTML 和 XHTML 中是一样的。

### 文档模式

文档模式有三种：

* 标准模式（standards mode）

* 准标准模式（almost standards mode）

  支持很多标准的特性，但是没有标准规定得那么严格。主要区别在于如何对待图片元素周围的空白（在表格中使用图片时最明显）。

* 混杂模式（quirks mode）

  混杂模式在所有浏览器中都以省略文档开头的 doctype 声明作为开关。混杂模式在不同浏览器中的差异非常大，不使用黑科技基本上就没有浏览器一致性可言。

可以使用 doctype 切换文档模式

```html
<!-- HTML 4.01 Strict --> <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

<!-- HTML 4.01 Transitional --> <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<!-- HTML 4.01 Frameset --> <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
```

### `<noscript>`元素

`<noscript>` 元素用于给不支持 JavaScript 的浏览器或者被用户关闭JavaScript支持的浏览器提供替代内容。

`<noscript>`元素可以包含任何可以出现在`<body>`中的 HTML 元素，`<script>`除外。
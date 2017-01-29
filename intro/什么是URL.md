# 什么是URL？ #
### [原文链接](https://launchschool.com/books/http/read/what_is_a_url) ###
## 简介 ##
当你去拜访某个朋友的时候，你需要知道他家的地址。而当你想打电话给他的时候，你需要知道他的电话号码。显然没有地址或者电话号码，你就干不了这些事。而且，只要有了地址或者电话号码马上就可以判断这个人的身份，因为地址和电话号码都是唯一的。

这跟在互联网上查找和访问的概念服务器差不多。如果你想查一下facebook上的游戏页面，首先要打开浏览器并且访问 http://www.facebook.com/games 。然后Web浏览器会根据这个网址发出一个HTTP请求，并且接收服务器返回的资源。而你输入的这个网址 http://www.facebook.com/games 就是我们所说的统一资源定位符（Uniform Resource Locator），或URL。URL就像你联系朋友时用到的地址和电话号码。URL应该是统一资源标识符（Uniform Resource Identifier）这个笼统的概念最常见的一种形式。这个章节我们将探讨URL及其组成，了解作为一个Web开发人员应该关注的关于URL的一些知识。
## URL的组成 ##
每当你看到像 "http://www.example.com:88/home?item=book"这样的URL时，你应该知道这其实是由几个部分组成的。下面我们将这个URL拆分成5个部分：

- ``http:``：通常被称为URL模式。模式一般出现在冒号和两个斜杠（“://”）之前，其目的是通知Web客户端采取相对应的方式去获取资源。在这个例子种，Web客户端将使用超文本协议也就是HTTP来发送请求。还有一些其他的比较常用的URL模式，如``ftp``，``mailto``和``git``等。
- ``www.example.com``：通常叫做主机地址，是客户端定位和获取资源的地方。
- ``88``：端口或端口号。这只有在不使用默认端口的情况下才需要输入。
- ``/home/``：路径，表示主机上被请求的资源。这一部分信息是可选的。
- ``?item=book``：查询字符串，由查询参数组成。这些是要传递给服务器的数据，同样也是可选的。

<img src="https://d186loudes4jlv.cloudfront.net/http/images/url_components.png" width="100%" />
有些时候，路径也可以指向特定的资源。比如， http://www.example.com/home/index.html 指向了example.com服务器上的一个名为index.html的HTML文件。

有些时候我们需要在URL中加入主机服务器监听HTTP请求的端口。像 http://localhost:3000/profile 这种形式的URL就表示主机使用3000端口监听HTTP请求。HTTP服务的默认端口为80。虽然大多数的URL中并没有指定端口，但是需要知道的是每一个URL中都有端口的信息。当没有指定端口号的时候，HTTP请求默认会使用80端口。如果不想使用默认的参数，那就需要在URL中指定。
## 查询字符串/参数 ##
带有查询字符串的URL一般是下面这样的：

``http://www.example.com?search=ruby&results=10``

我们可以将查询字段拆分：

|    查询字段   |    说明                     |
|:------------:|:--------------------------:|
| ?            | 保留字符，标志着查询字段的开始 |
| search=ruby  | 这是一个查询参数的键/值对     |
| &            | 保留字符，为查询字符串添加参数 |
| results=10y  | 另一个查询参数的键/值对       |
接下来我们看另一个例子，假设我们有这么一个URL：

``http://www.phoneshop.com?product=iphone&size=32gb&color=white``

<img src="https://d186loudes4jlv.cloudfront.net/http/images/query_string_components.png" width="100%" />

在上面的例子中，查询键/值对 ``product=iphone`` ，``size=32gb`` 和 ``color=white`` 通过URL传递给服务器，告诉 www.phoneshop.com 服务器我需要一个白色，32gb大小的手机。而服务器如何处理使用这些参数则是由服务器的后台程序决定。

使用搜索引擎进行搜索的时候也可以经常看到查询字符串。**因为查询字符串是通过URL传递的，所以只能用户HTTP的GET请求。**我们会在本书的后面讨论不同的HTTP请求方式，现在我们需要知道的是，每一次你通过在浏览器的地址栏输入网址进行浏览的时候，都是发送了HTTP的GET请求。大部分的链接都是HTTP的GET请求，当然偶尔会有些例外。

<img src="https://d186loudes4jlv.cloudfront.net/http/images/query_strings.jpg" width="100%">

使用查询字符串来传递信息是个很不错的方法，但是查询字符串的使用还是有一些限制的：

- 查询字符串有长度限制。如果你有很多数据需要传给服务器，显然这种方法是不可行的。
- 查询字符串里的键/值对是显示在URL上的。所以不推荐使用查询字符串来传递用户名，密码这些敏感的信息。
- 空格和像“&”这样的字符在查询字符串中不能用。这些字符必须用URL编码代替，接下来我们会说到这些。
## URL编码 ##
URL在设计的时候默认只接收ASCII字符集的字符。因此，不在ASCII字符集里面的字符需要转义或使用编码转换为特定的格式。URL编码使用“%”加上2个16进制的数字来代表这些不合规范的字符的ASCII编码。

下面使一些常用的字符编码和一些URL例子：

| 字符  | ASCII编码 | URL                                                    |
| :----:|:---------:|:-------------------------------------------------------|
| 空格  | 20        |http://www.thedesignshop.com/shops/tommy%20hilfiger.html |
| !     | 21        |http://www.thedesignshop.com/moredesigns%21.html  |
| +     | 2B        |http://www.thedesignshop.com/shops/spencer%2B.html|
| #     | 23        |http://www.thedesignshop.com/%23somequotes%23.html|
符合下面条件的字符都需要进行编码处理：

1. 不在ASCII字符集之内。
2. 字符使用是不安全的。比如“%”的使用就是不安全的，因为“%”被用作其他字符的编码转换。
3. URL模式里面的保留字符。有些保留的字符有特殊的含义，它们在URL中的存在有特殊的用途。像“/”，“？”，“：”，“@”和“&”这些字符都是保留字符，使用这些字符的时候都需要进行编码转换。

举个例子，“&”是一个保留字符，用作查询字符的分隔符。“:”也是一个保留字符，用于分隔主机和端口，用户名和密码等。

所以究竟有哪些字符在URL中使用的时候是安全的呢？只有字母，数字和$-_.+!'(),"这些特殊字符在URL中使用是安全的。其他的保留字符，在用于其特定的用途时可以不进行编码转换，否则需要进行编码转换。
## 总结 ##
本章，我们讨论了什么是URL，也认识了URL的组成，并以URL的编码结束本章。在准备章节之后，我们将更深入地探讨HTTP请求和响应的背后。

---
title: Hexo博客加密
mathjax: false
tags: ['Hexo', 'NexT', '博客美化']
categories: ['博客美化']
copyright: false
password: hello
abstract: 本文章已经被加密，请输入密码查看。在本文章中，密码是"hello"。
message: 您好，请输入密码！密码是："hello"。
wrong_pass_message: 抱歉，这个密码看着不太对，请再试试。
wrong_hash_message: 抱歉，这个文章不能被校验，不过您还是能看看解密后的内容。
date: 2022-09-04 10:10:40
---


# Hexo博客加密

在线上看到过好多博客都要求输入密码才能看，虽然hexo博客都是静态页面，但觉得好玩，所以试一试。

[戳这里，点击原文连接](https://zhuanlan.zhihu.com/p/113235573)

<!-- More -->

## 用到的工具

```
$ hexo version
INFO  Validating config
hexo: 6.2.0
hexo-cli: 4.3.0
os: win32 10.0.18363
node: 16.14.2
v8: 9.4.146.24-node.20
uv: 1.43.0
zlib: 1.2.11
brotli: 1.0.9
ares: 1.18.1
modules: 93
nghttp2: 1.45.1
napi: 8
llhttp: 6.0.4
openssl: 1.1.1n+quic
cldr: 40.0
icu: 70.1
tz: 2021a3
unicode: 14.0
ngtcp2: 0.1.0-DEV
nghttp3: 0.1.0-DEV
```

这是我的Hexo版本。我们还需要一个扩展：`hexo-blog-encrypt`

## 特性

一旦你输入了正确的密码，它就会被存储在本地浏览器的localStorage中。按个按钮，密码将会被清空。若博客中有脚本，它将被正确地执行。

支持按标签加密。

所有的核心功能都是由原生的API所提供的。在Node.js中，我们使用Crypto。在浏览器中，我们使用Web Crypto API。

PBKDF2，SHA256被用作复制密钥，AES256-CBC被用作加解密，我们还使用HMAC来验证密文的来源，并确保其纠正。

广泛地使用Promise来进行异步操作，从而确保线程不被杜塞。

过时的浏览器将无法正常显示，因此，请升级您的浏览器。

## 安装

```
npm install --save hexo-blog-encrypt
```

## 快速使用

对于一般用户而言，使用以下内容即可。

### 将“ password”添加到您的文章信息头

例如：
```
---
title: Hexo博客加密
mathjax: false
tags: ['Hexo']
categories: ['Hexo'， '博客']
copyright: true
password: hello
date: 2022-09-04 10:10:40
---
```

### 按标签加密

实际上作者这样搞我看不太懂，理论上按照标签加密，按照字面意思，就是根据这个tag是不是符合加密的规则，如果是就进行加密。

所以只需要配置_config.yml就行了，为什么还需要改文件头呢？

#### 修改文件头：
```
---
title: Hello World
tags:
- 加密文章tag
date: 2020-03-13 21:12:21
password: muyiio
abstract: 这里有东西被加密了，需要输入密码查看哦。
message: 您好，这里需要密码。
wrong_pass_message: 抱歉，这个密码看着不太对，请再试试。
wrong_hash_message: 抱歉，这个文章不能被纠正，不过您还是能看看解密后的内容。
---
```

#### 对博客根目录_config.yml进行添加：
```
# 安全
encrypt: # hexo-blog-encrypt
  abstract: 这里有东西被加密了，需要输入密码查看哦。
  message: 您好， 这里需要密码.
  tags:
  - {name: tagName， password: 密码A}
  - {name: tagName， password: 密码B}
  template: <div id="hexo-blog-encrypt" data-wpm="{{hbeWrongPassMessage}}" data-whm="{{hbeWrongHashMessage}}"><div class="hbe-input-container"><input type="password" id="hbePass" placeholder="{{hbeMessage}}" /><label>{{hbeMessage}}</label><div class="bottom-line"></div></div><script id="hbeData" type="hbeData" data-hmacdigest="{{hbeHmacDigest}}">{{hbeEncryptedData}}</script></div>
  wrong_pass_message: 抱歉， 这个密码看着不太对， 请再试试.
  wrong_hash_message: 抱歉， 这个文章不能被校验， 不过您还是能看看解密后的内容.
```

### 对 TOC 文章进行加密

首先，什么是TOC呢？？

是博客页面中显示文章结构的东西。使用博客加密后，TOC就没了。比如本篇就是没有的。

虽然原文中写的是matery主题，跟我NexT没什么关系，但还是介绍一下加密的方法。

如果您有一篇文章使用了TOC，您需要修改模板的部分代码。这里以matery主题作为示例：

- 在hexo/themes/matery/layout/_partial/article.ejs找到article.ejs。
- 
- 然后找到`<％post.content％>`这段代码，通常在30行左右。
- 
- 使用如下的代码来替代它：

```
<% if(post.toc == true){ %>
  <div id="toc-div" class="toc-article" <% if (post.encrypt == true) { %>style="display:none" <% } %>>
    <strong class="toc-title">Index</strong>
      <% if (post.encrypt == true) { %>
        <%- toc(post.origin， {list_number: true}) %>
      <% } else { %>
        <%- toc(post.content， {list_number: true}) %>
      <% } %>
  </div>
<% } %>
<%- post.content %>
```

在NexT中并没有找到什么合适的方法。还需加油，解决这个问题！

## 尾声

### tag加密探秘

看到结尾才发现这个文章也是转载的......

我跳到原文一看，才知道了对tag加密是怎么一回事。

简而言之就是优先级：文章信息头 > _config.yml (站点根目录下的) > 默认配置

按照两种方法应该都是可以加密成功的。**（还没有尝试，下次尝试一下来填坑）**

逛原文还看到了以下这几个：

### 对博客取消tag加密

> 只需要将博文头部的`password`设置为`""`即可取消 Tag 加密。

利用的特性就是上文说的优先级。

### 关于 Callback 函数

在部分博客中，解密后部分元素可能无法正常显示或者表现，这属于已知问题。
目前的解决办法是通过自行查阅自己的博客中的代码，了解到在 onload 事件发生时调用了哪些函数，并将这些函数挑选后写入到博客内容中。如：
```
---
title: Callback Test
date: 2019-12-21 11:54:07
tags:
    - Encrypted
---

This is a blog to test Callback functions. You just need to add code at the last of your post like following:

It will be called after the blog decrypted.

<script>
    // 添加一个 script tag 与代码一起放在文章末尾.
    alert("Hello World");
</script>
```


### 禁用 Log

如果你想要禁止使用 Log， 你可以在 _config.yml 中增加一个 silent 属性， 并将其设置为 true。

```
# Security
encrypt: # hexo-blog-encrypt
  silent: true
```

这样就会禁止如`INFO hexo-blog-encrypt: encrypting "{Blog Name}" based on Tag: "EncryptedTag".`的日志.

### 加密时使用的主题

之前，我们尝试使用`template`关键字来让用户能修改自己的主题。后来发现真不是一个好主意。所以我们现在引入了主题：theme关键字.

你可以简单的在`_config.yml`里或者文章头里使用`theme`，如下：

#### 文章信息头

```
---
title: Hello World
tags:
- 作为日记加密
date: 2016-03-30 21:12:21
password: mikemessi
abstract: 有东西被加密了, 请输入密码查看.
message: 您好, 这里需要密码.
theme: xray
wrong_pass_message: 抱歉, 这个密码看着不太对, 请再试试.
wrong_hash_message: 抱歉, 这个文章不能被校验, 不过您还是能看看解密后的内容.
---
```

#### 在 _config.yml

```
# Security
encrypt: # hexo-blog-encrypt
  abstract: 有东西被加密了, 请输入密码查看.
  message: 您好, 这里需要密码.
  tags:
  - {name: tagName, password: 密码A}
  - {name: tagName, password: 密码B}
  theme: xray
  wrong_pass_message: 抱歉, 这个密码看着不太对, 请再试试.
  wrong_hash_message: 抱歉, 这个文章不能被校验, 不过您还是能看看解密后的内容.
```

你可以在线挑选你喜欢的主题，并应用到你的博客中：
[default](https://mhexo.github.io/2020/12/23/Theme-Test-Default/)
[blink](https://mhexo.github.io/2020/12/23/Theme-Test-Blink/)
[shrink](https://mhexo.github.io/2020/12/23/Theme-Test-Shrink/)
[flip](https://mhexo.github.io/2020/12/23/Theme-Test-Flip/)
[up](https://mhexo.github.io/2020/12/23/Theme-Test-Up/)
[surge](https://mhexo.github.io/2020/12/23/Theme-Test-Surge/)
[wave](https://mhexo.github.io/2020/12/23/Theme-Test-Wave/)
[xray](https://mhexo.github.io/2020/12/23/Theme-Test-Xray/)

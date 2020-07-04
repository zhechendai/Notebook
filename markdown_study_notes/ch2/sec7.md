超链接
====

行内式
----

格式为`[link text](URL 'title text')`

①普通链接

```markdown
[Google](http://www.google.com/)
```

[Google](http://www.google.com/)

②指向本地文件的链接

```markdown
[icon.png](./images/icon.png)
```

[icon.png](./images/icon.png)

③包含'title'的链接

```markdown
[Google](http://www.google.com/ "Google")
```

[Google](http://www.google.com/ "Google")

>title使用'或"都是可以的。

参考式
----

参考式链接的写法相当于行内式拆分成两部分，并通过一个*识别符*来连接两部分。参考式能够尽量保持文章的结构简单，也方便统一管理URL。

①定义链接

```markdown
[Google](link)
```
[Google](link)

②定义链接内容

```markdown
[link]:http://www.google.com/ "Google"
```

其格式为：`[识别符]：URL 'title'`.

>其中，URL可以使用<>包括起来，title可以使用""、''、()包括（考虑到兼容性，建议使用引号），title部分也可以换行来写；  
>链接的内容定义可以放在同一个文件的*任意位置*。

③也可以省略*识别符*，使用链接文本作为*识别符*：

```markdown
[Google][]
[Google]:http://www.google.com/ "Google"
```

>参考式相对于行内式有一个明显的优点，就是可以在多个不同的位置引用同一个URL。

自动链接
----

使用`<>`包括的URL或邮箱地址会被自动转换为超链接：

```markdown
<http://www.google.com/>
<123@email.com>
```

<http://www.google.com/>

<123@email.com>

该方式适合行内较短的链接，会使用URL作为链接文本。邮箱地址会自动编码，以避免抓取机器人。
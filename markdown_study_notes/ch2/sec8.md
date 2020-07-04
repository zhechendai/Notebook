图像
====

插入图片的语法和插入超链接的语法基本一致，只是在最前面多一个`!`。也分为行内式和参考式两种。

行内式
----

```markdown
![Post](http://img.xiepp.com/Image/201604/p764799071.jpg "post")
```

![Post](http://img.xiepp.com/Image/201604/p764799071.jpg "post")

参考式
----

```markdown
![Post][post]
[post]:http://img.xiepp.com/Image/201604/p764799071.jpg
```

![Post][post]
[post]: http://img.xiepp.com/Image/201604/p764799071.jpg "Post"

指定图片的大小
----

Markdown不支持指定图片的显示大小，不过可以通过直接插入`<img />`标签来指定相关属性：

```markdown
<img src="http://img.xiepp.com/Image/201604/p764799071.jpg" width="50" height="50" />
```

<img src="http://img.xiepp.com/Image/201604/p764799071.jpg" width="100" height="100" />



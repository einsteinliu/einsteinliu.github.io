---
layout: post
title: 改造Jekyll模板的技术细节
description: "适用于Windows"
tags: [网页搭建, 网络编程]
categories: [Tech]
---
#### 框架的文件夹结构

_includes ：存放了一些定制的网页元素，比如header.html是整个页面的头，也就是最上面的菜单栏。又如author.html，是作者页面，用于展示作者信息。通用的JS文件都放在scripts.html里。

_layout ：主要定义了两种类型页面的排版，post是为单篇文章设计的排版，post-index是为一系列文章设计的排版。

_posts：用于存放所有文章的md文件，md文件的命名必须严格按照"年-月-日-标题"的格式命名。

_sass：用于存放定制的css文件，比如\_page就规定了页面各个元素的宽度颜色字体，\_variables定义了一些全局变量的值。

_site：模板编译完成后生成的页面，这个是真正可以直接部署的页面，平时不用看

_templates：规定了不同类型的排版文件中可以定义的变量

**前面不带下划线的文件夹存放用户自己定制的页面，比较重要的有：**

images：用于存放图片

search：用于存放搜索框页面

tags：用于存放按照tags列出所有文章的页面

categories：用于存放按照category列出所有文章的页面

posts：用于存放列出所有文章的页面

<!-- more -->

**对于我本人的页面**

aethetics文件夹里面是浏览所有categories为Aethetics的文章的页面，cpp是浏览categories为cpp的文章的页面等等，都是手动复制的，非动态

**重要的文件**

\_config.yml：非常重要，存放用于定制框架的全局变量

index.html：非常重要，用于存放本站的首页，也就是latest posts页

search.json：非常重要，search功能必需文件



#### 框架变量的形式:{% raw %}{{}}{% endraw %}

框架的变量一般用双大括号表示，例如在根目录的index.html中：
{% raw %}
**{{site.url}}**  
{% endraw %}
就是页面url的字符串，对于index.html页而言，经过编译的结果就是**"einsteinliu.github.io/index.html"**这个字符串




又比如：

{% raw %}
**{{post.image.feature}}**
{% endraw %}
 post是指排版类型为post的一个页面，而post的排版类型在**\_template**文件夹的post文件中规定了如下可以定义的变量：
{% raw %}
layout: {{ layout }}

title: {{ title }}

modified:

categories: {{ dir }}

description:

tags: []

image:

  feature:
  
  credit:
  
  creditlink:
  
comments:
{% endraw %}
所以image就是post的子变量，而image这个子变量又有一个子变量叫做feature，这个变量用于存放这篇文章的题图，比如我们使用image文件夹中的Caspar_David_Friedrich_Der_Monch_am_Meer_Google_Art_Project.jpg作为题图，我们就要在相应post的MD文件中加上：

**image:**

  **feature: Caspar_David_Friedrich_Der_Monch_am_Meer_Google_Art_Project.jpg**
  
  **credit: Der Mönch am Meer**
  
  **creditlink: https://en.wikipedia.org/wiki/The_Monk_by_the_Sea**

于是{% raw %}**{{post.image.feature}}**{% endraw %}就会被编译为字符串“Caspar_David_Friedrich_Der_Monch_am_Meer_Google_Art_Project.jpg”

同理，{% raw %}**{{post.image.credit}}**{% endraw %}会被编译为字符串“Der Mönch am Meer”



#### 框架控制流的形式：{% raw %}{% %}{% endraw %}

框架生成网站的逻辑控制流均使用以上形式，例如在根目录的index.html文件中：
{% raw %}
**{% for post in paginator.posts %}**

**{% endfor %}**
{% endraw %}
这一对循环语句中间，post 就是属于 paginator 的文章，也就是近期发布的文章列表中的文章。这一对控制语句就是将近期发布列表 paginator 中的文章loop一遍。




又比如：

{% raw %}
**{% if post.image.feature %}**

**{% endif %}**
{% endraw %}
显然构成一个判断语句，意思是如果当前的 post 包含了题图，就加入题图元素：
{% highlight html %}
{% raw %}
    <div class="entry-image-index">
       <a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}"><img src="{{ site.url }}/images/{{ post.image.feature }}" alt="{{ post.title }}"></a>
    </div>
{% endraw %}
{% endhighlight %}
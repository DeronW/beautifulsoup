.. BeautifulSoup文档 documentation master file, created by
   sphinx-quickstart on Fri Nov 29 13:49:30 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Beautiful Soup 4.2.0 文档
==========================

.. image:: http://www.crummy.com/software/BeautifulSoup/bs4/doc/_images/6.1.jpg
    :align: right

`BeautifulSoup <http://www.crummy.com/software/BeautifulSoup/>`_ 是一个可以从HTML或XML文件中提取数据的python库.它能够通过你喜欢的转换器实现惯用的文档导航,查找,修改文档的方式.使用BeautifulSoup会帮助你节省数小时甚至数天的工作.

这篇文档介绍了BeautifulSoup4中所有主要特性,并切有小例子.让我来向你展示它适合做什么,如何工作,怎样使用,如何达到你想要的效果,和处理异常情况.

文档中出现的例子在python2.7和python3.2中的执行结果相同

你可能在寻找 `BeautifulSoup3 <http://www.crummy.com/software/BeautifulSoup/bs3/documentation.html>`_ 的文档,Beautiful Soup 3 目前已经停止开发,我们推荐在现在的项目中使用Beautiful Soup 4, `移植到BS4`

获取帮助
--------

如果你有关于BeautifulSoup的问题,可以发送邮件到 `讨论组 <https://groups.google.com/forum/?fromgroups#!forum/beautifulsoup>`_ .如果你的问题包含了一段需要转换的HTML代码,确保你提的问题中附带HTML文档的在 **诊断代码** [1]_

快速开始
==========

下面的一段HTML代码将作为我们整篇文档的解析用的例子.这是 *爱丽丝梦游仙境的* 的一段内容

::

    html_doc = """
    <html><head><title>The Dormouse's story</title></head>
    <body>
    <p class="title"><b>The Dormouse's story</b></p>

    <p class="story">Once upon a time there were three little sisters; and their names were
    <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
    <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
    <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
    and they lived at the bottom of a well.</p>

    <p class="story">...</p>
    """

使用BeautifulSoup解析这段代码,能够得到一个BeautifulSoup的对象,并能按照格式化的结构输出:

::

    from bs4 import BeautifulSoup
    soup = BeautifulSoup(html_doc)

    print(soup.prettify())
    # <html>
    #  <head>
    #   <title>
    #    The Dormouse's story
    #   </title>
    #  </head>
    #  <body>
    #   <p class="title">
    #    <b>
    #     The Dormouse's story
    #    </b>
    #   </p>
    #   <p class="story">
    #    Once upon a time there were three little sisters; and their names were
    #    <a class="sister" href="http://example.com/elsie" id="link1">
    #     Elsie
    #    </a>
    #    ,
    #    <a class="sister" href="http://example.com/lacie" id="link2">
    #     Lacie
    #    </a>
    #    and
    #    <a class="sister" href="http://example.com/tillie" id="link2">
    #     Tillie
    #    </a>
    #    ; and they lived at the bottom of a well.
    #   </p>
    #   <p class="story">
    #    ...
    #   </p>
    #  </body>
    # </html>

几个简单的浏览HTML节点的方法:

::

    soup.title
    # <title>The Dormouse's story</title>

    soup.title.name
    # u'title'

    soup.title.string
    # u'The Dormouse's story'

    soup.title.parent.name
    # u'head'

    soup.p
    # <p class="title"><b>The Dormouse's story</b></p>

    soup.p['class']
    # u'title'

    soup.a
    # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

    soup.find_all('a')
    # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
    #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
    #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

    soup.find(id="link3")
    # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>

从文档中找到所有<a>标签的链接:

::

    for link in soup.find_all('a'):
        print(link.get('href'))
        # http://example.com/elsie
        # http://example.com/lacie
        # http://example.com/tillie

从文档中获取所有文字内容

::

    print(soup.get_text())
    # The Dormouse's story
    #
    # The Dormouse's story
    #
    # Once upon a time there were three little sisters; and their names were
    # Elsie,
    # Lacie and
    # Tillie;
    # and they lived at the bottom of a well.
    #
    # ...

这是你需要的吗?别着急,还有更好用的

安装 Beautiful Soup
======================

如果你用的是新版的Debain或ubuntu,那么可以通过系统包管理来安装

``$ apt-get install python-bs4``

Beautiful Soup 4 通过PyPi发布,所以如果你无法使用系统包管理安装,那么也可以通过easy_install活pip来安装.包的名字是 *beautifulsoup4* ,这个包兼容python2和python3

``$ easy_install beautifulsoup4``

``$ pip install beautifulsoup4``

(在PyPi中还有一个名字是 *beautifulsoup的* 包,但那可能不是你想要的,那是 `BeautifulSoup3 <http://www.crummy.com/software/BeautifulSoup/bs3/documentation.html>`_ 的发布版本,因为很多项目还在使用BS3, 所以*beautifulsoup* 包依然有效,但是如果你想执行新版本的代码,那么你需要安装的包是 *beautifulsoup4*)

如果你没有安装 *easy_install* 或 *pip* ,那你也可以 `下载BS4的源码 <http://www.crummy.com/software/BeautifulSoup/download/4.x/>`_ ,然后通过setup.py来安装.

``$ python setup.py install``

如果上述安装方法都行不通,Beautiful Soup的发布协议允许你将BS4的代码打包在你的项目中,这样无须安装即可使用.

作者在Python2.7和Python3.2的版本下开发Beautiful Soup, Beautiful Soup应该在所有当前的python版本中正常工作

安装完成后的问题
-----------------

Beautiful Soup被打包成Python2版本的编码,当在Python3环境下安装时,会自动转换成python3的代码,如果你没有安装的过程,那么代码就不会被转换.

如果代码抛出了 *ImportError* 的异常: "No module named HTMLParser", 这是因为你在python3版本中执行python2版本的代码.


如果代码抛出了 *ImportError* 的异常: "No module named html.parser", 这是因为你在python2版本中执行python3版本的代码.

如果遇到上述2种情况,最好的解决方法是重新安装BeautifulSoup4.

如果遇到 *SyntaxError* "Invalid syntax" 在`ROOT_TAG_NAME = u'[document]'` ,需要将把BS4的python代码版本从python2转换到python3. 可以重新安装BS4:

$ python3 setup.py install

或在bs4的目录中执行python代码版本转换脚本

$ 2to3-3.2 -w bs4

安装解析器
------------

Beautiful Soup支持python标准库中的HTML解析器,还支持一些第三方的解析器,其中一个是 `lxml parser <http://lxml.de/>`_ .根据操作系统不同,可以选择下列不同的安装lxml的方法:

$ apt-get install python-lxml

$ easy_install lxml

$ pip install lxml

另一个可以选择的解析器是纯python代码的 `html5lib parser <http://code.google.com/p/html5lib/>`_ , html5lib parser的解析方式与浏览器相同,根据安装方式不同,可以选择下列方法安装:

$ apt-get install python-html5lib

$ easy_install html5lib

$ pip install html5lib

下表列出了主要的解析器,以及它们的优缺点:

+-----------------------+---------------------------+---------------------------+---------------------------+
|         解析器        |         使用方法          |            优势           |            劣势           |
+=======================+===========================+===========================+===========================+
| Python’s html.parser  | ``BeautifulSoup(markup,   | - Batteries included      | - Not very lenient (before|
|                       | "html.parser")``          | - Decent speed            |   Python 2.7.3 or 3.2.2)  |
|                       |                           | - Lenient (as of Python   |                           |
|                       |                           |   2.7.3 and 3.2.)         |                           |
|                       |                           |                           |                           |
+-----------------------+---------------------------+---------------------------+---------------------------+
| lxml’s HTML parser    | ``BeautifulSoup(markup,   | - Very fast               | - External C dependency   |
|                       | "lxml")``                 | - Lenient                 |                           |
|                       |                           |                           |                           |
+-----------------------+---------------------------+---------------------------+---------------------------+
| lxml’s XML parser     | ``BeautifulSoup(markup,   | - Very fast               |  - External C dependency  |
|                       | ["lxml", "xml"])``        | - The only currently      |                           |
|                       |                           |   supported XML parser    |                           |
|                       | ``BeautifulSoup(markup,   |                           |                           |
|                       | "xml")``                  |                           |                           |
+-----------------------+---------------------------+---------------------------+---------------------------+
| html5lib              | ``BeautifulSoup(markup,   | - Extremely lenient       | - Very slow               |
|                       | "html5lib")``             | - Parses pages the same   | - External Python depende |
|                       |                           |   way a web browser does  |                           |
|                       |                           | - Creates valid HTML5     |                           |
+-----------------------+---------------------------+---------------------------+---------------------------+

推荐使用lxml作为解析器,因为lxml效率更高. 在python2.7.3之前的版本和python3中3.2.2之前的版本,必须安装lxml或html5lib, 因为那些python版本的标准库中内置的HTML解析方法不是很好.

提示: 如果一段HTML或XML文档格式不正确的话,那么在不同的解析器中返回的结果可能是不一样的,查看 `Differences between parsers <http://www.baidu.com>`_ 了解更多细节

用好beautifulSoup
==================

将一段文档传入BeautifulSoup 的构造方法,就能得到一个文档的对象, 可以传入一段字符串或一个文件句柄.

::

    from bs4 import BeautifulSoup

    soup = BeautifulSoup(open("index.html"))

    soup = BeautifulSoup("<html>data</html>")

首先,文档被转换成unicode,并且HTML的实例都被转换成unicode编码

::

    BeautifulSoup("Sacr&eacute; bleu!")
    <html><head></head><body>Sacré bleu!</body></html>

然后,Beautiful Soup选择最合适的解析器来解析这段文档,如果手动指定解析器那么Beautiful Soup会选择指定的解析器来解析文档.(参考`Parsing XML <http://www.crummy.com/software/BeautifulSoup/bs4/doc/#id16>`_ )

对象分类
========

Beautiful Soup将复杂HTML文档转换成一个复杂的树形结构的python对象,所有对象都是4类对象中的一种: Tag, NavigableString, BeautifulSoup, Comment.

Tag
-----

Tag 对象与XML或HTML原生文档中的tag内容相符

::

    soup = BeautifulSoup('<b class="boldest">Extremely bold</b>')
    tag = soup.b
    type(tag)
    # <class 'bs4.element.Tag'>

Tag有很多方法和属性,在 ` Navigating the tree` 和 `Searching the tree` 有详细解释.现在介绍一下tag中最重要的属性: name和attributes

Name
..........

每个tag都有自己的名字,通过 .name 来获取

::

    tag.name
    # u'b'

如果改变了tag的name,那将影响所有通过当前Beautiful Soup对象生成的HTML文档

::

    tag.name = "blockquote"
    tag
    # <blockquote class="boldest">Extremely bold</blockquote>

Attributes
............

一个tag可能有很多个属性. tag *<b class="boldest">* 有一个 *class* 的属性,值为"boldest".tag的属性的操作方法与字典相同:

::

    tag['class']
    # u'boldest'

也可以直接"点"取属性, 比如: .attrs:

::

    tag.attrs
    # {u'class': u'boldest'}

tag的属性可以被添加,删除或修改. 再说一次, tag的属性操作方法与字典一样

::

    tag['class'] = 'verybold'
    tag['id'] = 1
    tag
    # <blockquote class="verybold" id="1">Extremely bold</blockquote>

    del tag['class']
    del tag['id']
    tag
    # <blockquote>Extremely bold</blockquote>

    tag['class']
    # KeyError: 'class'
    print(tag.get('class'))
    # None

多值属性
``````````

HTML 4定义了一系列可以包含多个值的属性.在HTML5中移除了一些,却增加更多.最常见的多值的属性是 class (一个tag可以有多个CSS的class). 还有一些属性  rel, rev, accept-charset, headers, accesskey. 在Beautiful Soup中多值属性的返回值被定义为list(无论是否有多个值):

::

    css_soup = BeautifulSoup('<p class="body strikeout"></p>')
    css_soup.p['class']
    # ["body", "strikeout"]

    css_soup = BeautifulSoup('<p class="body"></p>')
    css_soup.p['class']
    # ["body"]

如果某个属性看起来好像有多个值,但在任何版本的HTML定义中都没有被定义为多值属性,那么Beautiful Soup会将这个属性作为字符串返回

::

    id_soup = BeautifulSoup('<p id="my id"></p>')
    id_soup.p['id']
    # 'my id'

将tag转换成字符串时,多值属性会合并为一个值

::

    rel_soup = BeautifulSoup('<p>Back to the <a rel="index">homepage</a></p>')
    rel_soup.a['rel']
    # ['index']
    rel_soup.a['rel'] = ['index', 'contents']
    print(rel_soup.p)
    # <p>Back to the <a rel="index contents">homepage</a></p>

如果转换的文档是XML格式,那么tag中不包含多值属性

::

    xml_soup = BeautifulSoup('<p class="body strikeout"></p>', 'xml')
    xml_soup.p['class']
    # u'body strikeout'

NavigableString
----------------

字符串常被包含在tag内.Beautiful Soup用 *NavigableString* 类来包装tag中的字符串

::

    tag.string
    # u'Extremely bold'
    type(tag.string)
    # <class 'bs4.element.NavigableString'>

一个 *NavigableString* 字符串与python中的unicode字符串相同,并且还支持包含在 Navigating the tree 和 Searching the tree 中的一些特性. 通过unicode()方法可以直接将 *NavigableString* 对象转换成unicode字符串:

::

    unicode_string = unicode(tag.string)
    unicode_string
    # u'Extremely bold'
    type(unicode_string)
    # <type 'unicode'>

tag中包含的字符串不能编辑,但是可以被替换成其它的字符串,用 *replace_with()* 方法:

::

    tag.string.replace_with("No longer bold")
    tag
    # <blockquote>No longer bold</blockquote>

`NavigableString` 对象支持 `Navigating the tree` 和 `Searching the tree` 中定义的大部分属性, 并非全部.尤其是,一个字符串不能包含其它内容(tag能够包含字符串或是其它tag),字符串不支持 `.contents` 或 `.string` 属性或 `find()` 方法.

如果想在Beautiful Soup之外使用 `NavigableString` 对象,需要调用 `unicode()` 方法,将该对象转换成普通的unicode字符串,否则就算Beautiful Soup已方法已经执行结束,该对象的输出也会带有实例的引用地址.这样会浪费内存.

BeautifulSoup
----------------

`BeautifulSoup` 对象表示的是一个文档的整体.大部分时候,可以把它当作 `Tag` 对象,它支持 `Navigating the tree` 和 `Searching the tree` 中描述的大部分的方法.

因为 `BeautifulSoup` 对象并不是真正的HTML或XML的tag,所以它没有name和attribute属性.但有事查看它的 `.name` 属性是很方便的,所以 `BeautifulSoup` 对象包含了一个值为 "[document]" 的特殊属性 `.name`

::

    soup.name
    # u'[document]'

注释及特殊字符串
-----------------------

*Tag* , *NavigableString* , *BeautifulSoup* 几乎覆盖了html和xml中的所有内容,但是还有一些特殊对象.容易让人担心的内容是文档的注释部分:

::

    markup = "<b><!--Hey, buddy. Want to buy a used parser?--></b>"
    soup = BeautifulSoup(markup)
    comment = soup.b.string
    type(comment)
    # <class 'bs4.element.Comment'>

*Comment* 对象是一个特殊类型的 *NavigableString* 对象:

::

    comment
    # u'Hey, buddy. Want to buy a used parser'

但是当它出现在HTML文档中时, *Comment* 对象会使用特殊的格式输出:

::

    print(soup.b.prettify())
    # <b>
    #  <!--Hey, buddy. Want to buy a used parser?-->
    # </b>

Beautiful Soup中定义的其它类型都可能会出现在XML的文档中: *CData* , *ProcessingInstruction* , *Declaration* , *Doctype* .与 *Comment* 对象类似,这些类都是 *NavigableString* 的子类,只是添加了一些额外的方法的字符串独享.下面是用CDATA来替代注释的例子:

::

    from bs4 import CData
    cdata = CData("A CDATA block")
    comment.replace_with(cdata)

    print(soup.b.prettify())
    # <b>
    #  <![CDATA[A CDATA block]]>
    # </b>

文档树的操作
===================

还是拿"there sister"的文档来做例子:

::

    html_doc = """
    <html><head><title>The Dormouse's story</title></head>

    <p class="title"><b>The Dormouse's story</b></p>

    <p class="story">Once upon a time there were three little sisters; and their names were
    <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
    <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
    <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
    and they lived at the bottom of a well.</p>

    <p class="story">...</p>
    """

    from bs4 import BeautifulSoup
    soup = BeautifulSoup(html_doc)

通过这段例子来演示怎样从文档的一段内容跳到另一段内容

Going down
-------------

Tag可能包含字符串或其它的tag,这些内容都是tag的子节点.Beautiful Soup提供了许多操作和遍历子节点的属性.

注意: Beautiful Soup中字符串节点不支持操作自己点的属性,因为字符串没有子节点

tag的名字
..........

操作文档树最简单的方法就是告诉它你想获取的tag的name.如果想获取 <head> 标签,只要用 *soup.head* :

::

    soup.head
    # <head><title>The Dormouse's story</title></head>

    soup.title
    # <title>The Dormouse's story</title>

这是个获取tag的小窍门,可以在文档树的tag中多次调用这个方法.下面的代码可以获取<body>标签中的第一个<b>标签:

::

    soup.body.b
    # <b>The Dormouse's story</b>

通过点取属性的方式只能获得当前名字的第一个tag:

::

    soup.a
    # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

如果想要得到所有的<a>标签,或是通过名字得到比一个tag更多的内容的时候,就需要用到 `Searching the tree` 中描述的方法,比如: find_all()

::

    soup.find_all('a')
    # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
    #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
    #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

.contents 和 .children
........................

tag的 *.contents* 属性可以将tag的子节点以列表的方式输出:

::

    head_tag = soup.head
    head_tag
    # <head><title>The Dormouse's story</title></head>

    head_tag.contents
    [<title>The Dormouse's story</title>]

    title_tag = head_tag.contents[0]
    title_tag
    # <title>The Dormouse's story</title>
    title_tag.contents
    # [u'The Dormouse's story']

BeautifulSoup对象本身一定会包含子节点,这说明<html>标签也是 BeautifulSoup 对象的子节点:

::

    len(soup.contents)
    # 1
    soup.contents[0].name
    # u'html'

BeautifulSoup中的字符串没有 *.contents* 属性,因为字符串不能包含人和内容:

::

    text = title_tag.contents[0]
    text.contents
    # AttributeError: 'NavigableString' object has no attribute 'contents'

通过tag的 *.children* 生成器,可以对tag的子节点进行循环:

::

    for child in title_tag.children:
        print(child)
        # The Dormouse's story

.descendants
..............

.contents 和 .children 属性仅包含tag的直接子节点.例如,<head>标签有一个直接子节点<title>

::

    head_tag.contents
    # [<title>The Dormouse's story</title>]

但是<title>标签有一个子节点:字符串 “The Dormouse’s story”,这种情况下字符串 “The Dormouse’s story”也属于<head>标签的子节点. *.descendants* 属性可以对所有tag的子节点进行递归循环,采用先序遍历方式:

::

    for child in head_tag.descendants:
        print(child)
        # <title>The Dormouse's story</title>
        # The Dormouse's story



Python_

`BeautifulSoup3 文档`_

.. _Python: http://www.python.org
.. _`BeautifulSoup3 文档`: http://www.crummy.com/software/BeautifulSoup/bs3/documentation.zh.html


.. [1] BeautifulSoup的googl讨论组不是很活跃,可能是因为库已经比较完善了吧

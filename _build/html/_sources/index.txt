.. BeautifulSoup文档 documentation master file, created by
   delong wang on Fri Nov 29 13:49:30 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Beautiful Soup 4.2.0 文档
==========================

.. image:: _static/cover.jpg
    :align: right

`BeautifulSoup <http://www.crummy.com/software/BeautifulSoup/>`_ 是一个可以从HTML或XML文件中提取数据的python库.它能够通过你喜欢的转换器实现惯用的文档导航,查找,修改文档的方式.使用BeautifulSoup会帮助你节省数小时甚至数天的工作.

这篇文档介绍了BeautifulSoup4中所有主要特性,并切有小例子.让我来向你展示它适合做什么,如何工作,怎样使用,如何达到你想要的效果,和处理异常情况.

文档中出现的例子在python2.7和python3.2中的执行结果相同

你可能在寻找 `BeautifulSoup3 <http://www.crummy.com/software/BeautifulSoup/bs3/documentation.html>`_ 的文档,Beautiful Soup 3 目前已经停止开发,我们推荐在现在的项目中使用Beautiful Soup 4, `移植到BS4 <http://www.baidu.com>`_

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

如何使用
========

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

对象的类型
==========

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

操作文档树
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

子节点
-------

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

但是<title>标签有一个子节点:字符串 “The Dormouse’s story”,这种情况下字符串 “The Dormouse’s story”也属于<head>标签的子节点. *.descendants* 属性可以对所有tag的子节点进行递归循环[5]_:

::

    for child in head_tag.descendants:
        print(child)
        # <title>The Dormouse's story</title>
        # The Dormouse's story

上面的例子中, <head>标签只有一个子节点,但是有2个子孙节点:<head>节点和<head>的子节点, ``BeautifulSoup`` 有一个直接子节点(<html>节点),却有很多子孙节点:

::

    len(list(soup.children))
    # 1
    len(list(soup.descendants))
    # 25

.string
........

如果tag只有一个 ``NavigableString`` 类型子节点,那么这个tag可以使用 ``.string`` 得到子节点:

::

    title_tag.string
    # u'The Dormouse's story'

如果一个tag仅有一个子节点,那么这个tag也可以使用 ``.string`` 方法,输出结果与当前唯一子节点的 ``.string`` 结果相同:

::

    head_tag.contents
    # [<title>The Dormouse's story</title>]

    head_tag.string
    # u'The Dormouse's story'

如果tag包含了多个子节点,tag就无法确定 ``.string`` 方法应该调用哪个子节点的内容, ``.string`` 的输出结果是 ``None`` :

::

    print(soup.html.string)
    # None

.strings 和 stripped_strings
.............................

如果tag中包含多个字符串 [2]_ ,可以使用 ``.strings`` 来循环获取:

::

    for string in soup.strings:
        print(repr(string))
        # u"The Dormouse's story"
        # u'\n\n'
        # u"The Dormouse's story"
        # u'\n\n'
        # u'Once upon a time there were three little sisters; and their names were\n'
        # u'Elsie'
        # u',\n'
        # u'Lacie'
        # u' and\n'
        # u'Tillie'
        # u';\nand they lived at the bottom of a well.'
        # u'\n\n'
        # u'...'
        # u'\n'

输出的字符串中可能包含了很多空格或空行,使用 ``.stripped_strings`` 可以去除多余空白内容:

::

    for string in soup.stripped_strings:
        print(repr(string))
        # u"The Dormouse's story"
        # u"The Dormouse's story"
        # u'Once upon a time there were three little sisters; and their names were'
        # u'Elsie'
        # u','
        # u'Lacie'
        # u'and'
        # u'Tillie'
        # u';\nand they lived at the bottom of a well.'
        # u'...'

全部是空格的行会被忽略掉,段首和段末的空白会被删除

父节点 
-------

继续分析文档树,每个tag或字符串都有父节点:被包含在某个tag中

.parent
........

通过 ``.parent`` 属性来获取某个元素的父节点.在例子“three sisters”的文档中,<head>标签是<title>标签的父节点:

::

    title_tag = soup.title
    title_tag
    # <title>The Dormouse's story</title>
    title_tag.parent
    # <head><title>The Dormouse's story</title></head>

文档title的字符串也有父节点:<title>标签

::

    title_tag.string.parent
    # <title>The Dormouse's story</title>

文档的顶层节点比如<html>的父节点是 ``BeautifulSoup`` 对象:

::

    html_tag = soup.html
    type(html_tag.parent)
    # <class 'bs4.BeautifulSoup'>

``BeautifulSoup`` 对象的父节点是None:

::

    print(soup.parent)
    # None

.parents
..........

通过元素的 ``.parents`` 属性可以递归得到元素的所有父辈节点,下面的例子使用了 ``.parent`` 方法遍历了<a>标签到根节点的所有节点.

::

    link = soup.a
    link
    # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
    for parent in link.parents:
        if parent is None:
                print(parent)
                    else:
                            print(parent.name)
                            # p
                            # body
                            # html
                            # [document]
                            # None

兄弟节点
---------

看一段简单的例子:

::

    sibling_soup = BeautifulSoup("<a><b>text1</b><c>text2</c></b></a>")
    print(sibling_soup.prettify())
    # <html>
    #  <body>
    #   <a>
    #    <b>
    #     text1
    #    </b>
    #    <c>
    #     text2
    #    </c>
    #   </a>
    #  </body>
    # </html>

因为<b>标签和<c>标签是同一层:他们是同一个元素的子节点,所以<b>和<c>可以被称为兄弟节点.一段文档以标准格式输出时,兄弟节点有相同的缩进级别.在代码中也可以使用这种关系

.next_sibling 和 .previous_sibling
....................................

在文档的树形结构中,可以使用 ``.next_sibling`` 和 ``.previous_sibling`` 属性来查询兄弟节点:

::

    sibling_soup.b.next_sibling
    # <c>text2</c>

    sibling_soup.c.previous_sibling
    # <b>text1</b>

<b>标签有 ``.next_sibling`` 属性,但是没有 ``.previous_sibling`` 属性,因为<b>标签在同级接点中是第一个.同理,<c>标签有 ``.previous_sibling`` 属性,却没有 ``.next_sibling`` 属性:

::

    print(sibling_soup.b.previous_sibling)
    # None
    print(sibling_soup.c.next_sibling)
    # None

例子中的字符串 “text1” 和 “text2”没有兄弟节点,因为它们的父节点不同:

::

    sibling_soup.b.string
    # u'text1'

    print(sibling_soup.b.string.next_sibling)
    # None

实际操作中大部分的文档中的tag的 ``.next_sibling`` 和 ``.previous_sibling`` 属性通常是字符串或空白. 看看“three sisters”文档:

::

    <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>
    <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a>
    <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>

如果你以为第一个<a>标签的 ``.next_sibling`` 结果是第二个<a>标签,那就错了,真是结果是第一个<a>标签和第二个<a>标签之间的顿号和换行符:

::
    
    link = soup.a
    link
    # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

    link.next_sibling
    # u',\n'

第二个<a>标签是顿号的 ``.next_sibling`` 属性:

::

    link.next_sibling.next_sibling
    # <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>

.next_siblings 和 .previous_siblings
......................................

通过 ``.next_siblings`` 和 ``.previous_siblings`` 属性可以对当前节点的兄弟节点迭代输出:

::

    for sibling in soup.a.next_siblings:
        print(repr(sibling))
        # u',\n'
        # <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
        # u' and\n'
        # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
        # u'; and they lived at the bottom of a well.'
        # None

        for sibling in soup.find(id="link3").previous_siblings:
            print(repr(sibling))
            # ' and\n'
            # <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
            # u',\n'
            # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
            # u'Once upon a time there were three little sisters; and their names were\n'
            # None

回退和前进
----------

看一下“three sisters” 文档:

::

    <html><head><title>The Dormouse's story</title></head>
    <p class="title"><b>The Dormouse's story</b></p>

HTML解析器把这段字符串转换成一连串的事件: "打开<html>标签","打开一个<head>标签","打开一个<title>标签","添加一段字符串","关闭<title>标签","打开<p>标签",等等.Beautiful Soup提供了重现解析器初始化过程的工具.

.next_element 和 .previous_element
...................................

``.next_element`` 属性指向解析过程中下一个被解析的对象(字符串或tag),结果可能与 ``.next_sibling`` 相同,但通常是不一样的.

这是“three sisters”文档中最后一个<a>标签,它的 ``.next_sibling`` 结果是一个字符串,因为当前的解析过程 [2]_ 因为当前的解析过程因为遇到了<a>标签而终端了:

::

    last_a_tag = soup.find("a", id="link3")
    last_a_tag
    # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>

    last_a_tag.next_sibling
    # '; and they lived at the bottom of a well.'

但这个<a>标签的 ``.next_element`` 属性结果是是<a>被解析之后的解析内容,不是<a>标签后的句子部分,而是字符串"Tillie":

::

    last_a_tag.next_element
    # u'Tillie'

这是因为在原始文档中,字符串“Tillie” 在分号前出现,解析器先进入<a>标签,然后是字符串“Tillie”,然后关闭</a>标签,然后是分号和剩余部分.分号与<a>标签在同一层级,但是字符串“Tillie”会被先解析.

``.previous_element`` 属性刚好与 ``.next_element`` 相反,它只想当前被解析的对象的前一个解析对象:

::

    last_a_tag.previous_element
    # u' and\n'
    last_a_tag.previous_element.next_element
    # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>

.next_elements 和 .previous_elements
.....................................

通过 ``.next_elements`` 和 ``.previous_elements`` 的迭代器就可以向前或向后访问文档的解析内容,就好像文档正在被解析一样:

::

    for element in last_a_tag.next_elements:
        print(repr(element))
        # u'Tillie'
        # u';\nand they lived at the bottom of a well.'
        # u'\n\n'
        # <p class="story">...</p>
        # u'...'
        # u'\n'
        # None

搜索文档树
=============

Beautiful Soup定义了很多搜索方法,这里着重介绍2个方法: ``find()`` 和 ``find_all()`` .其它方法的参数和用法类似,请读者举一反三.

再以“three sisters”文档作为例子:

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

使用 ``find_all()`` 类似的方法可以定位到想要查找的文档内容

过滤器
------

介绍 ``find_all()`` 方法前,先介绍一下过滤器的类型 [3]_ ,这些过滤器贯穿整个搜索的API.过滤器可以被用在tag的name中,节点的属性中,字符串中或他们的混合中

字符串
............

最简单的过滤器是字符串.在搜索方法中传入一个字符串参数,Beautiful Soup会查找与字符串完整匹配的内容,下面的例子用于查找文档中所有的<b>标签:

::

    soup.find_all('b')
    # [<b>The Dormouse's story</b>]

如果传入字节码参数,Beautiful Soup会当作UTF-8编码,可以传入一段Unicode 编码来避免Beautiful Soup解析编码出错

正则表达式
..............

如果传入正则表达式作为参数,Beautiful Soup会通过正则表达式的 ``match()`` 来匹配内容.下面例子中找出所有以b开头的标签,这表示<body>和<b>标签都应该被找到:

::

    import re
    for tag in soup.find_all(re.compile("^b")):
        print(tag.name)
        # body
        # b

下面代码找出所有包含h的标签:

::

    for tag in soup.find_all(re.compile("t")):
        print(tag.name)
        # html
        # title

列表
..............

如果传入列表参数,Beautiful Soup会将与列表中任一元素匹配的内容返回.下面代码找到文档中所有<a>标签和<b>标签:

::

    soup.find_all(["a", "b"])
    # [<b>The Dormouse's story</b>,
    #  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
    #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
    #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

True
.....

``True`` 可以匹配任何值,下面代码查找到所有的tag,但是没有查找到字符串

::

    for tag in soup.find_all(True):
        print(tag.name)
        # html
        # head
        # title
        # body
        # p
        # b
        # p
        # a
        # a
        # a
        # p

方法
....

如果没有合适过滤器,那么还可以定义一个方法,方法只接受一个元素参数 [4]_ ,如果这个方法返回 ``True`` 表示当前元素匹配并且被找到,如果不是则放回 ``False``

下面方法校验了当前元素,如果包含 ``class`` 属性却不包含 ``id`` 属性,那么将返回 ``True``:

::

    def has_class_but_no_id(tag):
        return tag.has_attr('class') and not tag.has_attr('id')

将这个方法作为参数传入 ``find_all()`` 方法,将得到所有<p>标签:

::

    soup.find_all(has_class_but_no_id)
    # [<p class="title"><b>The Dormouse's story</b></p>,
    #  <p class="story">Once upon a time there were...</p>,
    #  <p class="story">...</p>]

返回结果中只有<p>标签没有<a>标签,因为<a>标签还定义了"id",没有返回<html>和<head>,因为<html>和<head>中没有定义"class"属性.

下面代码找到所有被文字包含的节点内容:

::

    from bs4 import NavigableString
    def surrounded_by_strings(tag):
        return (isinstance(tag.next_element, NavigableString)
                    and isinstance(tag.previous_element, NavigableString))

                    for tag in soup.find_all(surrounded_by_strings):
                        print tag.name
                        # p
                        # a
                        # a
                        # a
                        # p

现在来了解一下搜索方法的细节

find_all()
-----------

参数: find_all(name, attrs, recursive, text, limit,**kwargs)

``find_all()`` 方法搜索当前tag的所有tag子节点,并判断是否符合过滤器的条件.这里有几个例子:

::

    soup.find_all("title")
    # [<title>The Dormouse's story</title>]

    soup.find_all("p", "title")
    # [<p class="title"><b>The Dormouse's story</b></p>]

    soup.find_all("a")
    # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
    #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
    #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

    soup.find_all(id="link2")
    # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

    import re
    soup.find(text=re.compile("sisters"))
    # u'Once upon a time there were three little sisters; and their names were\n'

有几个方法是刚出现的,参数中的 ``text`` 和 ``id`` 是什么含义? 为什么 ``find_all("p", "title")`` 返回的是CSS Class为"title"的<p>标签? 我们来仔细看一下 ``find_all()`` 的参数

name 参数
..............

Beautiful Soup的 ``name`` 参数可以查找所有名字为 ``name`` 的tag,字符串对象会被自动忽略掉.

简单的用法如下:

::

    soup.find_all("title")
    # [<title>The Dormouse's story</title>]

重申: 搜索 ``name`` 参数的值可以使任一类型的 `过滤器`_ ,字符窜,正则表达式,列表,方法或是 ``True`` .

keyword 参数
..............

如果一个指定名字的参数不是搜索内置的参数名,搜索时会把该参数当作

按CSS搜索
..........

按照CSS类名搜索tag的功能非常实用,但标识CSS类名的关键字 ``class`` 在python中是保留字,使用 ``class`` 做参数会导致语法错误.从Beautiful Soup的4.1.1版本开始,可以通过 ``class_`` 参数搜索有指定类名的tag:

::

    soup.find_all("a", class_="sister")
    # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
    #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
    #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

``class_`` 参数同样接受不同类型的 ``过滤器`` ,字符串,正则表达式,方法或 ``True`` :

::

    soup.find_all(class_=re.compile("itl"))
    # [<p class="title"><b>The Dormouse's story</b></p>]

    def has_six_characters(css_class):
        return css_class is not None and len(css_class) == 6

        soup.find_all(class_=has_six_characters)
        # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
        #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
        #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

tag的 ``class`` 属性是 `多值属性`_ .按照CSS类名搜索tag时,可以分别搜索tag中的多个CSS类名:

::

    css_soup = BeautifulSoup('<p class="body strikeout"></p>')
    css_soup.find_all("p", class_="strikeout")
    # [<p class="body strikeout"></p>]

    css_soup.find_all("p", class_="body")
    # [<p class="body strikeout"></p>]

搜索 ``class`` 属性时也可以通过CSS值完全匹配:

::
    
    css_soup.find_all("p", class_="body strikeout")
    # [<p class="body strikeout"></p>]

完全匹配 ``class`` 的值时,如果CSS类名的数序与实际不符,将搜索不到结果:

::

    soup.find_all("a", attrs={"class": "sister"})
    # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
    #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
    #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

``text`` 参数
...............

通过 ``text`` 参数可以搜搜文档中的字符串内容.与 ``name`` 参数的可选值一样, ``text`` 参数接受 `字符串`_ , `正则表达式`_ , `列表`_, `True`_ . 看例子:

::

    soup.find_all(text="Elsie")
    # [u'Elsie']

    soup.find_all(text=["Tillie", "Elsie", "Lacie"])
    # [u'Elsie', u'Lacie', u'Tillie']

    soup.find_all(text=re.compile("Dormouse"))
    [u"The Dormouse's story", u"The Dormouse's story"]

    def is_the_only_string_within_a_tag(s):
        ""Return True if this string is the only child of its parent tag.""
            return (s == s.parent.string)
            
            soup.find_all(text=is_the_only_string_within_a_tag)
            # [u"The Dormouse's story", u"The Dormouse's story", u'Elsie', u'Lacie', u'Tillie', u'...']
            
虽然 ``text`` 参数用于搜索字符串,还可以与其它参数混合使用来过滤tag.Beautiful Soup会找到 ``.string`` 方法与 ``text`` 参数值相符的tag.下面代码用来搜索内容里面包含“Elsie”的<a>标签:

::

    soup.find_all("a", text="Elsie")
    # [<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>]

``limit`` 参数
...............

``recursive`` 参数
...................

像调用 ``find_all()`` 一样调用tag
----------------------------------

``find_all()`` 几乎是Beautiful Soup中最常用的搜索方法,所以我们定义了 ``find_all()`` 的简写方法. 一个 ``BeautifulSoup`` 对象和 ``tag`` 对象可以被当作一个方法来看待,这个方法的执行结果与调用这个对象的 ``find_all()`` 方法相同,下面两行代码是等价的:

::

    soup.find_all("a")
    soup("a")

这两行代码也是等价的:

::

    soup.title.find_all(text=True)
    soup.title(text=True)

find()
-------

find( `name`_ , `attrs`_ , `recursive`_ , `text`_ , `**kwargs`_ )

``find_all()`` 方法将返回文档中符合条件的所有tag,尽管有时候我们只想得到一个tag结果.假如你知道文档中只有一个<body>标签,那么使用 ``find_all()`` 方法来查找<body>标签是不划算的, ``find()`` 方法仅返回一个值.

``find_all()`` 方法没有找到目标是返回空列表, ``find()`` 方法找不到目标时,返回 ``None`` .

::

    print(soup.find("nosuchtag"))
    # None

``soup.head.title`` 是 `tag的名字`_ 方法的简写.这个简写的原理就是多次调用当前tag的 ``find()`` 方法:

::

    soup.head.title
    # <title>The Dormouse's story</title>

    soup.find("head").find("title")
    # <title>The Dormouse's story</title>

find_parents() 和 find_parent()
--------------------------------

find_parents( `name`_ , `attrs`_ , `recursive`_ , `text`_ , `**kwargs`_ )

find_parent( `name`_ , `attrs`_ , `recursive`_ , `text`_ , `**kwargs`_ )

我们已经用了很大篇幅来介绍 ``find_all()`` 和 ``find()`` 方法,Beautiful Soup中还有10个用于搜索的API.它们中的五个用的是与 ``find_all()`` 相同的搜索参数,另外5个与 ``find()`` 方法的搜索参数类似.区别仅是它们搜索文档的不同部分.

记住: ``find_all()`` 和 ``find()`` 只搜索当前节点的所有子节点,孙子节点等. ``find_parents()`` 和 ``find_parent()`` 用来搜索当前节点的父辈节点,搜索方法与普通tag的搜索方法相同,搜索文档\搜索文档包含的内容. 我们从一个文档中的一个自己点开始:

::

    a_string = soup.find(text="Lacie")
    a_string
    # u'Lacie'

    a_string.find_parents("a")
    # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

    a_string.find_parent("p")
    # <p class="story">Once upon a time there were three little sisters; and their names were
    #  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
    #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
    #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
    #  and they lived at the bottom of a well.</p>

    a_string.find_parents("p", class="title")
    # []

find_next_siblings() 合 find_next_sibling()
-------------------------------------------

find_next_siblings( `name`_ , `attrs`_ , `recursive`_ , `text`_ , `**kwargs`_ )

find_next_sibling( `name`_ , `attrs`_ , `recursive`_ , `text`_ , `**kwargs`_ )

这2个方法通过 `.next_siblings`_ 属性对当tag的所有后面 [5]_ 兄弟tag节点进行迭代, ``find_next_siblings()`` 方法返回所有符合条件的后面的兄弟节点, ``find_next_sibling()`` 只返回符合条件的后面的第一个tag节点.

::

    first_link = soup.a
    first_link
    # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

    first_link.find_next_siblings("a")
    # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
    #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

    first_story_paragraph = soup.find("p", "story")
    first_story_paragraph.find_next_sibling("p")
    # <p class="story">...</p>

find_previous_siblings() 和 find_previous_sibling()
-----------------------------------------------------

find_previous_siblings( `name`_ , `attrs`_ , `recursive`_ , `text`_ , `**kwargs`_ )

find_previous_sibling( `name`_ , `attrs`_ , `recursive`_ , `text`_ , `**kwargs`_ )

这2个方法通过 `.previous_siblings`_ 属性对当前tag的前面 [5]_ 的兄弟tag节点进行迭代, ``find_previous_siblings()`` 方法返回所有符合条件的前面的兄弟节点, ``find_previous_sibling()`` 方法返回第一个符合条件的前面的兄弟节点:

::

    last_link = soup.find("a", id="link3")
    last_link
    # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>

    last_link.find_previous_siblings("a")
    # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
    #  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

    first_story_paragraph = soup.find("p", "story")
    first_story_paragraph.find_previous_sibling("p")
    # <p class="title"><b>The Dormouse's story</b></p>

find_all_next() 和 find_next()
--------------------------------

find_all_next( `name`_ , `attrs`_ , `recursive`_ , `text`_ , `**kwargs`_ )

find_next( `name`_ , `attrs`_ , `recursive`_ , `text`_ , `**kwargs`_ )

这2个方法通过 `.next_elements`_ 属性对当前节点后面的tag和字符串进行迭代, ``find_all_next()`` 方法返回所有符合条件的节点, ``find_next()`` 方法返回第一个符合条件的节点:

::

    first_link = soup.a
    first_link
    # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

    first_link.find_all_next(text=True)
    # [u'Elsie', u',\n', u'Lacie', u' and\n', u'Tillie',
    #  u';\nand they lived at the bottom of a well.', u'\n\n', u'...', u'\n']

    first_link.find_next("p")
    # <p class="story">...</p>

第一个例子中,字符串 “Elsie”也被显示出来,尽管它被包含在我们开始查找的<a>标签的里面.第二个例子中,最后一个<p>标签也被显示出来,尽管它与我们开始查找位置的<a>标签不属于同一部分.例子中,搜索的重点是要匹配过滤器的条件,并且在文档中出现的顺序而不是开始查找的元素的位置.

find_all_previous() 和 find_previous()
---------------------------------------

find_all_previous( `name`_ , `attrs`_ , `recursive`_ , `text`_ , `**kwargs`_ )

find_previous( `name`_ , `attrs`_ , `recursive`_ , `text`_ , `**kwargs`_ )

这2个方法通过 `.previous_elements`_ 属性对当前节点前面的tag和字符串进行迭代, ``find_all_previous()`` 方法返回所有符合条件的节点, ``find_previous()`` 方法返回第一个符合条件的节点.

::

    first_link = soup.a
    first_link
    # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

    first_link.find_all_previous("p")
    # [<p class="story">Once upon a time there were three little sisters; ...</p>,
    #  <p class="title"><b>The Dormouse's story</b></p>]

    first_link.find_previous("title")
    # <title>The Dormouse's story</title>



CSS选择器
------------

修改文档树
===========

修改tag的名称和属性
-------------------

修改 .string
-------------

append()
----------

BeautifulSoup.new_string() 和 .new_tag()
-----------------------------------------

insert()
--------

insert_before() 和 insert_after()
-----------------------------------

clear()
--------

extract()
----------

decompose()
------------

replace_with()
---------------

wrap()
------

unwrap()
---------

输出
====

格式化输出
-----------

压缩输出
----------

输出格式
---------

get_text()
----------

指定文档解析器
==============

解析器之间的区别
-----------------

编码
====

输出编码
--------

unicode, 靠!
-------------

输出的编码
-----------

矛盾的编码
----------

转换部分文档
============

SoupStrainer
-------------

常见问题
========

diagnose()
----------

文档解析错误
-------------

版本错误
----------

解析XML
--------

解析器的错误
------------

杂项错误
--------

如何提高效率
------------

Beautiful Soup 3
=================

迁移到BS4
----------

需要的解析器
............

方法的名字
..........

生成器
.......

XML
....

实例
.....

迁移杂项
.........



Python_

`BeautifulSoup3 文档`_

.. _Python: http://www.python.org
.. _`BeautifulSoup3 文档`: http://www.crummy.com/software/BeautifulSoup/bs3/documentation.zh.html
.. _name: `name 参数`_
.. _attrs: `按CSS搜索`_
.. _recursive: `recursive 参数`_
.. _text: `text 参数`_
.. _**kwargs: `keyword 参数`_
.. _.next_siblings: `.next_siblings 和 .previous_siblings`_
.. _.previous_siblings: `.next_siblings 和 .previous_siblings`_
.. _.next_elements: `.next_elements 和 .previous_elements`_
.. _.previous_elements: `.next_elements 和 .previous_elements`_

注释文档
========

.. [1] BeautifulSoup的googl讨论组不是很活跃,可能是因为库已经比较完善了吧
.. [2] 文档被解析成树形结构,所以下一步解析过程应该是当前节点的子节点
.. [3] 过滤器只能作为搜索文档的参数,或者说应该叫参数类型更为贴切,原文中用了 ``filter`` 因此翻译为过滤器
.. [4] 元素参数,HTML文档中的一个tag节点,不能是文本节点
.. [5] 采用先序遍历方式

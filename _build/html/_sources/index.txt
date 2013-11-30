.. BeautifulSoup文档 documentation master file, created by
   sphinx-quickstart on Fri Nov 29 13:49:30 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Beautiful Soup 4.2.0 文档
==========================

`BeautifulSoup <http://www.crummy.com/software/BeautifulSoup/>`_ 是一个可以从HTML或XML文件中提取数据的python库.它能够通过你喜欢的转换器实现惯用的文档导航,查找,修改文档的方式.使用BeautifulSoup会帮助你节省数小时甚至数天的工作.

这篇文档介绍了BeautifulSoup4中所有主要特性,并切有小例子.让我来向你展示它适合做什么,如何工作,怎样使用,如何达到你想要的效果,和处理异常情况.

文档中出现的例子在python2.7和python3.2中的执行结果相同

你可能在寻找 `BeautifulSoup3 <http://www.crummy.com/software/BeautifulSoup/bs3/documentation.html>`_ 的文档,Beautiful Soup 3 目前已经停止开发,我们推荐在现在的项目中使用Beautiful Soup 4, `移植到BS4`

获取帮助
========

如果你有关于BeautifulSoup的问题,可以发送邮件到`讨论组 <https://groups.google.com/forum/?fromgroups#!forum/beautifulsoup>`_ .如果你的问题包含了一段需要转换的HTML代码,确保你提的问题中附带HTML文档的在`诊断代码`

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

$ apt-get install python-bs4

Beautiful Soup 4 通过PyPi发布,所以如果你无法使用系统包管理安装,那么也可以通过easy_install活pip来安装.包的名字是 *beautifulsoup4* ,这个包兼容python2和python3

$ easy_install beautifulsoup4

$ pip install beautifulsoup4

(在PyPi中还有一个名字是 *beautifulsoup的* 包,但那可能不是你想要的,那是 `BeautifulSoup3 <http://www.crummy.com/software/BeautifulSoup/bs3/documentation.html>`_ 的发布版本,因为很多项目还在使用BS3, 所以*beautifulsoup* 包依然有效,但是如果你想执行新版本的代码,那么你需要安装的包是 *beautifulsoup4*)

如果你没有安装 *easy_install* 或 *pip* ,那你也可以 `下载BS4的源码 <http://www.crummy.com/software/BeautifulSoup/download/4.x/>`_ ,然后通过setup.py来安装.

$ python setup.py install

如果上述安装方法都行不通,Beautiful Soup的发布协议允许你将BS4的代码打包在你的项目中,这样无须安装即可使用.

作者在Python2.7和Python3.2的版本下开发Beautiful Soup, Beautiful Soup应该在所有当前的python版本中正常工作

安装完成后的问题
=================

Beautiful Soup被打包成Python2版本的编码,当在Python3环境下安装时,会自动转换成python3的代码,如果你没有安装的过程,那么代码就不会被转换.

如果代码抛出了 *ImportError* 的异常: "No module named HTMLParser", 这是因为你在python3版本中执行python2版本的代码.


如果代码抛出了 *ImportError* 的异常: "No module named html.parser", 这是因为你在python2版本中执行python3版本的代码.

如果遇到上述2种情况,最好的解决方法是重新安装BeautifulSoup4.

如果遇到 *SyntaxError* "Invalid syntax" 在`ROOT_TAG_NAME = u'[document]'` ,需要将把BS4的python代码版本从python2转换到python3. 可以重新安装BS4:

$ python3 setup.py install

或在bs4的目录中执行python代码版本转换脚本

$ 2to3-3.2 -w bs4

安装解析器
==============

Beautiful Soup支持python标准库中的HTML解析器,还支持一些第三方的解析器,其中一个是 `lxml parser <http://lxml.de/>`_ .根据操作系统不同,可以选择下列不同的安装lxml的方法:

$ apt-get install python-lxml

$ easy_install lxml

$ pip install lxml

另一个可以选择的解析器是纯python代码的 `html5lib parser <http://code.google.com/p/html5lib/>`_ , html5lib parser的解析方式与浏览器相同,根据安装方式不同,可以选择下列方法安装:

$ apt-get install python-html5lib

$ easy_install html5lib

$ pip install html5lib

下表列出了主要的解析器,以及它们的优缺点:

========    =========      ======      ======
解析器       使用方法        优点        缺点
========    =========      ======      ======
1
========    =========      ======      ======

推荐使用lxml作为解析器,因为lxml效率更高. 在python2.7.3之前的版本和python3中3.2.2之前的版本,必须安装lxml或html5lib, 因为那些python版本的标准库中内置的HTML解析方法不是很好.

提示: 如果一段HTML或XML文档格式不正确的话,那么在不同的解析器中返回的结果可能是不一样的,查看 `Differences between parsers <>`_ 了解更多细节

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
-----

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
-----------

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
-----------

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



Python_

.. _Python: http://www.python.org


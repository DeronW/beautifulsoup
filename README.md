# Beautiful Soup4 文档

Beautiful Soup 升级到 4.x 版本后官方没有继续更新中文文档, 为了方便大家阅读, 
将 4.x 版本文档翻译成中文, 如有纰漏还望指正。

官方网站: http://www.crummy.com/software/BeautifulSoup/

原版文档: http://www.crummy.com/software/BeautifulSoup/bs4/doc/

### 文档原文

在[官方文档](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 侧边栏最下有一个 
[Show Source](https://www.crummy.com/software/BeautifulSoup/bs4/doc/_sources/index.rst.txt) 链接，
点击可以查看文档的原始 rst 格式文件。文档原文未标记版本号，不方便对比版本变化，所以下载下来并放在 `source` 目录，
文档源文件是单个文件，所以用 `[version].rst` 格式命名，方便对比。

### 编译文档

```shell
# 安装 sphinx 编译工具
pip install sphinx
```

编译 html 命令

```shell
sphinx-build -b html docs docs/\_build
```

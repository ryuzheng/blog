---
title: "PyInstaller 打包 python 脚本的一些心得"
date: 2016-11-08T14:01:00+08:00
published: true
slug: pyinstaller-da-bao-python-jiao-ben-de-yi-xie-xin-de
tags: ['PyInstaller', '代码']
cover_image: ./images/pyinstaller.png
canonical_url: false
description: ""
---



![Pyinstaller][./images/pyinstaller.png]

因为在公司经常要帮同事做一个从excel表格中提取出需要的内容的重复工作，比较繁琐还容易出错；于是就想着要写个程序，但是同事又不可能在电脑上也装上python以及相关的包依赖（别人一看就觉得太麻烦而且太冗余），于是就想着将写好的python脚本打包成exe，直接双击使用，方便快捷。

说干就干，先是花点时间写完了脚本；然后搜索了相关的关键词，找到了py2exe、PyInstaller、cx_Freeze等工具，最后确定使用[PyInstaller](http://www.pyinstaller.org/)。

使用PyInstaller有几个原因：

 - PyInstaller现在仍然在更新
 - PyInstaller使用方法简单，py2exe比较繁琐
 - PyInstaller网上教程比较多

##安装PyInstaller
推荐使用pip安装

```bash
pip install pyinstaller -i https://pypi.douban.com/simple
```

后面加的`-i https://pypi.douban.com/simple`是使用豆瓣的源镜像，在天朝速度会快很多；如果你担心安全问题或者网速够快，可以不加，使用官方的源。

安装完后，直接

```bash
pyinstaller
usage: pyinstaller-script.py [-h] [-v] [-D] [-F] [--specpath DIR] [-n NAME]
                             [-p DIR] [--hidden-import MODULENAME]
                             [--additional-hooks-dir HOOKSPATH]
                             [--runtime-hook RUNTIME_HOOKS]
                             [--exclude-module EXCLUDES] [--key KEY] [-d] [-s]
                             [--noupx] [-c] [-w]
                             [-i <FILE.ico or FILE.exe,ID or FILE.icns>]
                             [--version-file FILE] [-m <FILE or XML>]
                             [-r RESOURCE] [--uac-admin] [--uac-uiaccess]
                             [--win-private-assemblies]
                             [--win-no-prefer-redirects]
                             [--osx-bundle-identifier BUNDLE_IDENTIFIER]
                             [--distpath DIR] [--workpath WORKPATH] [-y]
                             [--upx-dir UPX_DIR] [-a] [--clean]
                             [--log-level LEVEL] [--upx UPX]
                             scriptname [scriptname ...]
pyinstaller-script.py: error: the following arguments are required: scriptname
```
可以看到PyInstaller的信息，说明安装完成，可以使用了；详细帮助可以`pyinstaller -h`查看。

##使用PyInstaller

PyInstaller的使用非常简单：

```
# 打包成一个文件
pyinstaller -F test.py
# 打包成文件夹（默认）
pyinstaller test.py
```
因为要精简到底，所以我选择打包成一个文件，打包完成后，打开`dist`文件夹，里面的那个exe文件就是打包好的程序，运行测试一下是否打包成功。

PyInstaller本身也是有很多选项的。这里挑几个主要的说明一下：

 - `-D, --one-dir`打包成一个文件夹，默认
 - `-F, --one-file`打包成一个exe文件
 - `-p DIR, --paths DIR`添加路径，一般用来添加程序所用到的包的所在位置
 - `-c, --console, --nowindowed`提供程序视窗，程序有输入输出的界面，默认
 - `-w, --windowed, --noconsole`无视窗，程序后台运行
 - `-i <FILE.ico or FILE.exe,ID or FILE.icns>, --icon <FILE.ico or FILE.exe,ID or FILE.icns>`添加icon图标

##openpyxl的一个错误
程序打包之后一定要测试一下是否能成功运行，不然会在同事面前出糗，另外还需要用另一台电脑测试一下。

当我试着运行程序时，发生了报错：

```
Traceback (most recent call last):
  File "test.py", line 72, in <module>
  File "site-packages\pandas\core\frame.py", line 1414, in to_excel
  File "site-packages\pandas\io\excel.py", line 609, in __new__
  File "site-packages\pandas\io\excel.py", line 59, in get_writer
AttributeError: module 'openpyxl' has no attribute '__version__'
Failed to execute script test
```

于是就上网查找原因，一开始是去打包生成的`build`文件夹下面，查看一个名为`warntest.txt`的文件（这里是warn[yourscriptname].txt），发现有很多module都是miss，没有加载到。

```
missing module named 'win32com.gen_py' - imported by win32com, c:\python35\lib\site-packages\PyInstaller\loader\rthooks\pyi_rth_win32comgenpy.py
missing module named sys.exc_info - imported by sys, openpyxl.reader.excel, win32com.server.dispatcher
missing module named pywintypes.IIDType - imported by pywintypes, win32com.client.dynamic
missing module named win32com.client._get_good_object_ - imported by win32com.client, win32com.client.util
...
```

于是就想着会不会是包的路径没有加载，尝试着使用`-p path`去加载python下面储存package的目录，结果，重新打包一次仍然还是同样的报错。

于是我重新通过`AttributeError: module 'openpyxl' has no attribute '__version__'`去搜索结果，发现有人遇到同样的bug，原因是使用了**Pandas**，但是pyinstaller在pandas引用的‘openpyxl’包中，无法读取版本信息。*（一般使用Python处理科学数据都会使用到Pandas，我是处理excel文件的脚本，当然会用到openpyxl来读写excel）*

解决的办法是在PyInstaller的`hook`文件夹中添加‘openpyxl’的一个读取版本信息的hook。这个hook文件是在PyInstaller的[Github issue](https://github.com/pyinstaller/pyinstaller/pull/2066)上找到的。于是添加了这个hook，再重新打包，然后运行测试，终于成功了。

关于前面说到的`warn[youscriptname].txt`文件，有一种说法是如果你在其中没有找到你所用到的包，那么里面的错误信息一般可以忽略，anyway，我是直接忽略掉的。

##添加版本信息

辛辛苦苦写了个程序，当然希望给程序签个名；PyInstaller是可以添加你自己的个人版本信息的，详细可以参考[《Creating an Executable from a Python Script》](https://mborgerson.com/creating-an-executable-from-a-python-script)，写的非常详细，依照相同的格式修改`version.txt`即可。但不知什么原因，我试了好几次都没有成功。如果以后找到原因，我再更新这篇文章。

##关于32位和64位

这个算是后续剧情了。当把程序发给一位同事时，没想到同事的win系统是32位的，而我一直使用的都是64位的系统和64位的程序，而PyInstaller好像没有能选择生成32位/64位exe的选项；也算是PyInstaller的一个缺点吧。最后我是安装了32位的Python以及依赖的库重新生成32位的exe才解决这个问题；所以以后也许生成32位的程序兼容性更好。

##Reference
 - [PyInstaller official website](http://www.pyinstaller.org/)
 - [【记录】用PyInstaller把Python代码打包成单个独立的exe可执行文件](http://www.crifan.com/use_pyinstaller_to_package_python_to_single_executable_exe/)
 - [Creating an Executable from a Python Script](https://mborgerson.com/creating-an-executable-from-a-python-script)
 - [Add openpyxl hook](https://github.com/pyinstaller/pyinstaller/pull/2066)
 - [COMPILING PYTHON USING PYINSTALLER](https://metac0rtex.com/compiling-python-using-pyinstaller/)
 - [pyinstaller简洁教程](http://legendtkl.com/2015/11/06/pyinstaller/)

---
layout: post
title: "Pyqtgraph 中文文档"
subtitle: "本文是对pyqtgraph英文文档的中文翻译 原文档链接：https://pyqtgraph.readthedocs.io/en/latest/introduction.html"
date: 2021-09-22
author: 七月
category: 技术文档
tags:翻译 可视化 python
finished: false
---

## 简介

### 什么是pyqtgraph? ###

PyQtGraph是一个提供工程和科学应用程序中常用功能的用于python环境的图形和用户界面库。它的主要目标是：1）提供用于显示数据（数据、视频等）的快速、交互式图形。2）提供帮助快速应用开发（例如Qt Designer的属性树）的工具。

PyQtGraph 大量使用Qt GUI平台（通过PyQt 或者 PySide）实现高性能图形，同时大量使用numpy实现大量的数字处理。特别是，pyqtgraph使用了QT的GraphicsView框架，这个框架本事就是一个非常强大的图形系统；我们对这个框架做了优化和简化，使得用户可以通过最少的操作来实现数据可视化。

Pyqtgraph 可以在 Linux, Windows， 和OSX三个平台上运行。

### 它能做什么 ? ###

pyqtgraph 的核心功能如下：

- 基本数据可视化操作：图像、线图和散点图
- 视频/线图的实时更新
- 交互式缩放/平移，平均，FFT，SVG/PNG导出
- 标记/选择绘图区域
- 标记/选择感兴趣的图像区域和对多维图像数据自动切片
- 用于构建自定义图像感兴趣区域部件的框架
- 取代/补充Qt的对接系统，允许更复杂（和更可预测）的对接安排
- 快速原型化动态界面的参数树部件（类似于Qt Desinger 和许多其他应用程序中的属性树）

### 示例 ###

PyQtGraph 提供了大量的例子，可以通过命令 ` python -m pyqtgraph.examples ` 或者如下代码访问。

```python
import pyqtgraph.examples
pyqtgraph.exmaples.run()
```

如果这个项目仓库在本地，可以通过在根目录下运行`python examples/` 来查看示例。

上述命令或者代码会启动一个带有示例列表的启动器。从列表中选择一个项目以查看其源代码，并双击一个项目以运行示例。

![image-20210922104154313](/img/2021-09-22-pyqtgraph//image-20210922104154313.png)

注意，如果你是通过`python setup.py develop`命令来安装pyqtgraph的，那么示例会被作为最上层模块。这种情况下，要用`import examples; examples.run()`。

### 和别的工具比...

- matplotlib：对于绘图，pyqtgraph 不像matplotlib那样完整/成熟, 但是它运行速度更快。Matplotlib更倾向于制作出版物质量的图形，而pyqtgraph 是用于数据采集和分析应用程序。Matplotlib 对于matlab程序员来说更加直观；Pyqtgraph 对于python/qt程序员来说更加直观。据我所知，Matplotlib 不包括许多pyqtgraph拥有的特性，例如图像交互、体积渲染、参数树、流程图等。
- pyqwt5：几乎和pyqtgraph 一样快，但是在绘图功能上不完整。pyqtgraph中的图像处理要完整得多（同样的，qwt中没有ROI部件）。另外，pyqtgraph 是用纯python编写的，所以比pyqwt更容易移植，而pyqwt在开发过程中往往落后于pyqt(我最初使用pyqwt，但是后来觉得我的项目中依赖它太麻烦了)。据我所知，与matplotlib一样，pyqwt不包括许多pyqtgraph拥有的特性，如图像交互、体积渲染、参数树、流程图等。

（我使用这些库的经验有些过时了；如果我说错了，请纠正我）

## 鼠标交互

大多数使用pyqtgraph实现数据可视化的应用程序都会生成一个可以使用鼠标交互式缩放、平移和配置的部件。本节介绍鼠标与这些部件的交互。

### 2D 图形

在pyqtgraph里，大多数2D 可视化都有如下的鼠标交互操作：

- 鼠标左键：与场景中的物体进行交互（选择/移动物体等）。如果鼠标光标下没有可移动的物体，那么拖动鼠标左键将会平移场景。
- 鼠标右键拖拽：缩放场景。按住鼠标右键左右拖动会在水平方向上缩放场景，上下拖动会在垂直方向上缩放场景（虽然有些场景的X/Y轴坐标是绑定在一起的）。如果场景中X/Y轴可见，那么在轴上拖拽只会影响这个轴。
- 鼠标右键点击：大多数情况下点击鼠标右键会显示一个上下文菜单，该菜单根据鼠标光标下的对象提供各种选项。
- 中间按钮（滚轮）拖拽：按住滚轮拖动鼠标总是会移动场景（当使用左键移动被其他物体组织的情况下很有用）。
- 滚轮转动：缩放场景

对于那些通过鼠标右键或者中间按钮拖动比较困难的机器（例如MAC)，有另一种鼠标交互模式。在这种模式下，按住鼠标左键拖动会在场景上绘制一个方框。当按钮被释放后，场景被缩放和平移以适应这个方框。这个模式可以通过上下文菜单或者如下调用来访问：

```python
pyqtgraph.setConfigOption('leftButtonPan', False)
```

### 上下文菜单

大多数场景下点击鼠标右键会显示一个上下文菜单，其中有各种选项可以修改场景的行为。一些可用的选项有：

- 启用或者禁止当数据方位改变时自动缩放
- 多视图的坐标轴链接在一起
- 启动或者禁止每个轴的鼠标交互
- 显示地设置可见范围值

菜单中确切的可用选项取决于场景的内容和所点击的对象。

### 3D 图形

3D可视化使用下面的鼠标交互：

- 左键拖动：围绕中心点旋转场景
- 中间按钮拖动：通过在X-Y平面内移动中心“观察”点来平移场景
- 按住CTRL键的同时拖动中间按钮：通过沿着Z轴移动中心“观察点”来平移场景
- 滚轮转动：缩放场景
- 滚轮+CTRL：改变视场角

还有键盘控制：

- 方向键围绕中心点旋转，就像拖动鼠标左键一样

## 怎么使用pyqtgraph

pyqtgraph的一些建议使用方式：

- 交互式shell（python -i, ipython 等）
- 显示应用程序的弹出窗口
- PyQt应用程序的集成部件

### 使用命令行

PyQtGraph 是的通过命令行实现数据可视化非常容易。

```python
import pyqtgraph as pg
pg.plot(data) # data can be a list of values of numpy array
```

上述代码会打开一个窗口显示给出数据的一条拟合线。pg.plot 函数返回一个句柄，这个句柄允许同一个窗口添加更多数据。注意：从python 提示符进行交互式绘图只能在PyQt中使用。当交互式提示符运行的时候，PySide 不能运行Qt 事件循环。如果你希望使用pyqtgraph和PySide进行交互，参考 'console' 示例。

更多的例子：

```python
pw = pg.plot(xVals, yVals, pen='r') #plot x vs y in red
pw.plot(xVals, yVals, pen='b')

win = pg.GraphicsWindow() # Automatically generates grids with multiple items
win.addPlot(data1, row=0, col=0)
win.addPlot(data2, row=0, col=1)
win.addPlot(data3, row=1, col=1, colspan=2)

pg.show(imageData) #imageData must be a numpy array with 2 to 4 dimenstions
```

这里我们只接触了表面——这些函数接受许多不同的数据格式和选项，用于定制数据的外观。

### 在一个应用程序中显示窗口

虽然我认为这种方法有点懒惰，但通常情况下，“懒惰”和“高效”是无法区分的。这里的方法只是使用与命令行上使用的功能完全相同的功能，但是是从现有的应用程序中使用。当我只是想获得应用程序中数据状态的即时反馈，而不需要花时间为它构建用户界面时，我经常使用这个方法。

### 在PyQt应用程序里嵌入部件

对于认真的应用程序开发人员，pyqtgraph中的所有功能都可以部件来实现，这些部件可以下像任何其他Qt部件一样被嵌入。最重要最常见的部件有，PlotWidget, ImageView, GraphicsLayoutWidget， 和 GraphicsView。PyQtGraph的部件可以通过“Promote To..." 包含在Designer 的ui文件里：

1. 在Designer里，创建一个QGraphicsView 部件（”Display Widgets" 下 "Graphics View" 类别）
2. 在QGraphicsView 点击右键， 然后选择 “提升至..."
3. 在”提升的类名“ 中输入你想使用的名称（"PlotWidget"，"GraphicsLayoutWidget" 等）
4. 在"头文件"， 输入"pyqtgraph"
5. 点击”添加“，然后点击”提升“

有关提升部件的更多信息，请参阅设计文档。”VideoSpeedTest“和”ScatterPlotSpeedTest" 示例都演示了如何使用通过pyuic5 或者 pyside-uic 将.ui 文件编译成.py 模块。“designerExample” 示例演示了如何从.ui文件动态生成python类（不需要pyuic5 或者 pyside-uic）。

### HiDPI 显示器

PyQtGraph 有一个方法mkQApp， 当与非hidpi二级显示结合时，它默认设置了我们测试过的支持hidpi显示的最佳选项组合。对于您的应用程序，您可能已经自己实例化了QApplication， 在这种情况下，我们建议在运行QApplication.exec_()之前设置这些选项。

对于Qt6 绑定，这个功能不需要设置任何属性就可以工作。

对于Qt >= 5.14 和 <6 的版本；你可以通过下面的代码获得理想的行为：

```python
os.environ["QT_ENABLE_HIGHDPI_SCALING"] = "1"
QApplication.setHighDpiScaleFactorRoundingPolicy(QtCore.Qt.HighDpiScaleFactorRoundingPolicy.PassThrough)
```

如果Qt>=5.6 或者 <5.14；你可以通过下面的代码获得接近理想的行为：

```python
QApplication.setAttribute(QtCore.Qt.AA_EnableHighDpiScaling)
QApplication.setAttribute(QtCore.Qt.AA_UseHighDpiPixmaps)
```

对于后者，理想的行为并没有实现。

```python
pyqtgraph.Qt.mkQApp(name=None) # name: (str) 应用名称，传递给Qt
```

创建一个新的应用程序或者返回当前示例（如果存在）。

### PyQt 和 PySide

PyQtGraph支持Qt库的两种流行的python包装器：PyQt和PySide。这两个包提供了几乎相同的api和功能，但由于各种原因（在其他地方讨论），您可能更喜欢使用其中一个包。当第一次导入pyqtgraph时，它通过进行一下检查自动确定使用哪个库：

1. 如果PyQt5 已经导入，使用这个
2. 如果PySide2 已经导入，使用这个
3. 如果PySide6 已经导入，使用这个
4. 如果PyQt6 已经导入，使用这个
5. 其他情况，尝试按照顺序导入PyQt5，PySide2，PySide6，PyQt6.

如果你两种库都安装了，你希望强制pyqtgraph使用其中的一个，只要确保在导入pyqtgraph之前导入它就可以：

```python
import PySide2 ## this will force pyqtgraph to use PySide2 instead of PyQt5
import pyqtgraph as pg
```

### 将PyQtGraph 作为更大项目的子包嵌入

在编写使用pyqtgraph的应用程序或者python包时，最常见的方法时在系统范围内（或者在虚拟环境中）安装pyqtgraph，并简单地从应用程序中导入pyqtgraph包。这样做的主要好处是，pyqtgraph的配置是独立于应用程序的，因此您（或者您的用户）可以自由安装pyqtgraph的新版本，而无需更改应用程序中的任何内容。这是使用python开发时的标准做法。

有时，一个特定的程序需要在开发完成后一段时间内保持工作状态。对于单一用途的科学应用程序来说，情况往往如此。如果我们想确保该软件在十年后仍然能够工作，那么最好将其绑定到pyqtgraph的一个非常特定的版本，并避免导入系统安装的版本，该版本可能更新，而且可能不兼容。当应用程序需要特定站点的修改时尤其如此。

为了支持这种独立的本地安装，pyqtgraph 中的所有内部导入都是相对的。这意味着pyqtgraph在内部从不将自己称为‘pyqtgraph'。这允许将包重命名或者作为子包使用，而不会与系统上其他版本的pyqtgraph发生任何命名冲突。

最基本的方法时把代码仓库克隆到项目工程的合适位置。当你导入pyqtgraph，确定使用完整路径以避免和系统安装的pyqtgraph冲突。例如，假设有一个简单的项目，其项目结构如下：

```python
my_project/
    __init__.py
    plotting.py
        """Plotting functions used by this package"""
        import pyqtgraph as pg
        def my_plot_function(*data):
            pg.plot(*data)
```

为了集成一个特殊版本的pyqtgraph，我们将pyqtgraph的代码从库克隆大项目里，并重新命名以区别于系统安装的pyqtgraph:

```python
my_project$ git clone https://github.com/pyqtgraph/pyqtgraph.git local_pyqtgraph
```

然后相应地调整导入声明：

```python
my_project/
    __init__.py
    local_pyqtgraph/
    plotting.py
        """Plotting functions used by this package"""
        import local_pyqtgraph.pyqtgraph as pg  # be sure to use the local subpackage
                                                # rather than any globally-installed
                                                # version.
        def my_plot_function(*data):
            pg.plot(*data)

```

使用`git checkout pyqtgraph-x.x.x`选择一个特殊版本的库，或者用`git pull`从上游拉取最新的pyqtgraph(更多信息参考git说明文档)。如果你不打算使用git的版本功能，在`git clone`命令里添加选项`--depth 1`只检索最新的版本。

对于已经使用git管理代码的项目，也可以将pyqtgraph作为git子树包含在自己的仓库里。这种方法的主要优点是，除了能够从上游仓库里拉出pyqtgraph更新外，还可以将本地pyqtgraph提交到项目存储库里，并将这些更高推入上游：

```shell
my_project$ git remote add pyqtgraph https://github.com/pyqtgraph/pyqtgraph.git
my_project$ git fetch pyqtgraph
my_project$ git merge -s ours --allow-unrelated-histories --no-commit pyqtgraph/master
my_project$ mkdir local_pyqtgraph
my_project$ git read-tree -u --prefix=local_pyqtgraph/ pyqtgraph/master
my_project$ git commit -m "Added pyqtgraph to project repository"
```

更多信息参考 `git subtree` 文档。

## 安装

PyQtGraph 最新版依赖于:

- Python 3.7+
- Qt 库，例如：PyQt5，或者PySide2
- numpy

满足这些依赖的最容易的方法时用pip或者Anaconda。

这里有许多不同的方法来安装pyqtgraph， 选择哪一种取决于你的需要。

### pip

最常见的方法是用pip安装：

```shell
$ pip install pyqtgraph 
$ pip install pyqtgraph==0.11.1 #python 3.6不支持最新版，需要指定旧的版本
```

一些用户可能需要通过pip3来安装。这个方法应该能够在所有平台上工作。

### conda

pyqtgraph 位于默认的Anaconda通道上:

```shell
$ conda install pyqtgraph
```

conda-forge 通道上同样可用：

```shell
$ conda install -c conda-forge pyqtgraph
```

### 源代码

你有三种方式获知最新的功能和修复的bug：

1. 从github上克隆pyqtgraph

   ```shell
   $ git clone https://github.com/pyqtgraph/pyqtgraph
   $ cd pyqtgraph
   ```

   现在你可以从源代码安装pyqtgraph：

   ```shell
   $ pip install .
   ```

   

2. 直接从github上安装:

   ```shell
   $ pip install git+git://github.com/pyqtgraph/pyqtgraph.git@master
   ```

   你可以改变上述命令的“master”为你更想要的分支名称或者提交码

3. 你可以简单地将pyqtgraph文件夹纺织在任何可以导入的地方，例如放在另一个项目的根目录中。PyQtGraph不需要以任何方式“构建”或者编译。

### 其他软件包

pyqtgraph的包也有几种其他形式：

- Debian， Ubuntu，和Linux：使用`apt install python-pyqtgraph`或者下载pyqtgraph页面顶端链接的.deb文件
- Arch Linux：https://www.archlinux.org/packages/community/any/python-pyqtgraph/
- Windows：下载，然后运行pyqtgraph页面顶端链接的.exe文件：[http://pyqtgraph.org](http://pyqtgraph.org/)

## Qt 速成课

PyQtGraph 广泛使用Qt来实现几乎所有的可视化输出接口。Qt的文档写的很好，我们鼓励所有的pyqtgraph开发者都要熟悉这些文档。这节的目的是向pyqtgraph的开发者们介绍如何使用Qt编程（PyQt或者PySide）。

### QWidgets 和 Layouts

一个Qt 图形界面通常由几个基本组件组成：

- 一个窗口
- 多个QWidgets实例，例如QPushButton，QLabel，QComboBox 等
- QLayouts实例（可选但是强烈建议）自动管理部件的位置，以允许GUI以可用的方式调整大小。

PyQtGraph通过提供自己的QWidgets子类来插入到GUI中来适应这种模式。

例如：

```python
from PyQt5 import QtGui  # (the example applies equally well to PySide2)
import pyqtgraph as pg

## Always start by initializing Qt (only once per application)
app = QtGui.QApplication([])

## Define a top-level widget to hold everything
w = QtGui.QWidget()

## Create some widgets to be placed inside
btn = QtGui.QPushButton('press me')
text = QtGui.QLineEdit('enter text')
listw = QtGui.QListWidget()
plot = pg.PlotWidget()

## Create a grid layout to manage the widgets size and position
layout = QtGui.QGridLayout()
w.setLayout(layout)

## Add widgets to the layout in their proper positions
layout.addWidget(btn, 0, 0)   # button goes in upper-left
layout.addWidget(text, 1, 0)   # text edit goes in middle-left
layout.addWidget(listw, 2, 0)  # list widget goes in bottom-left
layout.addWidget(plot, 0, 1, 3, 1)  # plot goes on right side, spanning 3 rows

## Display the widget as a new window
w.show()

## Start the Qt event loop
app.exec_()
```

更加复杂的界面可以使用Qt Designer进行图形化设计，它允许您简单地将小部件拖到窗口来定义其外观。

### 命名规范

实际上，pyqtgraph中的每个类都是Qt提供的基类的扩展，在阅读文档时，请记住所有Qt的类都以字母'Q'开头。在阅读任何类的方法时，查看使用了哪些Qt基类以及查看Qt文档都是很有帮助的。

大多数Qt的类定义的信号很难与常规方法分开。pyqtgraph 定义的几乎所有信号都以"sig"开头，以表明这些信号不是在Qt级定义的。

大多数情况下，以“Widget"结束的类都是QWidget的子类，因此可以作为一个GUI元素在Qt窗口中使用。类名中以”Item"结尾的类是QGraphicsItem的子类，只能在QGraphicsView实例（如GraphicsLayoutWidget或者PlotWidget）中显示。

### 信号，槽位和事件

【待续...如果你想阅读更多内容，请在pyqtgraph 论坛上发布请求】

Qt 通过执行其事件循环来检测和响应用户交互。

- 在事件循环中发生了什么？
- 什么时候需要使用QApplication.exec_()?
- 如何控制事件循环的执行？(QApplication.processEvents)

### GraphicsView 和 GraphicsItem

Qt GraphicsView 的更多信息请参考：http://qt-project.org/doc/qt-4.8/graphicsview.html

### 坐标系统和变换

Qt GraphicsView 坐标系统的更多信息参考： http://qt-project.org/doc/qt-4.8/graphicsview.html#the-graphics-view-coordinate-system

## Plotting  in pyqtgraph

在pyqtgraph中绘制数据有几种基本方法：

| [`pyqtgraph.plot()`](https://pyqtgraph.readthedocs.io/en/latest/functions.html#pyqtgraph.plot) | Create a new plot window showing your data       |
| ------------------------------------------------------------ | ------------------------------------------------ |
| `PlotWidget.plot()`                                          | Add a new set of data to an existing plot widget |
| [`PlotItem.plot()`](https://pyqtgraph.readthedocs.io/en/latest/graphicsItems/plotitem.html#pyqtgraph.PlotItem.plot) | Add a new set of data to an existing plot widget |
| [`GraphicsLayout.addPlot()`](https://pyqtgraph.readthedocs.io/en/latest/graphicsItems/graphicslayout.html#pyqtgraph.GraphicsLayout.addPlot) | Add a new plot to a grid of plots                |

这些函数都接受相同的参数来控制数据如何解释和显示：

- x - 可选X数据；如果不指定，则会自动生成一组整数
- y - Y data
- pen - 绘制线条，当禁用线条的时候赋值为None
- symbol - 描述用于每个点的符号形状的字符串。这也可以是一个字符串序列，每个点都有不同的符号。
- symbolPen - 在绘制符号轮廓时使用的笔（或笔的顺序）
- symbolBrush - 当填充符号时使用的笔刷
- fillLevel - 将曲线下的面积填入这个Y值
- brush - 当填充曲线下方时使用的刷子

这些参数的演示请参阅“绘图”示例。

上述所有函数返回所创建对象的句柄，允许进一步修改绘图和数据。

### 绘图类的组织结构

在显示绘图数据时涉及到几个类。这些类中大多数都是自动实例化的，但是了解它们是如何组织在一起的以及彼此间的关系是很有用的。PyQtGraph 很大程度上基于Qt的GraphicsView框架——如果你还不太熟悉这个框架，值得读一下（但不是必须的）。最重要的是：1）Qt GUIs 由QWidgets组成，2）一个叫做QGraphicsView的特殊小部件用于显示复杂的图形，3）QGraphicsItem 定义了再AGraphicsView中显示的对象。

- 数据类（QGrapicsItem的所有子类）
  - PlotCurveItem - 显示给定x，y数据的线
  - ScatterPlotItem - 显示给定x，y数据的点
  - PlotDataItem - 结合了PlotCurveItem和ScatterPlotItem。上面讨论的绘图函数创建这种类型的对象。
- 容器类（QGraphicsItem的子类；包含其他QGraphicsItem对象，必须在GraphicsView中查看）
  - PlotItem - 包含用于显示数据的ViewBox以及用于显示轴和标题的AxisItems和标签。这是一个QGraphicsItem子类，因此只能在GraphicsView中使用。
  - GraphicsLayout - QGraphicsItem的子类，用于显示网格项目。这通常用于显示多个PlotItems。
  - ViewBox - QGraphicsItem的子类。用户可以通过鼠标缩放或者平移ViewBox的内容。通常所有的PlotData/PlotCurve/ScatterPlotItem都在一个ViewBox中显示。
  - AxisItem - 显示坐标轴的值、刻度和标签，通常用于PlotItem。
- 容器类（QWidget的子类；可以被集成到PyQt GUI里）
  - PlotWidget - GraphicsView的子类，只显示一个PlotItem。PlotItem的大多数方法都可以通过PlotWidget使用。
  - GraphicsLayoutWidget - QWidget 的子类，显示一个GraphicsLayout。GraphicsLayout 的多数方法都可以通过GraphicsLayoutWidget使用。

![image-20210923111704892](/img/2021-09-22-pyqtgraph//image-20210923111704892.png)

### 示例

请参考pygtgraph中的“plotting" 和 ”PlotWidget"

## 显示图像和视频

PyQtGraph 将 2D numpy 数组显示为图像，并提供了用于确定如何在屏幕上转换numpy数据类型和RGB值的工具。如果你想显示来自普通图像和视频文件格式的数据，你首先需要用另一个库加载数据（PIL 对图像和内置numpy转换工作得很好）。

显示2D或者3D数据最简单的方式就是使用pyqtgraph.image()函数：

```python
import pyqtgraph as pg
pg.image(imageData)
```

这个函数接受任何浮点数或者整数数据类型，然后在一个ImageView中显示书.这个部件可以控制图像数据如何转化为32位的RGBa 值。转化过程通过两步来实现（都是可选的）：

1. 缩放和偏移数据（通过在显示的直方图上选择暗/亮级别）
2. 使用查找表将数据转换为颜色（由渐变编辑器中显示的颜色决定）

如果是3D数据（时间，x，y），时间轴将以滑块的形式显示，并可以设置当前显示的数据帧。（如果你的数据的坐标轴顺序不一样，可以用numpy.transpose来重新排列）。

这里还有一些显示图像的其他方法：

- ImageView 可以直接实例化，然后嵌入到Qt应用程序中
- ImageItem 实例可以在ViewBox或者GraphicsView中使用
- 使用RawImageWidget 实现更高的新能

这些类中的任何一个都可以通过调用setImage（）来显示新帧来显示视频。

有关更多信息，请参阅上面列出的类和“VideoSpeedTest"，”ImageItem", "ImageView" 和 “HistogramLUT" 示例。

## 3D 绘图

PyQtGraph使用OpenGL提供一个3D场景系统。这个系统是功能性的，但仍然处于早期开发阶段。目前的功能包括：

- 带有缩放/旋转控件（鼠标拖动和滚轮）的3D视图控件
- 场景允许项目添加/删除每个项目的转换和父/子关系
- 三角网格
- 基本的网格计算函数：等值面、每个顶点的法线
- 体积渲染项目
- 网格/轴项目

更多信息，请参阅API手册和Volumetric (GLVolumeItem.py)和 Isosurface(GLMeshItem.py)示例

基本用法示例：

```python
## build a QApplication before building other widgets
import pyqtgraph as pg
pg.mkQApp()

## make a widget for displaying 3D objects
import pyqtgraph.opengl as gl
view = gl.GLViewWidget()
view.show()

## create three grids, add each to the view
xgrid = gl.GLGridItem()
ygrid = gl.GLGridItem()
zgrid = gl.GLGridItem()
view.addItem(xgrid)
view.addItem(ygrid)
view.addItem(zgrid)

## rotate x and y grids to face the correct direction
xgrid.rotate(90, 0, 1, 0)
ygrid.rotate(90, 1, 0, 0)

## scale each grid differently
xgrid.scale(0.2, 0.1, 0.1)
ygrid.scale(0.2, 0.1, 0.1)
zgrid.scale(0.1, 0.2, 0.1)
```

## 线条、填充和颜色

Qt依赖QColor,QPen 和 QBrush 类实现特定的线条和填充风格。pyqtgraph使用相同的内部机制，同时允许许多简写方法来指定相同的风格选项。

pyqtgraph 的许多函数和方法都接受指定线条风格（pen）、填充风格（brush）或者颜色的参数。对于大部分函数参数，可能会用到的有如下值：

- 表示颜色的单字符（b，g，r，c，m，y，k，w）
- （r，g，b）或者（r，g，b，a）元组
- 单灰度值（0.0 - 1.0）
- （index，maxinum）元组用于自动遍历颜色（参见intColor）
- QColor
- QPen / QBrush 

值得注意的是，使用mkPen()/mkBrush()函数或者Qt的QPen和QBrush类可以很容易地构建更复杂的笔和笔刷

```
mkPen('y', width=3, style=QtCore.Qt.DashLine)          ## Make a dashed yellow line 2px wide
mkPen(0.5)                                             ## solid grey line 1px wide
mkPen(color=(200, 200, 255), style=QtCore.Qt.DotLine)  ## Dotted pale-blue line
```

### 默认背景和前景颜色

默认情况下，pyqtgraph的绘图使用黑色背景，坐标轴、文本和绘图线使用灰色背景。这些默认设置可以通过pyqtgraph.setConfigOption() 修改：

```python
import pyqtgraph as pg

## Switch to using white background and black foreground
pg.setConfigOption('background', 'w')
pg.setConfigOption('foreground', 'k')

## The following plot has inverted colors
pg.plot([1,4,2,3,5])

```

（注意，必须在创建任何控件之前设置这些参数）

## 交互式数据选择控件

PyQtGraph 包含可以让用户选择和标记数据区域的图形项目。

### 线性选择与标记

两个允许标记和选择1维数据的类：LinearRegionItem 和 InfiniteLine。LinearRegionItem 可以被添加到ViewBox或者PlotItem中来标记一个水平或者垂直的区域。这个区域可以被拖动，它的边界可以被独立的移动。第二个类，InfiniteLine， 常常被用于标记沿着x或者y轴的特殊的位置。这些可能会被用户拖动。

### 2D 选择与标记

pyqtgraph 通过ROI类或者任何它的子类来选择一个图像的2D区域。默认情况下，ROI 仅仅展示一个矩形区域，用户可以移动这个矩形区域来标记一个特殊的区域（通常情况下这个区域是一个图像的区域，但这不是必须的）。有几个添加句柄的方法可以调整ROI的大小或者旋转（addScaleHandle， addRotateHandle， 等）。这些句柄可以放置在相对ROI的任何位置，并可以围绕任意中心点缩放/旋转ROI。有几个具有各种形状和交互模式的ROI子类。

可以使用 ROI.getArrayRegion 通过ROI 和 一个 ImageItem 自动提取一个图像的区域。ROI 类通过affineSlice 实现这个提取过程。

ROI 也可以用于控制场景中的移动/旋转/缩放项目，类似于大多数矢量图形编辑应用程序。

有关更多信息，参阅ROITypes 示例。

## 导出

PyQtGraph 提供了各种2D图形的导出格式。对于 3D 图形，参见下文的”3D图形导出“。

### 从GUI 导出

任何2D 图形都可以通过右键点击图形，然后选择”export“ 来导出。这将显示一个对话框，其中用户必须：

1. 选择一个项目（或者整个场景）来导出。选中的项目会在右边的图形窗口中高亮显示（但这种高亮不会在导出的文件中显示）。
2. 选择1个导出格式
3. 修改任何满意的导出选项
4. 点击”export“按钮

### 导出格式

- Image - PNG 是默认的格式。具体的图像格式区域与你的QT库。但是，常见的格式例如PNG, JPG和TIFF等都是支持的。
- SVG - 导出SVG格式一般是为了能在Inkscape 和 Adobe Illustrator 中也能很好的工作.为了得到高质量的导出文件,请使用PyQtGraph 0.9.3 以上的版本。这也是生成出版物质量的图像的好方法。
- CSV - 只能导出PlotItem 里的绘图数据
- Matplotlib - 此导出器将打开一个新窗口，并尝试使用matplotlib(如果可用）重新绘制数据。请注意，有些图形特性要么在此导出器中没有实现，要么在matplotlib中不可用。这个导出器只能导出PlotItem中的数据。
- Printer - 导出到操作系统的打印服务。这个导出器是为了完整性而提供的，但是由于Qt打印系统的问题，它并没有得到很好的支持。
- HDF5 - 如果安装了H5PY， 那么可以支持从PlotItem导出数据为H5PY文件。该导出器支持包含多条曲线的PlotItem对象，基于columnMode参数将数据堆叠到单个HDF5数据集。如果数据项大小不同，则每个数据项都有自己的数据集。

### 通过API导出

通过编程导出数据的代码如下：

```python
import pyqtgraph as pg
import pyqtgraph.exporters

# generate something to export
plt = pg.plot([1,5,2,4,3])

# create an exporter instance, as an argument give it
# the item you wish to export
exporter = pg.exporters.ImageExporter(plt.plotItem)

# set export parameters if needed
exporter.parameters()['width'] = 100   # (note this also affects height parameter)

# save to file
exporter.export('fileName.png')
```

如果要导出整个 GraphicsLayoutWidget  grl布局，需要用如下的方式初始化导出器：

```python
exporter = pg.exporters.ImageExporter( grl.scene() )
```

### 导出3D图形

上面描述的导出功能还不适用于3D图形。但是，可以使用QGLWidget.grabFrameBuffer 或者 QGLWidget.renderPixmap来生成图像。

```python
glview.grabFrameBuffer().save("fileName.png")
```

更多信息参考Qt文档

# unnitest 中文文档 （Unit testing framework）
***

unittest 框架中文文档，起因是看到官方文档有English、French、Japanese、Korean，唯独没有Chinese，这就很不爽了。  

🇨🇳  
本文：尊重原创，翻译遵循信达雅。   
🇨🇳   

Python版本：3.7.1

[unittest官方英文文档](https://docs.python.org/3/library/unittest.html#test-discovery)
***

# unnitest--单元测试框架
## Source code：[Lib/unittest/__init__.py](https://github.com/python/cpython/blob/3.7/Lib/unittest/__init__.py)  

(亲爱的，如果你对测试概念已经有所了解，请直接跳到 [断言方法列表](https://docs.python.org/3/library/unittest.html#assert-methods)。🌹)

unittest 单元框架最初的灵感来自于JUnit，它和其他语言中的单元测试框架有着相似的韵味。例如它支持自动化测试、共享测试的设置和关闭代码、将测试聚合到集合中，以及测试与报告框架的独立性。  

为此，unittest以面向对象的方式支持如下概念：

**测试夹：**

```
注：测试夹（Fixture） 指的是测试中依赖的数据和条件等等，unittest提供对Fixture的支持。
```
测试夹是指执行一个或多个测试任务所需要的准备工作，以及任何相关联的清理操作。这可能包括，例如，创建临时或代理数据库、创建目录或启动服务器进程。

**测试用例：**

一个测试用例是一个独立的单元测试，它检查对特定输入集的特定响应。unittest提供了一个基类（[TestCase](https://docs.python.org/3/library/unittest.html#unittest.TestCase)），能够用他去创建新的测试用例。

**测试套件：**  

测试套件是测试用例或者测试套件本身的集合，也可以是测试用例和测试套件的混合集合，它把要执行的测试汇聚起来。  

**测试运行器**  

测试运行器是用于编排测试用例，以及向用户展示测试结果。运行器可以使用图形界面、文本界面，或者是返回特定的值去展示执行后的测试结果。

```
请参阅：
doctest模块(https://docs.python.org/3/library/doctest.html#module-doctest)：
  另外一种具有独特韵味的测试支持模块。

Simple Smalltalk Testing: With Patterns（https://web.archive.org/web/20150315073817/http://www.xprogramming.com/testfram.htm）
  Kent Beck 的原论文，运用模式在测试框架上，被unittest共享。

Nose and py.test
  使用轻量级的语法来编写测试用例的第三方单元测试框架。
  例如：assert func(10) == 42

The Python Testing Tools Taxonomy（https://wiki.python.org/moin/PythonTestingToolsTaxonomy）
  一张巨大的Python测试工具列表，包括功能测试框架以及模拟对象库。

Python源代码 Tools/unittestgui/unittestgui.py 中的脚本是一个图像界面工具，用于测试调试和执行。这很大程度上是为了方便新手用户写测试用例。
对于生产环境，建议测试用例运行在持续集成系统，例如 Jenkins、Buidlbot、Hudson。

```

## 基本例子

unittest模块为用户构建和执行测试用例提供了一组丰富的工具。本节演示一小部分工具，足以满足大部分用户的需求。

以下简短的脚本，用于测试字符串的三个方法：

```python
import unittest

class TestStringMethods(unittest.TestCase):

    def test_upper(self):
        self.assertEqual('foo'.upper(), 'FOO')

    def test_isupper(self):
        self.assertTrue('FOO'.isupper())
        self.assertFalse('Foo'.isupper())

    def test_split(self):
        s = 'hello world'
        self.assertEqual(s.split(), ['hello', 'world'])
        # check that s.split fails when the separator is not a string
        with self.assertRaises(TypeError):
            s.split(2)

if __name__ == '__main__':
    unittest.main()

```
testcase是通过子类化unittesttest.testcase创建的。上面代码中的三个独立测试用例，用例名字分别都是以test开头定义的，这样的命名方式告诉运行器哪些方法表示测试。

每次测试的关键是调用assertEqual()去校验预期结果；调用assertTrue()和assertFalse()验证条件是否成立；或者是调用assertRaises()验证是否引发特定异常。以上这些方法被用来替代assert语句，因此运行器可以收集所有的结果并生成报告。

setUp()和tearDown()方法允许您定义将在每个测试方法前后执行的指令。在[组织测试代码的部分](https://docs.python.org/3/library/unittest.html#organizing-tests)中会更详细地介绍它们。

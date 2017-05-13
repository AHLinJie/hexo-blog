---
layout: post
title: Pyton Unittest 小记
category: python
tag: test
date: 2017-02-01
---
**摘要:**
个人觉得测试在项目的进行中是很重要的.
好的测试代码可以减少上线前的bug数量,提高项目的稳定性.
还可以给持续集成作为铺垫;同样一份好的测试代码还可以当做一份好的文档来使用;
在测试代码上面花去的时间是有价值,并且不是单方面的测试价值,还有整个项目整体价值的提升.
大量的黑盒测试不单消耗人力物力还很浪费时间且没有重复利用的空间.


#### test case
通过继承unittest的TestCase可以实现一个测试或者一组测试代码
测试方法必须是test开头.
```
import unittest

class TestExample(unittest.TestCase):
    def test_one_is_one(self):
        self.assertEqual(1, 1)

    def test_two_is_two(self):
        self.assertEqual(2, 1)

if __name__ == '__main__':
    unittest.main()

```

#### 命令行执行test

`python -m unittest -h`

其中可以制定测试的模块,指定测试的方法和测试类.


#### test fixtures
固定装置的测试,防止重复代码,准备测试环境,测试完成清理.

```
import unittest

class TestExample(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        cls.database = 'mysql'

    def test_db(self):
        self.assertEqual(self.database, 'mysql')


    @classmethod
    def tearDownClass(cls):
        cls.clean = True

if __name__ == '__main__':
    unittest.main()
```

#### test suite 
测试套件, 用来组装你需要的测试代码,有些情况并不是所有的测试代码都要执行.
可以使用test suite来处理.

```
# tt is a module, test_upper is a test case func

suite = unittest.TestSuite(map(tt.TestString, ['test_upper']))
suite.addTest(tt.TestString.test_upper)
# 可以使用addTest往测试套件中添加测试方法
suite = unittest.TestLoader().loadTestsFromTestCase(tt.TestString)
# 使用TestLoader实例化后方法loadTestsFromTestCase来将指定的
# testcase类中方法全部导入
```

#### 跳过测试

```
@unittest.skipIf(1==1, '1 == 1') 
def test_skipif(self):
    print 'if has skip decorator this not be print'
```


#### runner
实例化runner运行测试组件
```
runner = unittest.runner.TextTestRunner()
runner.run(suite)
```

#### test discover
测试方法的发现.
可以通过discover发现测试的项目中的testcase, 指定测试代码目录.
如果存在多级包目录的话建议添加top_level_dir,注意一定要有"__init__.py"的才可以发现
的testcase.
```
test_dir = 'path_to_pro'
suite = unittest.defaultTestLoader.discover(test_dir, top_level_dir='path_to_top')

```



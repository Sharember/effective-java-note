# 13:使类和成员的可访问性最小化

## 1. 简介

要区别设计良好的模块与设计不好的模块，最重要的因素在于，这个模块对于外部的其他模块而言，是否隐藏其内部数据和其他实现细节。

设计良好的模块会隐藏所有的实现细节，把它的 API 与它的实现清晰的隔离开来。然后模块之间只通过它们的 API 进行通信，一个模块不需要知道其他模块的内部工作情况。

这个概念被称为信息隐藏。

## 2. 尽可能的使每个类或者成员不被外界访问

- 对于顶层的类和接口，只有两种肯的访问级别：包私有的和公有的。

- 如果一个包级私有的顶层类只是在某一个类的内部被用到，就应该考虑使它成为唯一使用它的那个类的私有嵌套类。

- 不能为了测试而将类、接口或者成员变成包的导出的 API 的一部分。当然，一般以没必要这么做，因为可以让测试作为被测试的包的一部分来运行。

- 实例域绝对不能是公有的。静态域也一样。

> 长度非零的数组总是可变的，所以，类具有公有的静态 final 数组域，或者返回这种数组域的方法，这几乎总是错误的。
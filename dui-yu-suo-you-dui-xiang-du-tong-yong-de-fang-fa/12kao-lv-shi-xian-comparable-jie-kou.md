# 12:考虑实现Comparable接口

## 简介

一旦实现了 Comparable 接口，他就可以和许多反省算法以及依赖于该接口的集合实现进行协作。

java 平台类库中的所有值类都实现了 Comparable 接口。如果你正在编写一个值类，它具有非常明显的内在排序关系，比如按字母排序、按数值顺序或者时间排序，那你就应该坚决考虑实现这个接口。

## 几个建议与约定

- 1.实现者必须确保所有的 x 和 y 都满足 sgn(x.compareTo(y)) == -sgn(y.compareTo(x))。

- 2.实现者还必须保证这个比较关系是可传递的：x.compareTo(y) > 0 && y.compareTo(z) > 0 => x.compareTo(z) > 0。

- 3.实现者必须保证 x.compareTo(y) == 0 暗示着所有的 z 都满足 sgn(x.compareTo(z)) == sgn(y.compareTo(z))

**强烈建议：**

(x.compareTo(y) == 0) == (x.equals(y))，但是这并非绝对必要。一般来说，如果违反了这个条件，都应该予以说明。推荐使用这样的说法：”注意，该类具有内在的排序功能，但是与 equals 不一致“。




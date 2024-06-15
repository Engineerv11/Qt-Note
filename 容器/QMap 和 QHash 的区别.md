QMap 和 QHash 的区别
=======

是否有序：QMap有序，QHash无序

底层数据结构：QMap底层是红黑树，QHash底层是哈希表

查找速度：QHash的查找速度明显快于QMap

存储空间：QHash占用的存储空间明显多于QMap，QHash的速度是利用空间换来的



Key类型必须重载的函数：

- QMap的Key必须重载<运算符
- QHash的Key必须重载==运算符
- QHash的Key类型必须重载全局的qHash函数






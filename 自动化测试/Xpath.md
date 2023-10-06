## Xpath

### 简介

![image-20231006230726235](https://gitee.com/DiaoYangcao/md/raw/master/images/image-20231006230726235.png)

我们可以使用Xpath路径表达式来定位html网页的某一个元素

### 节点关系

**父节点**

在book包含里面的同一级别的**元素**都是认这个book是爹所谓父节点

```xml
<book>
  <title>Harry Potter</title>
  <author>J K. Rowling</author>
  <year>2005</year>
  <price>29.99</price>
</book>
```

**子节点**

反之book包含中的同一级别的元素下是book节点的儿子节点

```xml
<book>
  <title>Harry Potter</title>
  <author>J K. Rowling</author>
  <year>2005</year>
  <price>29.99</price>
</book>
```

**同胞节点**

在title一起的**同一级别**称为兄弟节点

```xml
<book>
  <title>Harry Potter</title>
  <author>J K. Rowling</author>
  <year>2005</year>
  <price>29.99</price>
</book>
```

**先辈**

在title级别眼里来看他的父节点<book>,<bookstore>都可以称为先辈节点

```xml
<bookstore>
<book>
  <title>Harry Potter</title>
  <author>J K. Rowling</author>
  <year>2005</year>
  <price>29.99</price>
</book>
</bookstore>
```

**后代**

在<bookstore>节点下的子<book>以及孙<title>都是后代节点

```xml
<bookstore>
<book>
  <title>Harry Potter</title>
  <author>J K. Rowling</author>
  <year>2005</year>
  <price>29.99</price>
</book>
</bookstore>
```

---



### Xpath运算符

**实例**

![image-20231006231916127](https://gitee.com/DiaoYangcao/md/raw/master/images/image-20231006231916127.png)

---

**选取节点**

| 表达式   | 描述                                             |
| :------- | :----------------------------------------------- |
| nodename | 选取此节点的所以子节点                           |
| /        | 从根节点选取(取子节点)                           |
| //       | 从匹配选择的当前节点选择文档中的节点，不考虑位置 |
| .        | 选取当前节点                                     |
| ..       | 选取当前上一个节点                               |
| @        | 选取属性                                         |

---



**举例**：

| 路径表达式      | 结果                                    |
| --------------- | --------------------------------------- |
| bookstore       | 选取bookstore元素下所有节点             |
| /bookstore      | 选取bookstore                           |
| bookstore/book  | 选取bookstore所属所以book节点           |
| //book          | 选取所有book子元素，而不管在任何位置    |
| bookstore//book | 选取属于bookstore元素的后代所有book元素 |
| //@lang         | 选取名为lang的所有属性                  |

---



### 谓语

| 路径表达式                          | 结果                                                         |
| :---------------------------------- | :----------------------------------------------------------- |
| /bookstore/book[1]                  | 选取属于 bookstore 子元素的第一个 book 元素。                |
| /bookstore/book[last()]             | 选取属于 bookstore 子元素的最后一个 book 元素。              |
| /bookstore/book[last()-1]           | 选取属于 bookstore 子元素的倒数第二个 book 元素。            |
| /bookstore/book[position()<3]       | 选取最前面的两个属于 bookstore 元素的子元素的 book 元素。    |
| //title[@lang]                      | 选取所有拥有名为 lang 的属性的 title 元素。                  |
| //title[@lang='eng']                | 选取所有 title 元素，且这些元素拥有值为 eng 的 lang 属性。   |
| /bookstore/book[price>35.00]        | 选取 bookstore 元素的所有 book 元素，且其中的 price 元素的值须大于 35.00。 |
| /bookstore/book[price>35.00]//title | 选取 bookstore 元素中的 book 元素的所有 title 元素，且其中的 price 元素的值须大于 35.00。 |



**选取未知节点**

| 通配符 | 描述                 |
| :----- | :------------------- |
| *      | 匹配任何元素节点。   |
| @*     | 匹配任何属性节点。   |
| node() | 匹配任何类型的节点。 |

举例

| 路径表达式   | 结果                              |
| :----------- | :-------------------------------- |
| /bookstore/* | 选取 bookstore 元素的所有子元素。 |
| //*          | 选取文档中的所有元素。            |
| //title[@*]  | 选取所有带有属性的 title 元素。   |
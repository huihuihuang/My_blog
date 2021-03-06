# 字符串匹配算法BF和RK算法

*本文是学习极客时间王争《数据结构与算法之美》的个人总结，图片均来源于其中。*

*参考地址：https://time.geekbang.org/column/article/71187*

BF算法和RK算法都是但模式匹配算法，也就是一个串跟一个串进行匹配。

## BF算法

BF(Brute Force)算法，即暴力匹配算法，也叫朴素匹配算法。

如果在字符串A中查找字符串B，那么字符串A就是主串，字符串B就是模式串。把主串得长度记为n，模式串得长度记为m，n>=m。

BF算法得思想是：**我们在主串中，检查起始位置分别为0，1，2... n-m，且长度为m得n-m+1个子串，看有没有跟模式串匹配的。**

看下面的图片示意：

![1](D:\MyBlog\1.png)

从上面的算法思想和例子可以看出，在极端情况下，我们每次要对比m个字符，要对比n-m+1次，所以这种算法的最坏时间复杂度为O(n*m)。

虽然理论上BF算法的时间复杂度很高，但是在实际开发中是一个比较常用的字符串匹配算法，原因如下：

* 第一，实际的软件开发中，大多情况模式串和主串的长度不会太长。而且每次模式串于主串的子串匹配的时候，中途遇到不匹配的字符时就停止，并不需要把m个字符都比较一下。所以，大部分情况下算法的执行效率比较高。
* 第二，BF算法思想简单，实现容易，不容易出错，如果有bug也容易暴露和修复，所以在满足性能要求的前提下，简单是首选，即KISS（Keep it Simple   and Stupid）设计原则。

## RK算法

RK算法全称为Rabin-Karp算法，即由Rabin和Karp两个人发明的。RK算法可以理解为BF算法的升级版。

在BF算法中，每次需要暴力的对比n-m+1个子串和模式串，每次最多需要对比m个字符，实际复杂度比较高。我们对其稍微进行改进，引入哈希算法，来降低时间复杂度。

RK算法思想：**我们通过哈希算法对主串中的n-m+1个子串分别求哈希值，然后逐个与模式串的哈希值比较大小。如果某个子串的哈希值与模式串相等，那就说明对应的子串和模式串匹配了。**

因为哈希值是一个数字，数字之间比较是否相等是非常快速的。不过在此之前我们还要遍历子串中的每个字符，来计算子串的哈希值。模式串和子串的比较效率提高了，但是算法整体效率没有提高。所以还要想办法提高计算子串哈希值的效率。

我们假设要匹配的字符集中只包含K个字符，然后用一个K进制数来表示一个子串，这个K进制数转化为十进制数，作为子串的哈希值。可以看下图中的例子：

“657”看作十进制表示，“cba"看作26禁止表示，即只算小写字母有26个字符。

<img src="D:\MyBlog\2.png" alt="image-20200218104238474" style="zoom:67%;" />

下面以字符串中只包含a-z这26个小写字符为例，在哈希值的计算中有一个特点，相邻两个子串s[i-1]和s[i] (i表示子串在主串中的起始位置，子串长度均为m)，对应的哈希值计算公式有交集。由公式表示如下图：

![image-20200218104649378](D:\MyBlog\3.png)

此外，在计算时为了提高效率，可以使用将26^(m-1)这部分计算先计算好存起来，后面使用时直接查表获取。

**RK算法的时间复杂度**：

RK算法包含两部分，计算子串哈希值和模式串哈希值与子串哈希值的比较。第一部分，可以设计特殊的哈希算法，只需要扫描一次主串便可以求出所有子串的哈希值。时间复杂度为O(n)；第二部分哈希值直接的比较时间复杂度为O(1)，总共需要比较n-m+1个子串，所以时间复杂度也为O(n)。所以整体的时间复杂度为**O(n)**。

其他可能遇到的问题：

如果模式串很长，相应的主串中的子串也会很长，通过上面的哈希算法计算的哈希值就会特别大，如果**超过了计算机中的整形数据可以表示范围**，如何解决？

刚才设计的哈希算法是没有散列冲突的，一个字符串与一个数值对应。但是为了能够将哈希值落在整型数据范围内，可以允许哈希冲突。我们可以更改哈希算法，a-z这26个字母每个对应一个数字，然后将字符串变成将每个字符对应的数字相加，最后的和作为哈希值，这个值就会相对小很多。或者将每一个字母从小到达对应一个素数，这样的冲突概率会降低一些。

现在新的问题时，**如何解决冲突**，方法很简单，当我们发现一个子串的哈希值跟模式串的哈希值相等时，在对比一下子串和模式串本身就好了。这里也要控制哈希算法冲突概率相对低一些，如果存在大量冲突，RK算法的时间复杂度也会退化，效率降低，极端情况下将会退化为O(n*m)。

思考：在RK算法指向时，可以边计算主串中的子串，边与模式串进行对比，如果哈希值相同，即匹配成功了，那么主串后面的子串就可以不用计算了，直接退出。




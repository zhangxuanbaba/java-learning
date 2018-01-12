## 1.String对象
String对象是java中重要的数据类型，在大部分情况下我们都会用到String对象。其实在Java语言中，其设计者也对String做了大量的优化工作，这些也是String对象的特点，它们就是:不变性，常量池优化和String类的final定义。

### 1.1 不变性
String对象的状态在其被创建之后就不在发生变化。为什么说这点也是Java设计者所做的优化，在java模式中，有一种模式叫不变模式，了解的童鞋也应该知道不变模式的作用：在一个对象被多线程共享，而且被频繁的访问时，可以省略同步和锁的时间，从而提高性能。而String的不变性，可泛化为不变模式。

### 1.2 常量池优化
常量池优化指的是什么呢？那就是当两个String对象拥有同一个值的时候，他们都只是引用了常量池中的同一个拷贝。所以当程序中某个字符串频繁出现时，这个优化技术就可以节省大幅度的内存空间了。例如：

String s1  = "123";

String s2  = "123";

String s3 = new String("123"); System.out.println(s1==s2); //true

System.out.println(s1==s3);  //false

System.out.println(s1==s3.intern());  //true



以上代码中，s1和s2引用的是相同的地址，故而第四行打印出的结果是true;而s3虽然只

与s1,s2相等，但是s3时通过new String(“123”)创建的，重新开辟了内存空间，因引用的

地址不同，所以第5行打印出false;intern方法返回的是String对象在常亮池中的引用，所以

最后一行打印出true。



### 1.3 final的定义



String类以final进行了修饰，在系统中就不可能有String的子类，这一点也是出于对系统安全性的考虑。

## 2、字符串操作中的常见优化方法

### 2.1 split()方法优化
通常情况下，split()方法带给我们很大的方便，但是其性能不是很好。建议结合使用

indexOf()和subString()方法进行自定义拆分，这样性能会有显著的提高。　　　　



### 2.2 String常亮的累加操作优化方法

示例代码:<br>

![Image text](http://mmbiz.qpic.cn/mmbiz_jpg/UtWdDgynLdb7ANibg0YcEadOS1BIGCUn0LURsMTNE1dLN2y4ymSZrsMibXwaapGVxJuCwiaibrAJibRMVBLtxdAdYKQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

结果：<br>

![Image text](http://mmbiz.qpic.cn/mmbiz_png/UtWdDgynLdb7ANibg0YcEadOS1BIGCUn0aKSDdH9r7FuktrzecTL3tXXwElYIC1SVlHNMh38hkjG13k18A2L8FQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

上例所示，使用+号拼接字符串，其效率明显较低，而使用StringBuffer和StringBuilder的

append()方法进行拼接，效率是使用+号拼接方式的百倍甚至千倍，而StringBuffer的效率

比StringBuilder低些，这是由于StringBuffer实现了线程安全，效率较低也是不可避免的。

所以在字符串的累加操作中，建议结合线程问题选择，应避免使用+号拼接字符串。


### 2.3 StringBuffer和StringBuilder的选择
上例中也使用过StringBuffer和StringBuilder了，两者只有线程安全方面的差别，所以呢，在无需考虑线程安全的情况下，建议使用性能相对较高的StringBuilder类，若系统要求线程安全，就选择StringBuffer类。

### 2.4 基本数据类型转化为String类型的优化方案
示例代码:<br>
![Image text](http://mmbiz.qpic.cn/mmbiz_jpg/UtWdDgynLdb7ANibg0YcEadOS1BIGCUn0VzraeSWKNwW6ttxjvlAllU2t2GLQiawfNtxgsicLH0OhY49X91HcyMhg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

以上示例中，String.valueOf()直接调用了底层的Integer.toString()方法，不过其中会先判空；+”“由StringBuilder实现，先调用了append()方法，然后调用了toString()方法获取字符串；num.toString()直接调用了Integer.toString()方法，所以效率是:num.toString()方法最快，其次是String.valueOf(num)，最后是num+”“的方式。以下是结果截图:

![Image text](http://mmbiz.qpic.cn/mmbiz_png/UtWdDgynLdb7ANibg0YcEadOS1BIGCUn0kWMMhRwCt4qflLT2lECewraW0IuAGxZbvUhv6pp8e4vlK7wPiaicdNDA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

版权声明：本文为本人原创和部分网上收集，对于网上收集部分除非确实无法确认，我都会注明作者和来源。部分文章推送时未能与原作者取得联系。若涉及版权问题，烦请原作者联系我，我会在24小时内删除处理，谢谢！^_^

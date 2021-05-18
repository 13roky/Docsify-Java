# Java常用类


## 1. String类

###  1.1 String的特性

- **String类：**代表字符串。Java程序中的所有字符串字面值（如“abc”）都作为此类的实例实现。

- String是一个final类，代表==不可变的字符序列==。

- String字符串是常量，用双引号引起来表示。他们的值在创建之后不能更改。

- String对象的找字符内容是存储在一个字符数组value[]中的。（jdk新版本已改为使用byte类型的数组value[]存放）

![](https://i.vgy.me/NnITJv.png)

### 1.2 String字面量赋值的内存理解

字面量赋值是直接在常量池中赋值的

**Demo:**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

/**
 * String 的使用
 *
 * @author 13roky
 * @date 2021-04-24 10:34
 */
public class StringTest {
    /*
        String:字符串,使用一对""来表示.
        1.String类是被声明为final的,不可被继承.
        2.String实现了Serializable接口:标识字符串是支持序列化的. (io流)
                实现了Comparable接口:可以比较大小.
        3.String内部定义了final char[] value用于存储字符串数字. final表示数组和其元素不能被修改。(为了节省jvm的内存空间jdk9已经改为byte[]类型数组)
        4.String:代表不可变的字符序列。简称：不可变性。
                体现:1.当对字符串重新赋值时,需要重新指定内存区域赋值,不能使用原有的value进行赋值.(因为原有的value是final的)
                    2.当对现有的字符串进行连接操作时,需要重新指定内存区域赋值,不能使用原有的value赋值.
                    3.当调用String的replace()方法修改字符或字符串时,也必须重新指定内存区域赋值,不能使用原有的value赋值.
        5.通过字面量的方式(区别于new)给一个字符串赋值,此时的字符串值生命在字符串常量池中.
        6.字符串常量池中是不会存储相同内容的字符串的.
     */
    @Test
    public void test01(){
        //字面量的定义方式, 在内存中用的是同一个内存地址
        String s1 = "abc";
        String s2 = "abc";
        //==比较的是地址值,为true说明s1和s2在内存中指向的是同一个位置
        System.out.println(s1 == s2);//true
        
        s1 = "hello";
        
        System.out.println(s1);//hello
        System.out.println(s2);//abc

        System.out.println("================================================");

        String s3 = "abc";
        s3 += "def";
        System.out.println(s3);//abcdef
        System.out.println(s2);//abc

        System.out.println("================================================");

        String s4 = "adb";
        String s5 = s4.replace('a','m');

        System.out.println(s4);//abc
        System.out.println(s5);//mbc
    }
}

```

**图解:**

- 由于==字符串常量池中是不会存储相同内容的字符串的==，所以在字符串常量池中s1和s2指向同一个内存地址。

![](https://i.vgy.me/pgskqy.png)

- 由于String内部定义了final char[] value用于存储字符串数字，final表示数组和其元素不能被修改，其也就有了==不可变的字符序列==的性质。所以改变s1取值为hello后，并不会改变字符串常量池中的对应位置的值，而是会新开辟一个内存地址存放hello值，并且s1指向新的内存地址。

![](https://i.vgy.me/pHgWhR.png)

- 以下图解类似。

![](C:\Users\boqia\AppData\Roaming\Typora\typora-user-images\image-20210425100846938.png)

![](https://i.vgy.me/XHCsa9.png)

### 1.3 String new方式赋值的内存理解

**Demo:**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

/**
 * String 的使用
 *
 * @author 13roky
 * @date 2021-04-24 10:34
 */
public class StringTest {
    /*
    String实例化方式测试:
    方式一: 通过字面量定义的方式
    方式二: 通过new + 构造器的方式

    面试题：String s = new String("abc);方式创建对象，在内存中创建了几个对象？
            两个：一个是堆空间中new结构，另一个是char[]对应的常量池中的数据"abc"
     */
    @Test
    public void test2() {
        //通过字面量定义的方式:此时的s1和s2的数据javaEE生命在方法区中的字符串常量池中.
        String s1 = "javaEE";
        String s2 = "javaEE";
        //通过new + 构造器的方式:此时的s3和s4保存的地址值是数据在堆空间中开辟空间后对应的地址值.
        String s3 = new String("javaEE");
        String s4 = new String("javaEE");

        System.out.println(s1 == s2);//true
        System.out.println(s1 == s3);//false
        System.out.println(s1 == s4);//false
        System.out.println(s3 == s4);//false
        System.out.println(s3.equals(s4));//true

        System.out.println("=================================================");

        Person p1 = new Person("Tom",12);
        Person p2 = new Person("Tom",12);

        System.out.println(p1.name.equals(p2.name));//true
        System.out.println(p1.name == p2.name);//true
    }
}

class Person{
    public String name;
    public int age;

    public Person(String name,int age) {
        this.name = name;
        this.age = age;
    }
}

```

**图解:**

new的结构是存在于堆中的，`比如 String s3 = new String("javaEE");`

![](https://i.vgy.me/FVZVIz.png)

### 1.4 String 拼接字面量和变量的方式赋值

**Demo：**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

/**
 * String 的使用
 *
 * @author 13roky
 * @date 2021-04-24 10:34
 */
public class StringTest {
    /*
    1.常量与常量的拼接结果在常量池。且常量池中不会存在享同内容的常量。
    2.只要其中有一个是变量，结果就在堆中。
    3.如果拼接的结果调用intern()方法，返回值就会在常量池中。
     */
    @Test
    public void test03() {
        String s1 = "javaEE";
        String s2 = "hadoop";
        String s3 = "javaEEhadoop";
        String s4 = "javaEE" + "hadoop";//引号中的为字面量，这里是字面量的拼接
        String s5 = s1 + "hadoop";
        String s6 = "javaEE" + s2;
        String s7 = s1 + s2;
        final String s8 = "hadoop";
        String s9 = "javaEE" + s8;

        System.out.println(s3 == s4);//true
        System.out.println(s3 == s5);//false
        System.out.println(s3 == s6);//false
        System.out.println(s3 == s7);//false
        System.out.println(s5 == s6);//false
        System.out.println(s5 == s7);//false
        System.out.println(s6 == s7);//false
        System.out.println(s3 == s9);//true

        String s10 = s5.intern();//返回值得到的s8使用的常量值中已经存在的”javaEEhadoop“（s5.intern返回的时常量池中对应的内存地址）
        System.out.println(s3 == s10);//true
    }    
}
```

**图解：**

- 常量与常量的拼接，结果直接保存在常量池中。如`String s4 = "javaEE" + "hadoop";`,如果常量池中存在“javaEEhadoop”，那么s4直接指向其地址。
- 只要拼接赋值时，其中有一个是变量，那么结果就会存在于堆中，如`String s5 = s1 + "hadoop";`,栈中的变量名s5指向堆中对应的地址0x0001，堆中的地址又指向常量池的地址0x1214。
- s5指向的是堆中的内存地址0x0001，但是方法`s5.intern`返回的直接是常量池中的地址。所以`String s10 = s5.intern();`这行代码会让s10直接指向常量池对应的内存地址。

![](https://i.vgy.me/WxrzkO.png)

```java
package com.broky.commonClass.exer;

import java.util.Arrays;

/**
 * @author 13roky
 * @date 2021-04-26 7:27
 */
public class StringValueChangeEx {
    String str = new String("good");
    char[]  ch = {'t','e','s','t'};

    public void change(String str,char ch[]){
        str = "test ok";
        ch[0] = 'b';
    }

    public static void main(String[] args) {
        StringValueChangeEx test01 = new StringValueChangeEx();
        test01.change(test01.str, test01.ch);
        //这里涉及字符串的拼接,所以会用toString方法,而char中的toString返回的是哈希值,所以要用arrays类
        System.out.println(test01.str + " and " + Arrays.toString(test01.ch));   //good and [C@2f4d3709
        System.out.println(test01.str); //good
        System.out.println(test01.ch);  //test
    }
}
```

### 1.5 String类常用方法

1. `int Length()`:返回字符的长度: return value.Length
2. `char charAt(int index)`:返回某索引处的字return vaLue[index]
3. `booLean isEmpty()`:判断是否是空字符牢: return value. Length == 0
4. `String toLowercase()`:使用默认语言环境,将 String中的所有字符转换为小写
5. `String toUppercase()`:使用默认语言环境,将 String中的所有字符转换为大写
6. `String trim()`:返回字符的副本,忽略前导空白和尾部空白
7. `boolean equals(Object obj)`:比较字符的内容是否相同
8. `booLean equalsIgnoreCase(String anotherString)`:与equls方法类似,忽略大小写
9. `String concat(string str)`:将指定字符牢连接到此字符的结尾。等价于用"+"
10. `int compare To(String anotherstring)`:比较两个字符的大小
11. `String substring(int beginIndex)`:返回一个新的字符,它是此字符的从 beginIndex开始截取到最后一个子字符串.
12. `String substring(int beginIndex, int endindex)`:返回一个新字符串,它是此字符从beginIndex开始截取到endIndex(不包含)的一个子字符串.

**Demo:**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.util.Locale;

/**
 * @author 13roky
 * @date 2021-04-26 21:47
 */
public class CommonMethod {
    /*
    int Length():返回字符的长度: return value.Length
    char charAt( nt index):返回某索引处的字return vaLue[index]
    booLean isEmpty():判断是否是空字符牢: return value. Length == 0
    String toLowercase():使用默认语言环境,将 String中的所有字符转换为小写
    String toUppercase():使用默认语言环境,将 String中的所有字符转换为大写
    String trim():返园字符的副本,忽略前导空白和尾部空白
    boolean equals(Object obj):比较字符的内容是否相同
    booLean equalsIgnoreCase(String anotherString):与equls方法类似,忽略大小写
    String concat(string str):将指定字符牢连接到此字符的结尾。等价于用"+"
    int compare To(String anotherstring):比较两个字符的大小
    String substring(int beginIndex):返回一个新的字符,它是此字符的从 beginIndex开始截取到最后一个子字符串.
    String substring(int beginIndex, int endindex):返回一个新字符串,它是此字符从beginIndex开始截取到endIndex(不包含)的一个子字符串.
     */
    @Test
    public void test01(){
        String s1 ="HelloWorld";
        System.out.println(s1.length());
        System.out.println(s1.charAt(0));
        System.out.println(s1.charAt(9));
        System.out.println(s1.isEmpty());

        String s2 = s1.toLowerCase();
        System.out.println(s1);
        System.out.println(s2);

        String s3 = "    he  llo    world    ";
        String s4 = s3.trim();
        System.out.println(s3);
        System.out.println(s4);
    }

    @Test
    public void test02(){
        String s1 = "HelloWorld";
        String s2 = "helloworld";
        System.out.println(s1.equals(s2));
        System.out.println(s1.equalsIgnoreCase(s2));

        String s3 = "abc";
        String s4 = "def".concat(s3);
        System.out.println(s4);

        String s5 = "abc";
        String s6 = new String("abd");
        System.out.println(s5.compareTo(s6));

        String s7 = "13roky学Java";
        String s8 = s7.substring(2,6);
        System.out.println(s7);
        System.out.println(s8);
    }
}
```

13. `boolean endsWith(String suffix)`:测试此字符串是否以指定的后缀结束
14. `boolean startsWith(String prefix)`:测试此字符串是否以指定的前缀开始
15. `boolean startsWith(String prefix, int toffset)`:测试此字符串从指定索引开始的子字符串是否以指定的前缀开始
16. `boolean contains(CharSequence s)`:当且仅当此字符串包含指定的char值序列时,返回true
17. `int indexOf(String str)`: 返回指定子字符串在此字符串中第一次出现处的索引
18. `int indexOf(String str,int fromIndex)`:返回指定子字符串在此字符串中第一次出现处的索引,从指定的索引处开始
19. `int lastIndexOf(String str)`:返回指定子字符串在此字符串中最右边出现处的索引
20. `int lastIndexOf(String str,int fromIndex)`:返回指定子字符串在此字符串中最后一次出现处的索引,从指定的索引开始反向搜索(从右往左搜索)indexOf和**lastindexOf方法如果未找到,返回结果都是-1**

**Demo:**

```java
package com.broky.commonClass;

import jdk.jfr.DataAmount;
import org.junit.jupiter.api.Test;

import java.util.Locale;

/**
 * @author 13roky
 * @date 2021-04-26 21:47
 */
public class CommonMethod {
    /*
    boolean endsWith(String suffix):测试此字符串是否以指定的后缀结束
    boolean startsWith(String prefix):测试此字符串是否以指定的前缀开始
    boolean startsWith(String prefix, int toffset):测试此字符串从指定索引开始的子字符串是否以指定的前缀开始

    boolean contains(CharSequence s):当且仅当此字符串包含指定的char值序列时,返回true
    int indexOf(String str): 返回指定子字符串在此字符串中第一次出现处的索引
    int indexOf(String str,int fromIndex):返回指定子字符串在此字符串中第一次出现处的索引,从指定的索引处开始
    int lastIndexOf(String str):返回指定子字符串在此字符串中最右边出现处的索引
    int lastIndexOf(String str,int fromIndex):返回指定子字符串在此字符串中最后一次出现处的索引,从指定的索引开始反向搜索(从右往左搜索)
    indexOf和lastindexOf方法如果未找到,返回结果都是-1
    */
    @Test
    public void test03(){
        String str1 = "helloworld";
        boolean b1 = str1.endsWith("rld");
        System.out.println(b1);

        boolean b2 = str1.startsWith("He");
        System.out.println(b2);

        boolean b3 =str1.startsWith("ll",2);
        System.out.println(b3);

        String str2 = "wo";
        System.out.println(str1.contains(str2));

        System.out.println(str1.indexOf("lol"));
        System.out.println(str1.indexOf("l"));
        System.out.println(str1.indexOf("lo", 5));

        String str3 = "hellorworld";
        System.out.println(str3.lastIndexOf("or"));
        System.out.println(str3.lastIndexOf("or",6));
    }
    //什么情况下,indexOf(str)和lastIndexOf(str)返回值相同?
    //情况一:存在唯一的一个str.
    //情况二:不存在str
}
```

21. 替换:
        String replace(char oldChar,char newChar):返回一个新的字符串,它是通过用newChar替换oldChar
        String replace(CharSequence target,CharSequence replacement):使用字面值替换序列替换此字符串所有匹配字面值目标序列的子字符串.
        String replaceAll(String regex,String replacement):使用给定的replacement替换此字符串多有匹配给定的正则表达式的子字符串
        String replaceFirst(String regex,String replacement):使用给定的replacement替换此字符串匹配给定的正则表达式的第一个子字符串.
22. 匹配:
        boolean matches(String regex):告知此字符串是否匹配给定得正则表达式
23. 切片:
        String[] split(String regex):根据给定的正则表达式的匹配拆分此字符串
        String[] split(String regex,int limit):根据匹配给定的正则表达式来分此字符串,最多不超过limit个,如果超出,剩下的全部都放到最后一个元素

**Demo:**

```java
package com.broky.commonClass;

import jdk.jfr.DataAmount;
import org.junit.jupiter.api.Test;

import java.util.Locale;

/**
 * @author 13roky
 * @date 2021-04-26 21:47
 */
public class CommonMethod {
    /*
    替换
    String replace(char oldChar,char newChar):返回一个新的字符串,它是通过用newChar替换oldChar
    String replace(CharSequence target,CharSequence replacement):使用字面值替换序列替换此字符串所有匹配字面值目标序列的子字符串.
    String replaceAll(String regex,String replacement):使用给定的replacement替换此字符串多有匹配给定的正则表达式的子字符串
    String replaceFirst(String regex,String replacement):使用给定的replacement替换此字符串匹配给定的正则表达式的第一个子字符串.
    匹配:
    boolean matches(String regex):告知此字符串是否匹配给定得正则表达式
    切片:
    String[] split(String regex):根据给定的正则表达式的匹配拆分此字符串
    String[] split(String regex,int limit):根据匹配给定的正则表达式来分此字符串,最多不超过limit个,如果超出,剩下的全部都放到最后一个元素
     */
    @Test
    public void test04(){
        String str1 = "13roky学Java";
        String str2 = str1.replace('学','写');

        System.out.println(str1);
        System.out.println(str2);

        String str3 = str1.replace("13roky", "geek");
        System.out.println(str3);

        System.out.println("=====================================================");
        String str = "123klnjklsdnafdmc123pojasvapos";
        String string = str.replace("\\d+",",").replaceAll("^,|,$","|");
        System.out.println(string);

        str = "12345";
        //判断str字符串中是否全部有数字组成,即有1-n个数字组成
        boolean matches = str.matches("\\d+");
        System.out.println(matches);
        String tel = "0571-4534289";
        //判断一个电话是否是杭州的
        boolean result = tel.matches("0571-\\d{7,8}");
        System.out.println(result);

        System.out.println("================================================");
        str = "hello|world|java";
        String[] strs = str.split("\\|");

        for (int i = 0; i < strs.length; i++) {
            System.out.println(strs[i]);
        }
        System.out.println();
        str2 = "hello.world.java";
        String[] strs2 = str2.split("\\|");
        for (int i = 0; i < strs2.length; i++) {
            System.out.println(strs2[i]);
        }
    }
}
```



### 1.6 String与其它类型的转换

**demo：**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.io.UnsupportedEncodingException;
import java.util.Arrays;

/**
 * String类与其他结构之间的转换
 * String 与 char[] 之间的转换
 * String 与 byte[] 之间的转换
 *
 * @author 13roky
 * @date 2021-05-02 19:33
 */
public class StringChange {
    /*
    复习：String与其他数据类型，包装类之间的转换
    String --> 基本数据类型、包装类：调用包装类的静态方法：parseXxx(str)
    基本数据类型、包装类 ——》String：调用String重载的valueOf(xxx) 或者直接拼接“”
     */
    @Test
    public void test(){
        String str = "123";
        //int num = (int) str;   只有子父类的关系才可以使用强制类型转换

        int num = Integer.parseInt(str);

        String str2 = String.valueOf(num);
        String str3 = num + "";
    }

    /*
    String 与 char[] 之间的转换

    String --> char[] ：String类中的toCharArray()方法
    char[] --> String ：String的构造器
     */
    @Test
    public void test02(){
        String str = "abcde";

        char[] c1 = str.toCharArray();

        for (int i = 0; i < c1.length; i++) {
            System.out.println(c1[i]);
        }

        char[] c2 = new char[]{'f','s','c','a'};

        String str2 = new String(c2);
        System.out.println(str2);
    }

    /*
    String 与 byte[] 之间的转换
    编码：String --> byte[] ：调用String的getBytes()
    解码：

    转化的时候会涉及编码和解码
    编码：字符串 --> 字节 （看得懂转换为看不懂的二进制数据）
    解码 字节 --> 字符串 （看不懂的二进制数据转换为看得懂）

     */
    @Test
    public void test03() throws UnsupportedEncodingException {
        String str = "abc123此方";
        // 使用ide默认的编码集进行转换
        byte[]  b1 = str.getBytes();
        // 字节byte类型 采用ASCLL编码 由于ASCLL中没有中文编码，所以中文会转为默认的编码如（UTF-8，UTF-8中一个汉字占三位）然后再转为ASCLL
        System.out.println(Arrays.toString(b1));
        // 使用 gbk 字符集进行编码，需要处理异常
        byte[] b2 = str.getBytes("gbk");

        System.out.println(Arrays.toString(b2));
        System.out.println("=======================================");
        // 使用ide默认的编码集进行解码
        String str2 = new String(b1);
        System.out.println(str2);
        // 出现乱码。原因：编码及和解码集不一致倒置的
        String str3 = new String(b2);
        System.out.println(str3);

        // 指定编码集
        String str4 = new String(b2, "gbk");
        System.out.println(str4);
    }
}
```



### 1.7 常见算法题目

1. 模拟一个trim方法，去除字符串两端的空格。
2. 将一个字符串进行反转。将字符串中间指定部分进行反转。比如“abcdefg”反转为“abfedcg”。
3. 获取一个字符串在另一个字符串中出现的次数。
4. 获取两个字符串中最大的相同字符串。
5. 将字符串中字符进行自然顺序排序。Arrays.sort()

```java
package com.broky.commonClass.exer;

import org.junit.jupiter.api.Test;

import java.nio.CharBuffer;
import java.util.Arrays;
import java.util.Objects;

/**
 * 四道常见String算法题目
 * 1. 模拟一个 trim 方法，去除字符串两端的空格。
 *
 * @author 13roky
 * @date 2021-05-08 10:06
 */
public class Algorithms {
    @Test
    public void testMyTrim() {
        String s1 = "   123   ";
        s1.trim();
        s1 = myTrim(s1);
        System.out.println(s1);
    }

    // 模拟一个 trim 方法，去除字符串两端的空格。
    public String myTrim(String str) {
        if (str != null) {
            int start = 0;// 用于记录从前往后首次索引位置不是空格的位置的索引
            int end = str.length() - 1;// 用于记录从后往前首次索引位置不是空格的位置的索引

            while (start < end && str.charAt(start) == ' ') {
                start++;
            }

            while (start < end && str.charAt(end) == ' ') {
                end--;
            }
            if (str.charAt(start) == ' ') {
                return "";
            }

            return str.substring(start, end + 1);
        }
        return null;
    }

    @Test
    public void testMyReverse() {
        System.out.println(myReverse01("abcdefg", 2, 5));
        System.out.println(myReverse02("abcdefg", 2, 5));
        System.out.println(myReverse03("abcdefg", 2, 5));
    }

    /*
    将一个字符串进行反转。将字符串中指定部分进行反转。比如“abcdefg”反转为“abfedcg”
     */
    // 方式一：转换为char[]
    public String myReverse01(String str, int start, int end) {
        if (str != null) {
            char[] chars = str.toCharArray();
            char tmp = 0;
            for (int s = start, e = end; s < e; s++, e--) {
                tmp = chars[s];
                chars[s] = chars[e];
                chars[e] = tmp;
            }
            return new String(chars);
        }
        return null;
    }

    // 方式二：使用String的拼接
    public String myReverse02(String str, int start, int end) {
        if (str != null) {
            String partOne = str.substring(0, start);
            String parThree = str.substring(end + 1);
            for (int i = end; i >= start; i--) {
                partOne += str.charAt(i);
            }
            partOne += parThree;
            return partOne;
        }
        return null;
    }

    // 方式三：使用StringBuffer 或 StringBuilder 替换String
    public String myReverse03(String str, int start, int end) {
        StringBuilder builder = new StringBuilder(str.length());
        builder.append(str.substring(0, start));
        for (int i = end; i >= start; i--) {
            builder.append(str.charAt(i));
        }
        builder.append(str.substring(end + 1));
        return String.valueOf(builder);
    }

    /*
    获取一个字符串在另一个字符串中出现的次数。
    如：获取 "ab" 在 “abkkcakabkebfkabkskab” 出现的次数
     */
    @Test
    public void testGetCount() {
        getCount01("ab", "abkkcakabkebfkabkskab");
        getCount02("ab", "abkkcakabkebfkabkskab");
    }
    // 方法一：
    public int getCount01(String mainStr, String subStr) {
        int n = 0;
        for (int i = 0; i < subStr.length() - mainStr.length() + 1; i++) {
            if (mainStr.charAt(0) == subStr.charAt(i)) {
                if (mainStr.equals(subStr.substring(i, i + mainStr.length()))) {
                    n++;
                }
            }
        }
        System.out.println(n);
        return n;
    }
    // 方法二：
    public int getCount02(String mainStr, String subStr) {
        int count = 0;
        int index = 0;
        if (subStr.length() >= mainStr.length()) {
//            while ((index = subStr.indexOf(mainStr)) != -1) {
//                count++;
//                subStr = subStr.substring(index + mainStr.length());
//            }
            // 改进
            while ((index = subStr.indexOf(mainStr,index)) != -1) {
                count++;
                index += mainStr.length();
            }
            System.out.println(count);
            return count;
        } else {
            return 0;
        }
    }
}
```





## 2. StringBuffer 和 StringBuilder类



### 2.1 String、StringBuffer、StringBuilder 三者的异同

- String：不可变的字符序列，底层使用char[]进行存储
- StringBuffer：可变的字符序列，线程安全的，效率低，底层使用char[]进行存储
- StringBuilder：可变的字符序列，线程不安全的，效率高，jdk5.0新特性，底层使用char[]进行存储

**源码分析**：

- String：
          String str = new String();  // 底层代码为 char[] value = new char[0];
          String str1 = new String("abc");    // 底层代码为 char[] value = new char[]{'a','b','c'};
- StringBuffer:
          String sb1 = new StringBuffer();    // char[] value = new char[16];底层创建了一个长度是16的数组
          sb1.append('a');    // value[0] = 'a';
          sb1.append('b');    // value[1] = 'b';
- 问题：
  1. System.out.println(sb1.length()); 返回的是有的值count，而不是底层开辟的空间value.length.
  2. 扩容问题，如果要添加的数据底层数组盛不下了，那就需要扩容底层的数组。默认情况下，扩容为原来容量的2倍+2（源码中采用了位运算<<），同时将原有数组中的元素复制到新数组中。
- 总结：
  1. 开发中尽量不要用String，String不可变，效率最差，每次都会新造。而StringBuufer和StringBuilder只有长度不够用的时候才去扩容并复制。
  2. 开发中如果知道会频繁使用append时，建议使用StringBuffer的指定容量的构造器，避免之后进行扩容。



### 2.2 StringBuffer 类的常用方法

- StringBuffer append(xx):提供了很多的 append()方法,用于进行字符拼接
- StringBuffer delete( int start, int end):删除指定位置的内容，本身发生改变，并返回值。
- StringBuffer replace( int start, int end, String str):把 start,end)位置替换为str
- StringBuffer insert( int offset,xxx):在指定位置插入xxx
- StringBuffer reverse():把当前字符序列逆转
- public int indexof(string str)
- pubLic String substring (int start, int end) 返回一个从Start开始到End结束的左闭右开区间的子字符串，本身值并未变化
- public int Length
- public char charAt(int n)
- public void setcharAt (int n, char ch)
- 总结：
      增：append(xx)
      删：delete( int start, int end)
      改：setCharAt(int n,char ch) / replace( int start, int end, String str)
      查：charAt(int n)
      插：insert( int offset,xxx)
      长度：Length()
      遍历：for() + charAt() / toString()
- 方法链的原理：s1.append().append().append(); s1调用完append后返回的依旧是s1，可以继续调用append

```java
package com.broky.commonClass.exer;

import org.junit.jupiter.api.Test;

/**
 * StringBuffer 的常用方法
 *
 * @author 13roky
 * @date 2021-05-07 13:27
 */
public class StringBufferMethod {

    @Test
    public void test(){
        StringBuffer s1 = new StringBuffer("abc");
        s1.append(1);
        s1.append("234");
        System.out.println(s1.delete(2, 4));
        System.out.println(s1);
        System.out.println(s1.replace(2, 4, "hello"));
        System.out.println(s1.reverse());

    }
}
```



**关于append方法**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

public class StringBufferBuilderTest {
   
    @Test
    public void test2(){
        String str = null;
        StringBuffer sb = new StringBuffer();
        //StringBuffer 的 append 方法会将null值转换为字符“null”加入
        sb.append(str);

        System.out.println(sb.length());//4

        System.out.println(sb);//"null"

        StringBuffer sb2 = new StringBuffer(str);
        System.out.println(sb2);
    }
}
```



### 2.3 String、StringBuffer、StringBuilder 三者的效率 

**效率：**StringBuilder > StringBuffer > String

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

    /**
     * String StringBuffer StringBuilder 效率测试
     */
    @Test
    public void timeTest() {
        long startTime = 0L;
        long endTime = 0L;
        String text = "";
        StringBuffer buffer = new StringBuffer("");
        StringBuilder builder = new StringBuilder("");

        startTime = System.currentTimeMillis();
        for (int i = 0; i < 20000; i++) {
            buffer.append(String.valueOf(i));
        }
        endTime = System.currentTimeMillis();
        System.out.println("StringBuffer 执行时间" + (endTime - startTime));

        startTime = System.currentTimeMillis();
        for (int i = 0; i < 20000; i++) {
            builder.append(String.valueOf(i));
        }
        endTime = System.currentTimeMillis();
        System.out.println("StringBuilder 执行时间" + (endTime - startTime));

        startTime = System.currentTimeMillis();
        for (int i = 0; i < 20000; i++) {
            text = text + String.valueOf(i);
        }
        endTime = System.currentTimeMillis();
        System.out.println("String 执行时间" + (endTime - startTime));

    }
}
```



## 3. JDK8 之前的日期和时间 api 相关类



### 3.1 java.lang.System 类

**System** 类提供的 **public static long currentTimeMillis()** 用来返回当前时间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差。



### 3.2 java.util.Date 和 java.sql.Date 类

1. 两个构造器的使用
    > 构造器一：Date()：创建一个对应当前时间的Date对象
    > 构造器二：创建指定毫秒数的Date对象
2. 两个方法的使用
    > toString()：显示当前的年、月、日、时、分、秒
    > getTime()：获取当前Date对象对应的毫秒数。（时间戳）

3. java.sql.Date 对应这数据库中的日期类型的变量
    > 实例化
    > 将 sql.date 转换 util.date: 多态直接赋值
    > 将 util.date 转换 sql.date: 通过共同的getTime()时间戳实现

**Demo：**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.util.Date;

/**
 *  JDK8 之前的日期和时间的 api 测试
 *
 * @author 13roky
 * @date 2021-05-08 9:15
 */
public class DateTimeTest {

    /*
    System类提供的 public static long currentTimeMillis()
    用来返回当前时间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差。
     */
    @Test
    public void test() {
        long time = System.currentTimeMillis();
        System.out.println(time);
    }

    /*
    java.util.Date 类
            |---java.sql.Date 类 数据库
    1. 两个构造器的使用
        > 构造器一：Date()：创建一个对应当前时间的Date对象
        > 构造器二：创建指定毫秒数的Date对象
    2. 两个方法的使用
        > toString()：显示当前的年、月、日、时、分、秒
        > getTime()：获取当前Date对象对应的毫秒数。（时间戳）

    3. java.sql.Date 对应这数据库中的日期类型的变量
        > 实例化
        > 将 sql.date 转换 util.date: 多态直接赋值
        > 将 util.date 转换 sql.date: 通过共同的getTime()时间戳实现
     */

    @Test
    public void test02() {
        // 构造器一：Date()：创建一个对应当前时间的Date对象
        Date date1 = new Date();
        System.out.println(date1.toString());
        System.out.println(date1.getTime());

        // 构造器二：创建指定毫秒数的Date对象
        Date date2 = new Date(1620437519431L);

        java.sql.Date date3 = new java.sql.Date(1620437519431L);
        System.out.println(date3);

        // 将 util.date 转换 sql.date:
        // 情况一
        Date date4 = new java.sql.Date(1620437519431L);
        java.sql.Date date5 = (java.sql.Date) date4;
        // 情况二
        Date date6 = new Date();
        java.sql.Date date7 = new java.sql.Date(date6.getTime());

        /*
        当new的是父类的时候，如果强转为子类，那么编译时不会报错，运行时会报错，因为new的父类缺少子类特有的属性和方法
        当new的是子类的时候，但是赋给了父类，这时候是可以强转成子类的，如情况一
         */

    }
}
```

**注意：**

使用Data的构造器，设置时间会存在 **偏移量** 的问题

**Demo：**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.util.Date;

/**
 * JDK8 之前的日期和时间的 api 测试
 *
 * @author 13roky
 * @date 2021-05-08 9:15
 */
public class DateTimeTest {

    // Date中的偏移量
    @Test
    public void test01() {
        /*
        由于构造方法存在一个偏移量，年份是从1990年开始，月份是从0开始所以构造时应该减去偏移量
         */
        Date date1 = new Date(2020, 9, 8);
        System.out.println(date1);  // Fri Oct 08 00:00:00 CST 3920
        date1 = new Date(2020 - 1900, 9 - 1, 8);
        System.out.println(date1);  // Tue Sep 08 00:00:00 CST 2020
    }
}
```



### 3.3 SimpleDateFormat 类

SimpleDateFormat 是用来 格式化 Date 类中 时间格式的 和 对格式进行解析成 Date 的类

可以使用默认格式，也可以自定义格式。

1. 两个操作
    1.1 格式化：日期 ---> 字符串
    1.2 解析：格式化的逆过程，字符串 ---> 日期
2. SimpleDateFormat 的实例化

**Demo**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * jdk 8 之前的日期时间的API测试
 * 1. System类中currentTimeMillis();
 * 2. java.util.Date 和 子类 java.sql.Date
 * 3. SimpleDateFormat
 * 4. Calendar
 *
 * @author 13roky
 * @date 2021-05-09 12:53
 */
public class DateTimeTest2 {
    /*
    SimpleDateFormat 的使用：SimpleDateFormat对日期 Date 类的格式化和解析

    1. 两个操作
    1.1 格式化：日期 ---> 字符串
    1.2 解析：格式化的逆过程，字符串 ---> 日期

    2. SimpleDateFormat 的实例化
     */
    @Test
    public void testSimpleDateFormat() throws ParseException {
        // 实例化SimpleDateFormat:使用默认的构造器
        SimpleDateFormat sdf = new SimpleDateFormat();

        // 格式化：日期 ---> 字符串
        Date date = new Date();
        System.out.println(date);

        String format = sdf.format(date);
        System.out.println(format);

        // 解析： 格式化的逆过程，字符串 ---> 日期
        String str = "2021/5/9 下午1:04";
        Date date1 = null;

        date1 = sdf.parse(str);

        System.out.println(date1);

        //*********************************
        // SimpleDateFormat sdf1 = new SimpleDateFormat("yyyyy.MMMMM.dd GGG hh:mm aaa");
        SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
        String format1 = sdf1.format(date);
        System.out.println(format1);

        Date date2 = sdf1.parse("2021-05-09 01:09:56");
        System.out.println(date2);

    }
}
```

**练习**: 

1. 字符串“2020-02-23”转化为java.sql.Date
2. 三天打鱼两天晒网 1990-01-01    视频分析：[P482480.尚硅谷_常用类-SimpleDateFormat的课后练习2](https://www.bilibili.com/video/BV1Kb411W75N?p=482)

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

    @Test
    public void testEx01() throws ParseException {
        //练习一：字符串“2020-02-23”转化为java.sql.Date
        String str ="2020-02-23";
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");

        Date date = sdf.parse(str);
        System.out.println(date);

        java.sql.Date sqlDate = new java.sql.Date(date.getTime());

        System.out.println(sqlDate);
    }

    @Test
    public void testEx02() throws ParseException {
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        Date startDate = sdf.parse("1990-01-01");

        long oneDay = sdf.parse("1990-01-02").getTime() - startDate.getTime();
        long period = oneDay * 5;
        //        long now = sdf.parse(sdf.format(new Date())).getTime() - startDate.getTime();
        long now = sdf.parse("1990-01-06").getTime() - startDate.getTime() + oneDay;

        if (now % period > 3 * oneDay || now % period == 0) {
            System.out.println("晒网");
        } else {
            System.out.println("打鱼");
        }

    }
}
```



### 3.4 Calendar 类

- calendar类是一个抽象类，是可变的

**demo**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;

/**
 * jdk 8 之前的日期时间的API测试
 * 1. System类中currentTimeMillis();
 * 2. java.util.Date 和 子类 java.sql.Date
 * 3. SimpleDateFormat
 * 4. Calendar
 *
 * @author 13roky
 * @date 2021-05-09 12:53
 */
public class DateTimeTest2 {
    
     /*
        Calendar 日历类（抽象类）的使用
     */
    @Test
    public void testCalendar(){
        // 1. 实例化
        // 方式一： 创建其子类 （GregorianCalendar）的对象
        // 方式二：调用其静态方法getInstance()
        // 两种创建方法本质上一样
        Calendar calendar = Calendar.getInstance();
        System.out.println(calendar.getClass());

        // 2.常用方法
        //get()
        int days = calendar.get(Calendar.DAY_OF_MONTH);
        System.out.println(days);

        //set()
        calendar.set(Calendar.DAY_OF_MONTH,22);
        days = calendar.get(Calendar.DAY_OF_MONTH);
        System.out.println(days);

        //add()
        calendar.add(Calendar.DAY_OF_MONTH,-3);
        days = calendar.get(Calendar.DAY_OF_MONTH);
        System.out.println(days);

        //getTime(): 日历类-->Date
        Date date = calendar.getTime();
        System.out.println(date);

        //setTime(): Date--->日历类
        Date date1 = new Date();
        calendar.setTime(date1);
        days = calendar.get(Calendar.DAY_OF_MONTH);
        System.out.println(days);
    }
}
```

**注意**

- 获取月份时：一月是0，二月是1，以此类推12月是11
- 获取星期时：周日是1，周二是2.。。。周六是7



### 3.5 JDK8 之前时间日期api的一些问题

如果我们可以跟别人说:“我们在1502643933071见晚了!”那么就再简单不过了。但是我们希望时间与量夜和四季有关,于是事情就变复杂了。JDK10中包含了个 java util. Date类,但是它的大多数方法已经在JDK11引入 Calendar类之后被弃用而 Calendar并不比Date好多少。它们面临的问题是：

**可变性: **像日期和时间这样的类应该是不可变的。

**偏移性: **Date中的年份是从1900开始的,而月份都从0开始。

**格式化:** 格式化只对Date有用, Calendar则不行

此外,它们也**不是线程安全的**;**不能处理闰秒**等。

**因此我们需要引入 以下 新的时间日期api来处理这些问题**。

 

## 4. JDK8 中新的时间日期 api

- 第三次引入的API是成功的，并且Java8中引入的 java. time API 已经纠正了过去的缺陷，将来很长一段时间内它都会为我们服务。
- Java8 吸收了**Joda-Time**的精华，以一个新的开始为Java创建优秀的AP新的 java. time中包含了所有关于本地**日期( LocalDate)**、**本地时间( LocalTime)**、**本地日期时间( LocalDate Time)**、**时区( Zoned Date Time)**和**持续时间( Duration)**的类。历史悠久的Date类新增了 tolnstant()方法，用于把Date转换成新的表示形式。这些新增的本地化时间口期 API 大大简化了日期时间和本地化的管理。
- 在JDK8 之前joda-time是作为jar包的形式导入项目使用的，在jdk8 的时候官方将其引入.



### 4.1 LocalDate、LocalTime、LocalDateTime 类的使用

**实例化：**

- 实例化方式一：使用静态方法 `now() / now(zoneld zone)` 获取当前的日期时间
- 实例化方式二：使用静态方法 `of()`自定义时间，无 Date 中偏移量的问题



**方法 :** 

- `now() / now(zoneld zone)`
- `of()`

- `getXxx()` 获取相关属性
- `withXxxx()` 设置相关属性
- `plusXxx()`  在现有基础上加上指定数据
- `minusXxx()`  在现有基础上减去指定数据



**说明 :** 

- 三个类都具有不可变性
- LocalDateTime 相较于 LocalDate , LocalTime 使用频率更高
- 类似于 jdk8 之前的 Calendar 类

**Demo：**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;

/**
 * jdk 8 中的时间日期 api
 *
 * @author 13roky
 * @date 2021-05-10 7:05
 */
public class JDK8DateTimeTest {

    /*
    LocalDate、LocalTime、LocalDateTime 类的使用:

     */

    @Test
    public void test01(){
        // 实例化方式一：使用静态方法 now() / now(zoneld zone) 获取当前的日期时间
        LocalDateTime localDateTime = LocalDateTime.now();
        LocalDate localDate = LocalDate.now();
        LocalTime localTime = LocalTime.now();

        System.out.println(localDateTime);
        System.out.println(localDate);
        System.out.println(localTime);

        // 实例化方式二：of() 可以自定义时间，无 Date 中偏移量的问题

        LocalDateTime localDateTime1 = LocalDateTime.of(2020, 10, 1, 4, 23, 43);
        System.out.println(localDateTime1);

        // getXxx() 获取相关属性
        System.out.println(localDateTime.getDayOfMonth());
        System.out.println(localDateTime.getDayOfWeek());
        System.out.println(localDateTime.getMonth());
        System.out.println(localDateTime.getMonthValue());
        System.out.println(localDateTime.getMinute());

        // withXxxx() 设置相关属性， 具有不可变性
        LocalDate localDate1 = localDate.withDayOfMonth(20);
        System.out.println(localDate1);

        LocalDateTime localDateTime2 = localDateTime.withHour(16);
        System.out.println(localDateTime);
        System.out.println(localDateTime2);

        //plusXxx() 在现有基础上加上指定数据
        LocalDateTime localDateTime3 = localDateTime.plusMonths(3);
        System.out.println(localDateTime);
        System.out.println(localDateTime3);

        // minusXxx() 在现有基础上减去指定数据
        LocalDateTime localDateTime4 = localDateTime.minusMonths(3);
        System.out.println(localDateTime);
        System.out.println(localDateTime4);
    }
}
```



### 4.2 Instant 类的使用

> - Instant : 时间线上的瞬时点. 这可能被用来记录应用程序中的事件时间戳.

**方法 :**

- `now()` 静态方法, 返回 UTC 时区的 Instant 类对象
- `atOffset()` 添加 默认时区 与 当前需要时区 的 时间 的 偏移量
- `toEpochMilli()` 获取 1970年1月1日0时0分0秒 (UTC) 开始的毫秒数
- `ofEpochMilli()` 静态方法, 通过给定的毫秒数获取 Instant 的实例

**Demo :**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.time.*;

/**
 * jdk 8 中的时间日期 api
 *
 * @author 13roky
 * @date 2021-05-10 7:05
 */
public class JDK8DateTimeTest {

    /*
    Instant 的使用
     */

    @Test
    public void test02() {
        // now : 获取本初子午线的时间
        Instant instant = Instant.now(); //格林威治时间
        System.out.println(instant);

        // 添加 默认时区 与 当前需要时区 的 时间 的 偏移量
        OffsetDateTime offsetDateTime = instant.atOffset(ZoneOffset.ofHours(8));
        System.out.println(offsetDateTime);

        // 获取 1970年1月1日0时0分0秒 (UTC) 开始的毫秒数
        long milli = instant.toEpochMilli();
        System.out.println(milli);
        
        // ofEpochMilli 通过给定的毫秒数获取 Instant 的实例
        Instant instant1 = Instant.ofEpochMilli(1620783200875L);
        System.out.println(instant1);
    }
}
```



### 4.3 DateTimeFormatter 类

> 用于格式化和解析 LocalDate、LocalTime、LocalDateTime
>
> 类似于 SimpleDateFormat

**格式化方式 :**

1. 预定义的标准格式。如：ISO_LOCAL_DATE_TIME;ISO_LOCAL_DATE;ISO_LOCAL_TIME;
2. 本地化相关的格式。如：ofLocalizedDateTime(FormatStyle.LONG);
3. 自定义的格式。如：ofPattern("yyyy-MM-dd hh:mm:ss E");

**解析方式 :**

1. `parse()` 方法

**Demo :**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.time.*;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.time.temporal.TemporalAccessor;

/**
 * jdk 8 中的时间日期 api
 *
 * @author 13roky
 * @date 2021-05-10 7:05
 */
public class JDK8DateTimeTest {

    /*
    DateTimeFormatter:格式化或解析日期、时间
    类似于SimpleDateFormat
     */

    @Test
    public void test03() {
        // 方式一：预定义的标准格式。如：ISO_LOCAL_DATE_TIME;ISO_LOCAL_DATE;ISO_LOCAL_TIME;
        DateTimeFormatter formatter = DateTimeFormatter.ISO_LOCAL_DATE_TIME;
        LocalDateTime localDateTime = LocalDateTime.now();
        System.out.println(localDateTime);
        String s = formatter.format(localDateTime);
        System.out.println(s);
        // 因为使用标准格式进行格式化，所以 localDateTime 格式化前后的值基本没有变化，但是类型变变为了 String
        // 解析：字符串-->日期
        TemporalAccessor parse = formatter.parse("2021-05-12T20:48:52.1539765");
        System.out.println(parse);

        // 方式二：
        // 本地化相关的格式。如：ofLocalizedDateTime(FormatStyle.LONG);
        // FormatStyle.LONG / FormatStyle.MEDIUM / FormatStyle.SHORT : 适用于LocalDateTime
        DateTimeFormatter formatter1 = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM);
        String s1 = formatter1.format(localDateTime);
        System.out.println(s1);

        // 本地化相关的格式。如：ofLocalizedDate()
        // FormatStyle.FULL / FormatStyle.LONG / FormatStyle.MEDIUM / FormatStyle.SHORT : 适用于LocalDateTime
        DateTimeFormatter formatter2 = DateTimeFormatter.ofLocalizedDate(FormatStyle.FULL);
        String s2 = formatter2.format(LocalDate.now());
        System.out.println(s2);

        /*
        可能会报错 ： java.time.DateTimeException: Unable to extract ZoneId from temporal
        参考：https://stackoverflow.com/questions/59531046/java-time-datetimeexception-unable-to-extract-zoneid-from-temporal
         */

        // 方式三：自定义的格式。如：ofPattern("yyyy-MM-dd hh:mm:ss E");
        DateTimeFormatter formatter3 = DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss");
        String s3 = formatter3.format(localDateTime);
        System.out.println(s3);

        // 解析
        TemporalAccessor parse1 = formatter3.parse("2021-05-12 09:24:47");
        System.out.println(parse1);
    }
}
```



### 4.4 其它API

-  **ZoneId：**该类中包含了所有的时区信息，一个时区的ID，如 Europe/Paris
-  ZonedDateTime：一个在ISO-8601日历系统时区的日期时间，如 2007-12- 03T10:15:30+01:00 Europe/Paris。
  -  其中每个时区都对应着ID，地区ID都为“{区域}/{城市}”的格式，例如： Asia/Shanghai等
-  Clock：使用时区提供对当前即时、日期和时间的访问的时钟。
-  持续时间：Duration，用于计算两个“时间”间隔
- 日期间隔：Period，用于计算两个“日期”间隔
- TemporalAdjuster : 时间校正器。有时我们可能需要获取例如：将日期调整 到“下一个工作日”等操作。
- TemporalAdjusters : 该类通过静态方法 (firstDayOfXxx()/lastDayOfXxx()/nextXxx())提供了大量的常用 TemporalAdjuster 的实现。



## 5. Comparable 和 Comparator 接口

### 5.1 Comparable 接口 自然排序

> 用于对象与对象的比较, Comparable属于自然排序

**说明：**

​	Java中的对象, 正常情况下, 只能进行比较:==或!= 不能使用>或<

​	但是在开发场景中, 我们需要对多个对象进行排序, 言外之意, 就需要比较对象的大小

​	使用两个接口中的任何一个来实现: Comparable 或 Comparator

**Comparable 接口的使用举例 :**

  1. 像 String, 包装类等实现了 Comparable 接口, 重写了 `compareTo(obj)` 方法, 给出了比较两个对象
  2. 像 String, 包装类重写了`compareTo()`方法以后, 进行了从小到大的排列
  3. 重写 `compareTo(obj)` 的规则:
        如果当前对象 this 等于形参对象 obj, 则返回为零
        如果当前对象 this 小于形参对象 obj, 则返回负整数
        如果当前对象 this 大于形参对象 obj, 则返回正整数
  4. **对于自定义类来说**, 如果需要排序, 我们可以让自定义类实现 Comparable 接口, 重写 `compareTo(obj)` 方法并在 `compareTO(obj)` 方法中指明如何排序

**Demo：**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.util.Arrays;

/**
 * 1) 说明: Java中的对象, 正常情况下, 只能进行比较:==或!= 不能使用>或<
 *          但是在开发场景中, 我们需要对多个对象进行排序, 言外之意, 就需要比较对象的大小
 *          如何实现? 使用两个接口中的任何一个: Comparable 或 Comparator
 * 2) Comparable 接口的使用
 *
 * @author 13roky
 * @date 2021-05-12 22:15
 */
public class CompareTest {
    /*
    Comparable 接口的使用举例: 自然排序
    1. 像 String, 包装类等实现了 Comparable 接口, 重写了 compareTo(obj) 方法, 给出了比较两个对象
    2. 像 String, 包装类重写了compareTo()方法以后,进行了从小到大的排列
    3. 重写 compareTo(obj) 的规则:
        如果当前对象 this 等于形参对象 obj, 则返回为零
        如果当前对象 this 小于形参对象 obj, 则返回负整数
        如果当前对象 this 大于形参对象 obj, 则返回正整数
    4. 对于自定义类来说, 如果需要排序, 我们可以让自定义类实现 Comparable 接口, 重写compareTo(obj) 方法
        在 compareTO(obj) 方法中知名如何排序
     */
    @Test
    public void test01(){
        String[] arr = new String[]{"aa","cc","kk","mm","gg","jj","dd"};
        Arrays.sort(arr);
        System.out.println(Arrays.toString(arr));
    }

    @Test
    public void test02() {
        Goods[] arr = new Goods[4];
        arr[0] = new Goods("lenovoMouse",34);
        arr[1] = new Goods("dellMouse",43);
        arr[2] = new Goods("xiaomiMouse",12);
        arr[3] = new Goods("huaweiMouse",65);
        arr[3] = new Goods("MicroSoftiMouse",12);
		
        // 根据arr所对应类的CompareTo方法排序
        Arrays.sort(arr);
        System.out.println(Arrays.toString(arr));
    }

}
```

```java
package com.broky.commonClass;

/**
 * 商品类 实现Comparable方法
 *
 * @author 13roky
 * @date 2021-05-12 22:42
 */
public class Goods implements Comparable {
    private String name;
    private double price;

    public Goods() {
    }

    public Goods(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Goods{" + "name='" + name + '\'' + ", price=" + price + '}';
    }

    // 指明商品比较大小的方式：按照价格从低到高排序，再按照产品名称从第到高排序
    @Override
    public int compareTo(Object o) {
        if (o instanceof Goods) {
            // 方式一
            Goods goods = (Goods) o;
            if (this.price > goods.price) {
                return 1;
            } else if (this.price < goods.price) {
                return -1;
            } else {
                // 字符串类型的本身就有 compareTo 方法
                return this.name.compareTo(goods.name);
                // return -this.name.compareTo(goods.name); 加了负号变成了从高到低排
            }
            // 方式二
            //return Double.compare(this.price,goods.price);

        }
        throw new RuntimeException("传入的数据类型不一致");
    }
}
```



### 5.2 Comparator 接口 定制排序

> 根据定制的规则进行比较, Comparator属于定制排序
>
> 当元素的类型没有实现java.lang.Comparable 接口而又不方便修改代码，或者实现了java.lang.Comparable 接口的排序规则不适合当前的操作, 那么可以使用 Compare(obj o1,obj o2)方法, 比较o1和o2的大小

**说明 :**

1. 当元素的类型没有实现java.lang.Comparable接口而又不方便修改代码， 或者实现了java.lang.Comparable接口的排序规则不适合当前的操作，那么可以考虑使用 Comparator 的对象来排序，强行对多个对象进行整体排序的比较。

**使用 :**

1. 在 `Arrays.sort()` 方法中使用匿名内部类的方法实现定制排序 **(详情见Demo)**

**Comparable 接口与 Comparator 的使用的对比 : **

1. Comparable 接口的方式一旦实现,保证 Comparable 接口实现类的对象在任何位置都可以比较大小

2. Comparator 接口属于临时性的比较.

**Demo :**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.util.Arrays;
import java.util.Comparator;

/**
 * 1) 说明: Java中的对象, 正常情况下, 只能进行比较:==或!= 不能使用>或<
 *          但是在开发场景中, 我们需要对多个对象进行排序, 言外之意, 就需要比较对象的大小
 *          如何实现? 使用两个接口中的任何一个: Comparable 或 Comparator
 * 2) Comparable 接口与 Comparator 的使用的对比
 *          Comparable 接口的方式一旦实现,保证 Comparable 接口实现类的对象在任何位置都可以比较大小
 *          Comparator 接口属于临时性的比较.
 *
 * @author 13roky
 * @date 2021-05-12 22:15
 */
public class CompareTest {
    
    /*
    Comparator 接口的使用： 定制排序
    1. 背景
    当元素的类型没有实现java.lang.Comparable 接口而又不方便修改代码，
    或者实现了java.lang.Comparable 接口的排序规则不适合当前的操作
    那么可以使用 Compare(obj o1,obj o2)方法, 比较o1和o2的大小
    2. 重写 Compare(obj o1,obj o2) 方法, 比较o1和o2的大小
    如果方法返回正数,则表示o1大于o2
    如果方法返回负数,则表示01小于02
    如果方法返回0, 则表示相等
     */
    @Test
    public void test03(){
        // test1
        String[] arr = new String[]{"aa","cc","kk","mm","gg","jj","dd"};
        // 从大到小排序
        Arrays.sort(arr, new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                if(o1 instanceof String && o2 instanceof String){
                    String s1 = (String) o1;
                    String s2 = (String) o2;
                    return -s1.compareTo(s2);
                }
                throw new RuntimeException("输入的类型不一致");
            }
        });

        System.out.println(Arrays.toString(arr));

    }

    @Test
    public void test04() {

        Goods[] arr = new Goods[5];
        arr[0] = new Goods("lenovoMouse",34);
        arr[1] = new Goods("huaweiMouse",43);
        arr[2] = new Goods("xiaomiMouse",12);
        arr[3] = new Goods("huaweiMouse",65);
        arr[4] = new Goods("microSoftiMouse",12);

        Arrays.sort(arr,new Comparator(){
            // 指明商品比较大小的方式：再按照产品名称从第到高排序, 按照价格从高到低排序
            @Override
            public int compare(Object o1, Object o2) {
                if (o1 instanceof Goods && o2 instanceof Goods){
                    Goods g1 = (Goods) o1;
                    Goods g2 = (Goods) o2;
                    if (g1.getName().equals(g2.getName())){
                        return -Double.compare(g1.getPrice(),g2.getPrice());
                    }else {
                        return g1.getName().compareTo(g2.getName());
                    }
                }
                throw new RuntimeException("输入的数据类型不一致");
            }
        });

        System.out.println(Arrays.toString(arr));
    }
}
```



## 6. System 类

> System类代表系统，系统级的很多属性和控制方法都放置在该类的内部。 该类位于java.lang包。 
>
> 由于该类的构造器是private的，所以无法创建该类的对象，也就是无法实 例化该类。其内部的成员变量和成员方法都是static的，所以也可以很方便 的进行调用。   
>
>   成员方法  native long currentTimeMillis()： 该方法的作用是返回当前的计算机时间，时间的表达格式为当前计算机时 间和GMT时间(格林威治时间)1970年1月1号0时0分0秒所差的毫秒数。  void exit(int status)： 该方法的作用是退出程序。其中status的值为0代表正常退出，非零代表 异常退出。使用该方法可以在图形界面编程中实现程序的退出功能等。

**成员变量 ：**

- System类内部包含in、out和err三个成员变量，分别代表标准输入流 (键盘输入)，标准输出流(显示器)和标准错误输出流(显示器)。

**成员方法 :** 

- native long currentTimeMillis()： 该方法的作用是返回当前的计算机时间，时间的表达格式为当前计算机时 间和GMT时间(格林威治时间)1970年1月1号0时0分0秒所差的毫秒数。 
- void exit(int status)： 该方法的作用是退出程序。其中status的值为0代表正常退出，非零代表 异常退出。使用该方法可以在图形界面编程中实现程序的退出功能等。
-  void gc()： 该方法的作用是请求系统进行垃圾回收。至于系统是否立刻回收，则 取决于系统中垃圾回收算法的实现以及系统执行时的情况。 
-  String getProperty(String key)： 该方法的作用是获得系统中属性名为key的属性对应的值。系统中常见 的属性名以及属性的作用如下表所示：

![](https://i.vgy.me/ce0h3E.png)

 

## 7. Math类

> java.lang.Math提供了一系列静态方法用于科学计算。
>
> 其方法的参数和返回 值类型一般为double型。 

**方法 :**

- abs 绝对值 
- acos,asin,atan,cos,sin,tan 三角函数 
- sqrt 平方根 
- pow(double a,doble b) a的b次幂 
- log 自然对数 
- exp e为底指数 
- max(double a,double b) 
- min(double a,double b) 
- random() 返回0.0到1.0的随机数 
- long round(double a) double型数据a转换为long型（四舍五入） 
- toDegrees(double angrad) 弧度—>角度 
- toRadians(double angdeg) 角度—>弧度



## 8. BigInteger 与 BigDecimal 类

> 用于操作较大的整数

### 8.1 BigInteger 类

> 类似于整形

**说明 :**

1. Integer类作为int的包装类，能存储的最大整型值为2 31-1，Long类也是有限的， 最大为2 63-1。如果要表示再大的整数，不管是基本数据类型还是他们的包装类 都无能为力，更不用说进行运算了。
2. **java.math包的BigInteger可以表示不可变的任意精度的整数。**BigInteger 提供 所有 Java 的基本整数操作符的对应物，并提供 java.lang.Math 的所有相关方法。 另外，BigInteger 还提供以下运算：模算术、GCD 计算、质数测试、素数生成、 位操作以及一些其他操作。

**构造器 :**

- BigInteger(String val)：根据字符串构建 BigInteger 对象

**常用方法 :** 

public **BigInteger** abs()：返回此 BigInteger 的绝对值的 BigInteger。

BigInteger **add**(BigInteger val) ：返回其值为 (this + val) 的 BigInteger 

BigInteger subtract(BigInteger val) ：返回其值为 (this - val) 的 BigInteger 

BigInteger **multiply**(BigInteger val) ：返回其值为 (this * val) 的 BigInteger 

BigInteger **divide**(BigInteger val) ：返回其值为 (this / val) 的 BigInteger。整数 相除只保留整数部分。

BigInteger **remainder**(BigInteger val) ：返回其值为 (this % val) 的 BigInteger。 

BigInteger[] **divideAndRemainder**(BigInteger val)：返回包含 (this / val) 后跟 (this % val) 的两个 BigInteger 的数组。

BigInteger pow(int exponent) ：返回其值为 (thisexponent) 的 BigInteger。



### 8.2 BigDecimal 类

> 类似于浮点型

**说明 :**

1. 一般的Float类和Double类可以用来做科学计算或工程计算，但**在商业计算中， 要求数字精度比较高，故用到java.math.BigDecimal类**。
2. BigDecimal 类支持不可变的、任意精度的有符号十进制定点数。

**构造器 :** 

- public BigDecimal(double val) 

- public BigDecimal(String val)

**常用方法 :**

public BigDecimal **add**(BigDecimal augend)

public BigDecimal **subtract**(BigDecimal subtrahend) 

public BigDecimal **multiply**(BigDecimal multiplicand) 

public BigDecimal **divide**(BigDecimal divisor, int scale, int roundingMode)

**Demo :**

```java
package com.broky.commonClass;

import org.junit.jupiter.api.Test;

import java.math.BigDecimal;
import java.math.BigInteger;

/**
 * @author 13roky
 * @date 2021-05-13 12:56
 */
public class OtherClassTest {
    
    @Test
    public void testBigInteger() {
        BigInteger bi = new BigInteger("12433241123");
        BigDecimal bd = new BigDecimal("12435.351");
        BigDecimal bd2 = new BigDecimal("11");
        System.out.println(bi);
        // BigDecimal.ROUND_HALF_UP 四舍五入
        System.out.println(bd.divide(bd2, BigDecimal.ROUND_HALF_UP));
        System.out.println(bd.divide(bd2, 15, BigDecimal.ROUND_HALF_UP));
    }

}
```
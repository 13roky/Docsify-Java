## Java集合



## 1. Java 集合框架概述

> 一方面， 面向对象语言对事物的体现都是以对象的形式，为了方便对多个对象 的操作，就要对对象进行存储。
>
> 另一方面，使用Array数组存储对象方面具有一些弊端，而Java 集合就像一种**容器**，可以动态地把多个对象的引用放入容器中。



###  数组存储多个数据方面的特点

- 数组初始化以后，长度就确定了。
- 数组声明的类型，就决定了进行元素初始化时的类型。



### 数组在存储数据方面的弊端

- 数组初始化以后，长度就不可变了，不便于扩展
- 数组中提供的属性和方法少，不便于进行添加、删除、插入等操作，且效率不高。同时无法直接获取存储元素的个数
- 数组存储的数据是有序的、可以重复的。---->存储数据的特点单一



### 集合的特点

- Java 集合类可以用于存储数量不等的多个对象，还可用于保存具有映射关系的 关联数组。



### Java 集合可分为 Collection 和 Map 两种体系

- **Collection接口：**单列数据，定义了存取一组对象的方法的集合
  - **List接口：**元素有序、可重复的集合
  - **Set接口：**元素无序、不可重复的集合
- **Map接口：**双列数据，保存具有映射关系“key-value对”的集合



### 集合框架

- Collection接口：单列集合，用来存储一个个的对象
  - list接口：存储有序的、可重复的数据（动态数组）
    - 实现类：ArrayList、LinkedList、Vector
  - Set接口：存储无序的、不可重复的数组（高中讲的集合）
    - 实现类：HashSet、LinkedHashMap、TreeSet
- Map接口：双列集合，用来存储一对（key-value）一对的数据（高中函数 y=f(x)）
  - 实现类：HashMap、LinkedHashMap、TreeMap、Hashtable、Properties



## 2. Collection 接口

### Collection 接口方法

1. add(Object e) : 将元素e添加到集合中
2. addAll(Collection coll) : 将coll中的元素添加到当前的集合中
3. int size() : 获取添加的元素个数
4. boolean isEmpty() : 判断当前集合是否为空
5. void clear() : 清空集合元素
6. boolean contains(Object obj) : 是通过元素的equals方法来判断是否是同一个对象
7. boolean containsAll(Collection c)：也是调用元素的equals方法来比 较的。拿两个集合的元素挨个比较
8. boolean remove(Object obj) ：通过元素的equals方法判断是否是 要删除的那个元素。**只会删除找到的第一个元素**
9. boolean removeAll(Collection coll)：取当前集合的差集, 从当前集合中移除和 coll 集合共有的元素
10. boolean retainAll(Collection c)：把交集的结果存在当前集合中，不 影响c
11. boolean equals(Object obj) : 集合是否相等



### 对于 Collection 的说明

1. Collection 接口的实现类的对象中添加 obj 时, 要求 obj 所在类重写 equals().
2. Collection 无法储存基本数据类型, 其所储存这些基本数据类型时是采用装箱来储存的



::: details 点击查看 demo

```java
package com.broky.Collection;

import jdk.jfr.StackTrace;
import org.junit.jupiter.api.Test;

import java.util.*;

/**
 * @author 13roky
 * @date 2021-05-15 13:20
 */
public class CollectionTest {
    @Test
    public void test01(){
        Collection coll = new ArrayList();

        //add(Object e) : 将元素e添加到集合coll中
        coll.add("AA");
        coll.add("BB");
        coll.add(123);
        coll.add(new Date());

        //size() : 获取添加的元素个数
        System.out.println(coll.size());

        //addAll(Collection coll1) : 将coll1中的元素添加到当前的集合中
        Collection coll1 = new ArrayList();

        coll1.add(456);
        coll1.add("CC");
        coll.addAll(coll1);

        System.out.println(coll.size());
        System.out.println(coll);

        //isEmpty() : 判断当前集合是否为空
        System.out.println(coll.isEmpty());

        //clear() : 清空集合元素
        coll.clear();
        System.out.println(coll.isEmpty());
        System.out.println(coll);

        //contains() : 是通过元素的equals方法来判断是否是同一个对象
        coll.add(new String("Tom"));
        coll.add(new Person("13roky",21));
        coll.add(new Person("niko",22));

        System.out.println(coll.contains("Tom"));
        //这里是true, contains判断的是内容, 因为使用的是String重写的equals方法
        System.out.println(coll.contains(new String("Tom")));
        //这里是false, contains判断的是内存地址 因为Person类没有重写equals方法, 所以用的是父类Object的equals
        System.out.println(coll.contains(new Person("13roky", 21)));
        System.out.println(coll.contains(new Person("niko", 22)));

        /*
        这里可以看出, contains调用了两次 Person 的 equals 方法.
        其原理为, 依次拿要判断的 Person 对象去和集合中所存在的元素比对, 直到比对到出现 true 停止.
        也就是说将集合中的值依次放入 new Person("13roky", 21).equals() 中, 调用的次数为出现 true 时所调用的次数
         */

        //containsAll(Collection coll) : 判断形参coll中的所有元素是否都存在于集合中
        Collection coll2 = Arrays.asList(new Person("niko", 22),"Tom");
        System.out.println(coll.containsAll(coll2));

        //boolean remove(Object obj) ：通过元素的equals方法判断是否是, 要删除的那个元素。只会删除找到的第一个元
        System.out.println(coll);
        coll.remove("Tom");
        System.out.println(coll);
        /*
        如果对象所在类没有重写 equals 方法, 那么就无法删除
         */

        //boolean removeAll(Collection coll)：取当前集合的差集, 从当前集合中移除和 coll 集合共有的元素
        System.out.println(coll2);
        coll.removeAll(coll2);
        System.out.println(coll);

    }

    @Test
    void test02() {
        // boolean retainAll(Collection c)：把交集的结果存在当前集合中，不影响c
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add(456);
        coll.add(789);

        Collection coll1 = Arrays.asList(456,789,91011);
        coll.retainAll(coll1);
        System.out.println(coll);

        //boolean equals(Object obj) : 集合是否相等, 传入的是形参obj, 所以必须也得是形参集合才可返回true

        System.out.println(coll.equals(Arrays.asList(456, 789)));
        // 由于Collection 中 new 的是 ArrayList 类型的对象, 但是 ArrayList 存在顺序, 所以顺序不同会返回false
        System.out.println(coll.equals(Arrays.asList(789, 456)));

        //hashCode() : 获取当前对象的哈希值
        System.out.println(coll.hashCode());

        //Object[] toArray() : 集合转换成数组, 添加的时候添加的obj返回的时候也返回obj数组
        Object[] array = coll.toArray();
        System.out.println(Arrays.toString(array));

        //拓展 : 数组转集合, 调用Arrays类的静态方法asList()
        List<String> strings = Arrays.asList(new String[]{"AA", "BB", "CC"});
        System.out.println(strings);

        //这里使用基本数据类型会将整个数组识别为一个整体存入集合
        List ints = Arrays.asList(new int[]{123, 456});
        System.out.println(ints);

        //可以采用包装类的形式传入
        List ints2 = Arrays.asList(new Integer[]{123, 456});
        System.out.println(ints2);

        //也可一这样直接传入, 默认使用包装类包装
        List list = Arrays.asList(123, 456);
        System.out.println(list);

        //iterator()：返回Iterator接口的实例，用于集合遍历. 凡在IteratorTest.java中测试
        coll.iterator();
    }
}

class Person{
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {
        System.out.println("调用Person的equals方法");
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }

    @Override
    public String toString() {
        return "Person{" + "name='" + name + '\'' + ", age=" + age + '}';
    }
}
```

:::

## 3. Iterator 迭代器接口

> - Iterator对象称为迭代器(设计模式的一种)，主要用于遍历 Collection 集合中的元素。 
> - GOF给迭代器模式的定义为：提供一种方法访问一个容器(container)对象中各个元 素，而又不需暴露该对象的内部细节。迭代器模式，就是为容器而生。类似于“公 交车上的售票员”、“火车上的乘务员”、“空姐”。 
> - Collection接口继承了java.lang.Iterable接口，该接口有一个iterator()方法，那么所 有实现了Collection接口的集合类都有一个iterator()方法，用以返回一个实现了 Iterator接口的对象。 
> - Iterator 仅用于遍历集合，Iterator 本身并不提供承装对象的能力。如果需要创建 Iterator 对象，则必须有一个被迭代的集合。 
> - 集合对象每次调用iterator()方法都得到一个全新的迭代器对象，默认游标都在集合 的第一个元素之前。



### 过于 Iterator 的说明 

1. 集合每次调用 iterrator() 方法都得到一个全新的迭代器对象
2. 迭代器生成时, 默认指针都在第一个元素之前
3. 迭代器内部定义了 remove() , 可以再遍历的时候, 删除集合中的元素. 此方法不同于集合直接调用 remove()
4. 如果还未调用next()或在上一次调用 next 方法之后已经调用了 remove 方法， 再调用remove都会报IllegalStateException
5. 迭代器遍历主要是用来遍历 Collection 的, 不包括 Map



### Iterator 接口的方法

1. boolean hasNext()
2. <E> next()
3. void remove()



### 使用 

1. 使用 Collection 对象的 iterator 方法, 返回一个 Iterator 对象
2. 使用 Iterator 接口中的 hasNext(), next(), remove() 三个方法完成对集合的遍历操作
3. 在调用it.next()方法之前必须要调用it.hasNext()进行检测。若不调用，且 下一条记录无效，直接调用it.next()会抛出NoSuchElementException异常。



### 遍历操作 

1. 直接调用 next() 方法, 超出集合时会报错 (不推荐)
2. 使用 for 循环遍历 (不推荐)
3. 使用 while 循环遍历, 判断条件使用 hasNext() 方法判断是否还有下一个元素 (推荐)
4. 使用 for-each 增强for循环遍历 (内部仍然使用的迭代器 )



### 迭代器遍历的实现原理 

1. 当返回了一个迭代器对象时, 就会有一个指针指向集合第一个元素的上面
2. 当调用 hasNext() 方法时, 迭代器会判断指针的下一个位置是否存在元素
3. 如果存在元素, 那么执行 while 语句, 在 while 中执行 next() 方法, 这时**指针先下移, 然后并返回当前指针元素.**
4. 重复上述操作, 直到运行到指针指向最后一个元素时,  hasNext() 返回为 false 退出循环, 遍历结束

![](https://i.vgy.me/EkBNWQ.png)



::: details 点击查看 demo

```java
package com.broky.Collection;

import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Date;
import java.util.Iterator;

/**
 * @author 13roky
 * @date 2021-05-16 11:33
 */
public class IteratorTest {
    @Test
    void test01(){
        Collection coll = new ArrayList();
        coll.add("AA");
        coll.add("BB");
        coll.add(123);
        coll.add(new Date());

        Iterator iterator = coll.iterator();
        //方式一 (不推荐): 当超出集合范围后会报错
//        System.out.println(iterator.next());
//        System.out.println(iterator.next());
//        System.out.println(iterator.next());
//        System.out.println(iterator.next());

        //方式二 (不推荐): for循环遍历
//        for (int i = 0; i < coll.size(); i++) {
//            System.out.println(iterator.next());
//        }

        //方式三(推荐): while循环遍历, hasNext()函数用于判断还有没有下一个值
        while (iterator.hasNext()){
            //指针先下移,然后才返回元素
            System.out.println(iterator.next());
        }
        
        //方式四 : 增强for循环遍历
        for (Object o : coll) {
            System.out.println(o);
        }
        
                //错误方式一 : 
//        Iterator iterator1 = coll.iterator();
//        while (iterator1.next()!=null) {
//            System.out.println(iterator1.next());
//        }
        /*
        这种错误方式跳步输出, 在while判断实, 指针下移一次, 在输出时, 指针又下移一次.
         */

//        while (coll.iterator().hasNext()) {
//            System.out.println(coll.iterator().next());
//        }
        /*
        死循环, 每次while创建的都是一个新的迭代器对象
         */
    }
}
```

:::



## 4. Colltion 子接口 : List 接口  

>- 鉴于Java中数组用来存储数据的局限性，我们通常使用List替代数组
>- List集合类中元素有序、且可重复，集合中的每个元素都有其对应的顺序索引
>- List容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根据 序号存取容器中的元素
>- DK API中List接口的实现类常用的有：ArrayList、LinkedList和Vector



### 关于 List 的说明

1. 所有用数组的地方都可以用ArrayList



### List 的实现类

- ArrayList : 作为List的主要实现类, 线程不安全的, 效率高; 底层使用Object[]存储

*              LinkedList : 对于频繁插入, 删除操作, 使用此类效率比ArrayList高; 底层使用双向链表存储
*              Vector : 作为List的古老实现类, 线程安全的, 效率低; 底层使用Object[]存储 (一般开发不会用)



**面试题: ArrayList, LinkedList, Vector 三者的异同**

- 同 : 三个类都实现了List接口, 存储数据的特点相同: 存储有序的, 可重复的数据;

- 不同 : 见上



### ArrayList底层

- JDK 7情况下 (类似饿汉式)

  1. ArrayList list = new ArrayList(); //底层创建了长度是10的Object[]数组elementData

  2. list.add(123); //elementData[0] = new Integer(123);

  3. list.add(11); //如果此次的添加导致底层elementData数组容量不够, 则扩容; 

     默认情况下扩容为原来容量的1.5倍, 同时需要将原有数组中的数据复制到新的数组中.

  4. 开发中建议使用带参构造器

- JDK 8情况下 (类似懒汉式, 延迟了数组的创建)

  1. ArrayList list = new ArrayList(); //底层Object[] elementData初始化为{}. 并没有创建长度为10的数组.
  2. list.add(123); //第一次调用add()时, 底层才创建了长度为10的数组, 并将数据123添加到element
  3. 后续与 JDK7 一致

JDK7 中的ArrayList的对象的创建类似于单例的饿汉式, 而JDK8 中的ArrayList对象的创建类似于单例的懒汉式, 延迟了数组的创建, 节省内存 



### LinkedList底层

1. LinkedList list = new LinkedList(); //内部声明了Node类型的first和last属性, 默认值为null
2. list.add(123); //将123封装到Node中, 创建了Node对象.



### Vector底层

1. 基本就是线程安全版的ArrayList
2. 扩容时是扩容为原来的两倍
3. 开发中及时是涉及到线程安全会使用工具类完成, 一般不会使用Vector实现



### List 接口的方法

1. void add(int index, Object ele) : 在index位置插入ele元素
2. boolean addAll(int index, Collection eles):从index位置开始将eles中 的所有元素添加进来
3. Object get(int index):获取指定index位置的元素
4. int indexOf(Object obj):返回obj在集合中首次出现的位置, 如果不存在返回值为-1
5. int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置
6. Object remove(int index):移除指定index位置的元素，并返回此元素
7. Object set(int index, Object ele):设置指定index位置的元素为ele, 将原来的值覆盖
8. List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex 位置的子集合

::: details 点击查看 demo

```java
package com.broky.Collection;

import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * @author 13roky
 * @date 2021-05-16 19:04
 */
public class ListTest {
    @Test
    void test01(){
        ArrayList list = new ArrayList();
        list.add(123);
        list.add(456);
        list.add("AA");
        list.add(new Person("Tom",12));
        list.add(456);

        System.out.println(list);

        //void add(int index, Object ele) : 在index位置插入ele元素
        list.add(1,"BB");
        System.out.println(list);

        //boolean addAll(int index, Collection eles):从index位置开始将eles中 的所有元素添加进来
        List list1 = Arrays.asList(1, 2, 3);
        list.addAll(list1);
        System.out.println(list.size());

        //Object get(int index):获取指定index位置的元素
        System.out.println(list.get(1));

        //int indexOf(Object obj):返回obj在集合中首次出现的位置, 如果不存在返回值为-1
        System.out.println(list.indexOf(456));

        //int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置
        System.out.println(list.lastIndexOf(456));

        //Object remove(int index):移除指定index位置的元素，并返回此元素
        Object obj = list.remove(1);
        System.out.println(obj);
        System.out.println(list);

        //Object set(int index, Object ele):设置指定index位置的元素为ele
        list.set(1,"CC");
        System.out.println(list);

        //List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex 位置的子集合, 左闭右开

        System.out.println(list.subList(1, 3));

    }
}
```

:::



**List遍历**

- 迭代器方式
- 增强for循环
- 普通循环

:::  details 点击查看 demo

```java
package com.broky.Collection;

import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Iterator;
import java.util.List;

/**
 * @author 13roky
 * @date 2021-05-16 19:04
 */
public class ListTest {
 
    //List遍历
    @Test
    void test02() {
        ArrayList list = new ArrayList();
        list.add(123);
        list.add(456);
        list.add("AA");
        list.add(new Person("Tom",12));
        list.add(456);

        //方式一 : 迭代器
        Iterator iterator = list.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }

        //方式二 : 增强for循环
        for (Object o : list) {
            System.out.println(o);
        }

        //方式三 : 普通循环

        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
    }
}
```

:::



### 总结ArrayList常用方法

- 增 : add(Object obj)
- 删 : remove(int index) / remove(Object obj) 一个是自己的方法, 一个是Collection中的方法
- 改 : set(int index, Object obj)
- 查 : get(int index)
- 插 : add(int index, Object obj)
- 长度 : size()
- 遍历 : 
  - 迭代器方式
  - 增强for循环
  - 普通循环



### 关于remove方法的注意点

**说明**

- Collection 接口中存在一个  `remove(Object obj)` 方法, 用于删除集合中的对象.
- Collection 子接口 List 接口中存在一个 `remove(int index)` 方法, 用于删除对应的索引处的对象.
- 所以 实现了 List 接口的类  也实现了以上的两个方法

**解析**

1. 当使用 `add()` 向集合中添加数据时, 如果是基本数据类型, 方法会将传入的数据自动装箱成对象存入.
2. 当使用 `remove()` 方法时, 并不会对其自动装箱, 如果传入 int 类型那么就会使用 `remove(int index)`, 如果传入对象就会调用`remove(Object obj)`

::: details 点击查看Demo

```java
package com.broky.Collection;

import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.Collection;

/**
 * @author 13roky
 * @date 2021-05-18 7:11
 */
public class ListExer {
    @Test
    void test01(){
        ArrayList list = new ArrayList();
        //像list中存的数据是基本数据类型1234, 但实际上会将1234自动装箱, 以对象的形式存入list
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);
        //remove有两种使用方法 remove(int index) / remove(Object obj) 一个是List自己的,一个是Collection的
        //由于add添加的2是自动装箱后的对象, 但是remove不会自动装箱, 所以将2当成了index而不是obj, 因此remove(2)删除的是索引.
        list.remove(2);
        System.out.println(list);
        //这里因为形参是一个对象, 所以也就会匹配remove(Object obj), 删除list中相符的对象
        list.remove(new Integer(2));
        System.out.println(list);

    }
}
```

:::



## 5. Collection 子接口 : Set 接口

> 存储无序的, 不可重复的数据



### Set 的实现类

- HashSet : 作为 Set 接口的主要实现类; 线程不安全的; 可以存储 null 值
  - LinkedHashSet : 作为 HashSet 的子类; 遍历其内部数据时可以按照添加时的顺序去遍历
- TreeSet : 可以按照添加对象的指定属性, 进行排序



### Set 存储无序的, 不可重复的数据

- 无序性
  - 不等于随机性
  - hashset 底层也是用数组存储, 无序性就是向数组中添加元素, 不是按照数据索引顺序添加的, 而是按照添加元素的哈希值添加的

- 不可重复性
  - 保证添加的元素按照equals方法判断时不能返回true. 即相同的元素不能添加进来



### 添加元素的过程, 以 HashSet 为例



![](https://i.vgy.me/aT84mA.png)



1. 在 HashSet 的底层是采用 数组 和 链表 的结构来存储对象的

2. HashSet 对象被实例化时, 如果没有指定长度, 其底层会默认为这个对象生成一个长度为 16 的数组

3. 当添加 对象a 时, 会调用 对象a 的 hashCode 方法获取其哈希值, 将哈希值与数组长度做某种算法的运算, 得到的数值就是插入时其位于底层数组中的索引值.

4. 当添加 对象b 时, 如果经计算得出 对象b 应存放的位置无对象, 则 对象b 添加成功.

5. 当添加 对象b 时, 如果经计算得出 对象b 应存放的位置有 对象a, 则比较二者的哈希值.

6. 当 对象b 的哈希值与 对象a 相同时, HashSet 会调用 对象b 的 equals 方法 (equals判断的是哈希值是否相等) 与数组中的 对象a 比较, 如果返回是 false, 那么 对象b 不会插入数组, 如果返回值是 false 对象b 会以链表的形式存入

7. 在链表中 a和b的 方向的指向在 jdk7 和 jdk8 中有不同, 7上8下

   jdk7 中 : 新增对象放到数组中指向被挤出去的原来的元素

   jdk8中 : 原来的对象放在数组中, 指向新增元素

8. 如果数组的使用率超过 0.75, 就会扩大为原来的两倍.



### equals() 和 hashCode 的重写

1. equals() 方法盘算的是哈希值是否相等, 如果不重写 hashCode 方法, 那么调用的就是 Objetc 中的 hashCode 方法, 在 Object 的 hashCode 中的哈希值可以理解为是随机生成的.

2. 为什么IDEA和Eclipse中重写hashSet会有31
   - 选择系数的时候要选择尽量大的系数。因为如果计算出来的hash地址越大，所谓的 “冲突”就越少，查找起来效率也会提高。（减少冲突）
   - 并且31只占用5bits,相乘造成数据溢出的概率较小。
   - 31可以 由i*31== (i<<5)-1来表示,现在很多虚拟机里面都有做相关优化。（提高算法效率）
   - 31是一个素数，素数作用就是如果我用一个数字来乘以这个素数，那么最终出来的结 果只能被素数本身和被乘数还有1来整除！(减少冲突)



> [哈希碰撞]([(22条消息) 通俗讲解哈希表，哈希碰撞问题！_You can walk as far as you want.-CSDN博客](https://blog.csdn.net/qq_33384191/article/details/103417171?ops_request_misc=%7B%22request%5Fid%22%3A%22162137114316780357251320%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=162137114316780357251320&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-103417171.first_rank_v2_pc_rank_v29&utm_term=哈希碰撞&spm=1018.2226.3001.4187))



### 说明

1. Set 接口中没有额外定义新的方法, 使用的都是 Collection 中声明的方法
2. 向 Set 中添加的数据, 其所在类一定要重写 hashCode() 和 equals()
3. 重写 hashCode() 和 equals() 尽可能保持一致性 : 相等的对象必须具有相等的散列码

4. 重写两个方法的小技巧 : 对象中用作 equals() 方法比较的 Field, 都应该用来计算 hashCode

5. **HashSet 内部其实是靠 HashMap 来完成的, new 一个 HashSet 的时候, 在其内部也 new 了 HashMap**



### Demo

```java
package com.broky.Collection;

import org.junit.jupiter.api.Test;

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

/**
 * @author 13roky
 * @date 2021-05-18 21:52
 */
public class SetTest {
    @Test
    void Test01(){
        Set set = new HashSet();
        set.add(456);
        set.add(123);
        set.add("AA");
        set.add("CC");
        set.add(new User("tom",12));
        set.add(new User("tom",12));
        set.add(129);

        Iterator iterator = set.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
        
    }
}
```

```java
package com.broky.Collection;

import java.util.Objects;

/**
 * @author 13roky
 * @date 2021-05-18 22:07
 */
public class User {
    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" + "name='" + name + '\'' + ", age=" + age + '}';
    }

    @Override
    public boolean equals(Object o) {
        System.out.println("User.equals");
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        User user = (User) o;
        return age == user.age && Objects.equals(name, user.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```



### LinkedHashSet

> 为了频繁的遍历而涉及的类, 在原有的HashSet上, 在添加数据的同时, 每个数据还维护了两个引用, 记录此数据的前一个数据和后一个数据, 既有next 也有 front

**优点**

- 对于频繁的遍历操作, LinkedHashSet 效率高于 HashSet



### TreeSet

**说明**

- 向 TreeSet 中添加的数据, 要求是向同类的对象

  

**两种排序方式, 自然排序, 定制排序**

- 自然排序中比较两个对象是否相同的标准为 : comparable() 返回0, 不再是equals()
- 定制排序中比较两个对象是否相同的标准为 : compare() 返回0, 不再是equals()

**TreeSet 底层 **(了解)

- TreeSet 和后面要讲的 TreeMap 采用红黑树的存储结构, 其特点为有序, 查询速度比 List 快



## Map 接口



### Map 实现类的结构

 * |----Map:双列数据，存储key-value对的数据     --类似于高中的函数：y=f(x)
   *      |----HashMap:作为Map的主要实现类；线程不安全的，效率高；可以存储null的key和value
   *      |----LinkedHashMap: 保证在遍历map元素时, 可以安装添加的顺序实现遍历
   *      原因: 在原有的HashMap底层结构基础上, 添加了一堆指针, 指向前一个和后一个元素
   *      对于频繁的遍历操作, 此类的执行效率高于HashMap
   *      HashMap底层: 数组+链表(jdk7之前)
   *      数组+链表+红黑树(jdk8)
 * |----TreeMap:保证按照添加的key-value对进行潘旭, 实现排序遍历. 此时考虑key的自然排序或定制排序，底层使用红黑树
 * |----Hashtable:作为古老的实现类；线程安全的，效率低；不可以存储null的key和value
   *          |----Properties: 常用来处理配置文件. key和value都是String类型



### 面试题

1. HashMap的底层实现原理?

2. HashMap和Hashtablede的异同?

3. CurrentHashMap（分段锁）和Hashtable的异同?



### Map 结构的理解

Map 中的 Key : 无序的, 不可重复的, 使用Set存储所有的 Key ----> 对于 hashMap 来说 key 所在的类要重写equals() 和 hashCode()

Map 中的 Value : 无序的, 可重复的, 使用 Collection 存储所有的 Value ----> value 所在类要重写 equals()

一个键值对 : key-value 构成了一个Entry 对象.

Map 中的 entry : 无序的, 不可重复的, 使用 Set 存储所有的 entry



### HashMap的底层原理, 以jdk7为例 

1. HashMap map = new HashMap() :
   在实例化以后, 底层创建了长度是16的一维数组 Entry[] table
2. 可能已经执行过多次put，map.put(key1,value1) :
   首先, 调用 key1 所在类的 hashCoed() 计算key1哈希值, 此哈希值经过某种算法计算后,得到在 Entry 数组中的存放位置
   如果此位置上数据为空, 此时的 key1-value1 添加成功     ----情况1
   如果此位置上的数据不为空, (意味着此位置上存在一个或多个数据(以链表形式存在)), 比较 key1 和已经存在的有个或多个数据的哈希值:
   - 如果key1的哈希值与已经存在的数据的哈希值都不相同, 此时key1-value1添加成功       ----情况2
   - 如果key1的哈希值和已经存在的某一个数据的哈希值相同, 继续比较: 
     - 调用key1所在类的equals()方法, 比较:
       	如果equals()返回false : 此时key1-value1添加成功       ----情况3

补充: 关于情况2和情况3: 此时key1-value1和原来的数据以链表的方式存储

在不断地添加过程中, 会涉及到扩容问题, 当超出临界值（且要存放的位置非空）时，扩容。默认的扩容方式: 扩容为原来容量的2倍, 并将原有的数据复制过来.

如果扩容，原来数据的存储位置需要重新进行计算，所以扩容前后的原有数据位置可能会不一样



### jdk8 相较于 jdk7 在HashMap底层实现方面的不同

1. new HashMap() : 底层没有创建一个长度为16的数组

2. jdk8 底层的数组是 : Node[], 而非Entry[]

3. 首次使用 put() 方法时, 底层创建长度为16的数组

4. jdk7 底层结构只有: 数组+链表. jdk8 中底层结构: 数组+链表+红黑树

   当数组的某一个索引位置上的元素以链表的形式存在的数据个数 > 8 且当前数组的长度 > 64时,

   此时此索引位置上的所有数据改为使用红黑树存储.



### Map中定义的方法



**添加、删除、修改操作：** 

- Object put(Object key,Object value)：将指定key-value添加到(或修改)当前map对象中 
- void putAll(Map m):将m中的所有key-value对存放到当前map中 
- Object remove(Object key)：移除指定key的key-value对，并返回value 
- void clear()：清空当前map中的所有数据

```java
@Test
void test01(){

    Map map = new HashMap();

    //put()
    map.put("AA", 123);
    map.put("BB", 123);
    map.put(45, 56);
    map.put("AA", 87);
    System.out.println(map);

    //putAll()
    HashMap map1 = new HashMap();
    map1.put("CC",98);
    map1.put("DD",47);

    map.putAll(map1);
    System.out.println(map);

    //remove()
    Object remove = map.remove("CC");
    System.out.println(remove);
    System.out.println(map);

    //clear()
    System.out.println(map.size());
    map.clear();
    System.out.println(map.size());

}
```



**元素查询的操作：** 

- Object get(Object key)：获取指定key对应的value 
- boolean containsKey(Object key)：是否包含指定的key 
- boolean containsValue(Object value)：是否包含指定的value 
- int size()：返回map中key-value对的个数 
- boolean isEmpty()：判断当前map是否为空 
- boolean equals(Object obj)：判断当前map和参数对象obj是否相等

```java
@Test
void test02() {

    Map map = new HashMap();
    map.put("AA", 123);
    map.put("BB", 123);
    map.put(45, 56);
    map.put("AA", 87);
    System.out.println(map);

    //get()
    System.out.println(map.get("AA"));

    //containsKey()
    System.out.println(map.containsKey("AA"));

    //containsValue()
    System.out.println(map.containsValue(87));


    //isEmpty()
    System.out.println(map.isEmpty());

    //size()
    System.out.println(map.size());

    //equals()
    Map map1 = map;
    System.out.println(map.equals(map1));

}
```



**元视图操作的方法：** 

- Set keySet()：返回所有key构成的Set集合 
- Collection values()：返回所有value构成的Collection集合 
- Set entrySet()：返回所有key-value对构成的Set集合

```java
@Test
    void test03() {

        Map map = new HashMap();
        map.put("AA", 123);
        map.put("BB", 123);
        map.put(45, 56);
        map.put("AA", 87);
        System.out.println(map);

        //Set keySet()
        Set keySet = map.keySet();
        System.out.println(keySet);

        //Collection values()
        Collection values = map.values();
        System.out.println(values);

        //遍历key-values方式一
        //Set entrySet()
        Set entrySet = map.entrySet();
        System.out.println(entrySet);
        Iterator iterator = entrySet.iterator();
        while (iterator.hasNext()){
            //entrySet中的元素都是entry
            Object obj = iterator.next();
            Map.Entry entry = (Map.Entry) obj;
            System.out.println(entry.getKey() + "------>" + entry.getValue());
        }

        //遍历key-values方式二
        Iterator iterator1 = keySet.iterator();
        while (iterator1.hasNext()) {
            Object key1 = iterator1.next();
            Object values1 = map.get(key1);
            System.out.println(key1 + "=====" + values1);
        }
    }
```



### HashMap 源码解析 jdk1.7



**继承关系**

```java
public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable {
```

继承了Dictionary，实现了Map、Cloneable、Serializable接口



**基本属性和默认值**

```java
private transient Entry<K,V>[] table;
private static class Entry<K,V> implements Map.Entry<K,V> {
        int hash;
        final K key;
        V value;
        Entry<K,V> next;
//该属性用来存储HashTable的数据
//当前采用的是哈希表的存储结构，采用的是链地址法解决哈希冲
 
 
//表示集合中存储的Entry实体个数=hashmap->size
private transient int count;
 
//阈值  threshold=loadFactor*table.length
private int threshold;
 
 
//加载因子[0-1]的取值范围
private float loadFactor;
 
//版本记录
private transient int modCount = 0;
```



**构造器**

```java
public Hashtable(int initialCapacity, float loadFactor) {
	//校验
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal Load: "+loadFactor);
 
        if (initialCapacity==0)
            initialCapacity = 1;
        this.loadFactor = loadFactor;
	//对hash中数组做初始化
        table = new Entry[initialCapacity];
	//变更加载因子
        threshold = (int)Math.min(initialCapacity * loadFactor, MAX_ARRAY_SIZE + 1);
        initHashSeedAsNeeded(initialCapacity);
    }
 
    /**
     * Constructs a new, empty hashtable with the specified initial capacity
     * and default load factor (0.75).
     *
     * @param     initialCapacity   the initial capacity of the hashtable.
     * @exception IllegalArgumentException if the initial capacity is less
     *              than zero.
     */
    public Hashtable(int initialCapacity) {
        this(initialCapacity, 0.75f);
    }
 
    /**
     * Constructs a new, empty hashtable with a default initial capacity (11)
     * and load factor (0.75).
     */
    public Hashtable() {
        this(11, 0.75f);
    }
 
 
 public Hashtable(Map<? extends K, ? extends V> t) {
        this(Math.max(2*t.size(), 11), 0.75f);
        putAll(t);
    }
/*默认值：
初始容量默认值：11
加载因子默认值：0.75f
*/
```



**put操作**

```java
public synchronized V put(K key, V value) {
        // Make sure the value is not null
        if (value == null) {
            throw new NullPointerException();
        }
	//HashTable中的value值是不能为null的
 
        // Makes sure the key is not already in the hashtable.
        Entry tab[] = table;
        int hash = hash(key);
        int index = (hash & 0x7FFFFFFF) % tab.length;
	//key是不能重复的
        for (Entry<K,V> e = tab[index] ; e != null ; e = e.next) {
            if ((e.hash == hash) && e.key.equals(key)) {
                V old = e.value;
                e.value = value;
                return old;
            }
        }
	
        modCount++;
        if (count >= threshold) {
            // Rehash the table if the threshold is exceeded
            rehash();
 
            tab = table;
            hash = hash(key);
            index = (hash & 0x7FFFFFFF) % tab.length;
        }
 
        // 采用的是头插法插入新的节点
        Entry<K,V> e = tab[index];
        tab[index] = new Entry<>(hash, key, value, e);
        count++;
        return null;
    }
```

synchronized是线程同步关键字，其修饰在HashTable中的put方法
即表明同一时刻只能有一个线程来访问HashTable的实例，所以HashTable是线程安全的
为什么有些地方不需要加锁（即没有synchronize）

HashTable中的key和value都不能为null
key是不能重复的，value是可以重复的

**rehash操作**

```java
protected void rehash() {
        int oldCapacity = table.length;
        Entry<K,V>[] oldMap = table;
 
        //扩容机制   2*table.length+1
        int newCapacity = (oldCapacity << 1) + 1;
        if (newCapacity - MAX_ARRAY_SIZE > 0) {
            if (oldCapacity == MAX_ARRAY_SIZE)
                // Keep running with MAX_ARRAY_SIZE buckets
                return;
            newCapacity = MAX_ARRAY_SIZE;
        }
        Entry<K,V>[] newMap = new Entry[newCapacity];
 
        modCount++;
        threshold = (int)Math.min(newCapacity * loadFactor, MAX_ARRAY_SIZE + 1);
        boolean rehash = initHashSeedAsNeeded(newCapacity);
 
        table = newMap;
 
 
//重新hash
        for (int i = oldCapacity ; i-- > 0 ;) {
            for (Entry<K,V> old = oldMap[i] ; old != null ; ) {
                Entry<K,V> e = old;
                old = old.next;
 
                if (rehash) {
                    e.hash = hash(e.key);
                }
                int index = (e.hash & 0x7FFFFFFF) % newCapacity;
                e.next = newMap[index];
                newMap[index] = e;
            }
        }
    }
 
 
 private int hash(Object k) {
        
        return hashSeed ^ k.hashCode();//key不能为null，从这里提现出来的
    }
 
```



**get操作**

```java
public synchronized V get(Object key) {
        Entry tab[] = table;
        int hash = hash(key);
        int index = (hash & 0x7FFFFFFF) % tab.length;
        for (Entry<K,V> e = tab[index] ; e != null ; e = e.next) {
            if ((e.hash == hash) && e.key.equals(key)) {
                return e.value;
            }
        }
        return null;
    }
//有的版本get操作没有加synchronized？
//get操作只是查询，没有修改类的操作
```



**remove操作**

```java
public synchronized V remove(Object key) {
//类似与单链表的删除，主要分析是头结点和不是头结点的区别
        Entry tab[] = table;
        int hash = hash(key);
        int index = (hash & 0x7FFFFFFF) % tab.length;
        for (Entry<K,V> e = tab[index], prev = null ; e != null ; prev = e, e = e.next) {
            if ((e.hash == hash) && e.key.equals(key)) {
                modCount++;
                if (prev != null) {
                    prev.next = e.next;
                } else {
                    tab[index] = e.next;
                }
                count--;
                V oldValue = e.value;
                e.value = null;
                return oldValue;
            }
        }
        return null;
    }
//删除操作使用synchronize修饰，即同一时刻只能是一个线程访问remove()方法
```



### HashMap 源码解析 jdk1.8



### LinkedHashMap 源码解析



### TreeMap

> 向TreeMap中添加key-value, 要求key必须是由同一个类创建的对象, 因为要按照key进行排序 : 自然排序, 定制排序



```java
//TreeMap自然排序
@Test
void testTreeMap() {
    TreeMap map = new TreeMap();
    User u1 = new User("Tom", 12);
    User u2 = new User("Jerry", 12);
    User u3 = new User("Bob", 12);
    User u4 = new User("TK", 12);
    map.put(u1,12);
    map.put(u2,89);
    map.put(u3,48);
    map.put(u4,34);
    Set entrySet = map.entrySet();
    Iterator iterator = entrySet.iterator();
    while (iterator.hasNext()) {
        Object obj = iterator.next();
        Map.Entry entry = (Map.Entry) obj;
        System.out.println(entry.getKey()+"---------------->"+entry.getValue());
    }
}
```

```java
//TreeMap定制排序

@Test
void testTreeMap1() {
    TreeMap map = new TreeMap(new Comparator() {
        @Override
        public int compare(Object o1, Object o2) {
            if(o1 instanceof User && o2 instanceof User){
                User u1 = (User) o1;
                User u2 = (User) o2;
                return Integer.compare(u1.getAge(),u2.getAge());
            }
            throw new RuntimeException("输入的类型不匹配");
        }
    });
    User u1 = new User("Tom", 12);
    User u2 = new User("Jerry", 19);
    User u3 = new User("Bob", 13);
    User u4 = new User("TK", 18);
    map.put(u1,12);
    map.put(u2,89);
    map.put(u3,48);
    map.put(u4,34);
    Set entrySet = map.entrySet();
    Iterator iterator = entrySet.iterator();
    while (iterator.hasNext()) {
        Object obj = iterator.next();
        Map.Entry entry = (Map.Entry) obj;
        System.out.println(entry.getKey()+"---------------->"+entry.getValue());
    }
}
```



### Properties

> 用来处理配置文件, key和value都是String类型

```java
@Test
void testProperties() {
    FileInputStream fis = null;
    try {
        Properties properties = new Properties();

        fis = new FileInputStream("F:\\CodeHub\\JavaWorkSpace\\JavaLearn\\Collection\\src\\com\\broky\\Collection\\jdbc.properties");
        properties.load(fis);
        String name = properties.getProperty("name");
        String password = properties.getProperty("password");

        System.out.println("name = " + name + " , password = " + password);
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            fis.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



### Collections工具类

**说明**

1. Collections 是一个**操作 Set, List 和 Map** 等集合的工具类
2. Collections 中提供了一系列静态的方法对集合元素进行排序, 查询和修改等操作, 还提供了对集合对象设置不可变, 对集合对象实现同步控制等方法.

**面试题: Collection 和 Collections 的区别**

- Collection 是一个创建集合的接口

- Collections 是一个操作集合的工具类

**排序操作：**（均为static方法）

- reverse(List)：反转List 中元素的顺序
- shuffle(List)：对List集合元素进行随机排序
- sort(List)：根据元素的自然顺序对指定List 集合元素按升序排序
- sort(List，Comparator)：根据指定的Comparator 产生的顺序对List 集合元素进行排序
- swap(List，int，int)：将指定list 集合中的i处元素和j 处元素进行交换

**查找、替换**

- Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
- Object max(Collection，Comparator)：根据Comparator 指定的顺序，返回给定集合中的最大元素
- Object min(Collection)
- Object min(Collection，Comparator)
- intfrequency(Collection，Object)：返回指定集合中指定元素的出现次数
- void copy(List dest,Listsrc)：将src中的内容复制到dest中
- booleanreplaceAll(List list，Object oldVal，Object newVal)：使用新值替换List 对象的所有旧值

**demo**

```java
@Test
void testCollectons() {
    ArrayList list = new ArrayList();
    list.add(123);
    list.add(43);
    list.add(43);
    list.add(765);
    list.add(-97);
    list.add(0);
    System.out.println(list);

    Collections.reverse(list);
    System.out.println(list);

    Collections.shuffle(list);
    System.out.println(list);

    Collections.sort(list); //调用Integer的compareto()函数
    System.out.println(list);

    System.out.println(Collections.frequency(list, 43));

    //        ArrayList dest = new ArrayList(); 报异常
    //        ArrayList dest = new ArrayList(list.size()); //这么写, 是指定ArrayList中底层实现数组的大小,而不是ArrayList中size()的大小

    List dest = Arrays.asList(new Object[list.size()]); //new Object[list.size()]为长度为list.size(),值为null的object类型数组
    System.out.println(dest);
    Collections.copy(dest, list); //copy中dest.size()必须大于list.size()
    System.out.println(dest);
}
```



**Collections常用方法：同步控制**

Collections 类中提供了多个**synchronizedXxx()** 方法，该方法可使将指定集合包装成线程同步的集合，从而可以解决多线程并发访问集合时的线程安全问题

```java
@Test
void testSynchronized() {
    ArrayList list = new ArrayList();
    list.add(123);
    list.add(43);
    list.add(43);
    list.add(765);
    list.add(-97);
    list.add(0);
	
    //将list转换为线程安全的list1
    List list1 = Collections.synchronizedList(list);
    
}
```



### Enumeration(了解)

> Enumeration 接口是Iterator迭代器的“古老版本”


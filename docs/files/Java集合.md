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
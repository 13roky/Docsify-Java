# Java枚举类与注解


## 一、枚举类

> 类的对象只有有限个, 确定的. 我们称此类为枚举类.



**说明：**

1. 类的对象只有有限个，确定的。如：
   - 星期：Monday(星期一)、......、Sunday(星期天) 
   - 性别：Man(男)、Woman(女)  季节：Spring(春节)......Winter(冬天) 
   - 支付方式：Cash（现金）、WeChatPay（微信）、Alipay(支付宝)、BankCard(银 行卡)、CreditCard(信用卡) 
   - 就职状态：Busy、Free、Vocation、Dimission 
   - 订单状态：Nonpayment（未付款）、Paid（已付款）、Delivered（已发货）、 Return（退货）、Checked（已确认）Fulfilled（已配货）
   - 线程状态：创建、就绪、运行、阻塞、死亡
2. 当需要定义一组常量时，强烈建议使用枚举类。
3. 若枚举只有一个对象, 则可以作为一种单例模式的实现方式。



**枚举类的实现：**

1. JDK1.5之前需要自定义枚举类。
2. JDK 1.5 新增的 enum 关键字用于定义枚举类。



**枚举类的属性：**

1. 枚举类对象的属性不应允许被改动, 所以应该使用 private final 修饰。
2. 枚举类的使用 private final 修饰的属性应该在构造器中为其赋值。
3. 若枚举类显式的定义了带参数的构造器, 则在列出枚举值时也必须对应的 传入参数。





## 	① 自定义枚举类

> 通过自己写一个自定义的类来实现自定义枚举类。



**自定义枚举类的实现：**

1. 私有化类的构造器，保证不能在类的外部创建其对象。

2. 在类的内部创建枚举类的实例。声明为：public static final。

3. 对象如果有实例变量，应该声明为private final，并在构造器中初始化。

**Demo：**

```java
package com.broky.EnumClass;

/**
 * 自定义枚举类
 *
 * @author 13roky
 * @date 2021-05-13 17:16
 */
public class SeasonTest {
    public static void main(String[] args) {
        Season spring = Season.SPRING;
        System.out.println(spring);
    }
}

// 自定义枚举类
class Season{
    // 1. 声明 Season 对象的属性
    // final 不能使用初始化赋值, 可以手动 显式赋值, 构造器赋值, 代码块赋值
    // 现式赋值和代码块赋值 会导致创建当前类的不同对象时 他们的这些属性都是一样的
    // 构造器赋值 可以在实例化的时候设置属性
    private final String seasonName;
    private final String seasonDesc;

    // 2. 私有化类的构造器, 并给对象属性赋值
    private Season(String seasonName, String seasonDesc){
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    // 3. 提供当前枚举类的多个对象 : public static final 的
    public static Season SPRING = new Season("春天","春暖花开");
    public static Season SUMMER = new Season("夏天","夏日炎炎");
    public static Season AUTUMN = new Season("秋天","秋高气爽");
    public static Season WINTER = new Season("冬天","冰天雪地");

    // 4. 其它诉求1 : 获取枚举类对象的属性
    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }

    // 5. 其他诉求2 : 提供toString（）
    @Override
    public String toString() {
        return "Season{" + "seasonName='" + seasonName + '\'' + ", seasonDesc='" + seasonDesc + '\'' + '}';
    }
}
```





## 	② enum关键字定义枚举类

> 通过使用enum关键字，和一些简便的规则，更方便枚举类的创建



**说明：**

1. enum 枚举类是继承 java.lang.Enum 类的，所以其中如果不重写 toString 使用的是 java.lang.Enum 中的 toString，不会输出内存地址，而是会打印对象名



**enum 枚举类的实现：**

1. 使用 `enum` 声明类为枚举类。

2. 在枚举类的开头首先定义枚举类中所需要的对象。

   - 枚举类对实例化枚举类的对象做了简化

     只需要使用 对象名(参数···) 就可以完成实例化，如：

     `PRING("春天", "春暖花开"), WINTER("冬天", "冰天雪地");`

     多个对象用 “,” 隔开，最后一个以 “;” 结尾

     如果没有属性，可以去掉括号，如：

     `PRING, WINTER;`

3. 其余规则均与自定义枚举类相同。

   

**Demo：**

```java
package com.broky.EnumClass;

/**
 * @author 13roky
 * @date 2021-05-13 18:32
 */
public class SeasonTest1 {
    public static void main(String[] args) {
        Season1 spring = Season1.SPRING;
        System.out.println(spring);
        System.out.println(Season1.class.getSuperclass());
    }
}

enum Season1 {
    // 1. 提供当前枚举类的对象, 多个对象之间用","隔开, 末尾对象用";"结束
    SPRING("春天", "春暖花开"), 
    SUMMER("夏天", "夏日炎炎"), 
    AUTUMN("秋天", "秋高气爽"), 
    WINTER("冬天", "冰天雪地");

    // 2. 声明 Season 对象的属性 : private final 修饰
    private final String seasonName;
    private final String seasonDesc;

    // 3. 私有化类的构造器, 并给对象属性赋值
    private Season1(String seasonName, String seasonDesc) {
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    // 4. 其它诉求1 : 获取枚举类对象的属性
    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }

    // 5. 其他诉求2 : 提供toString（）
    // 不重写 toString 时, 如果 enum 类继承的是 Object 类, 那么打印时应该使用 Object 的 toString 打印的是地址值.
    // 但是经过实践得知, 打印出的不是地址值, 由 Season1.class.getSuperclass() 知, 其父类是 java.lang.Enum

    //    @Override
    //    public String toString() {
    //        return "Season{" + "seasonName='" + seasonName + '\'' + ", seasonDesc='" + seasonDesc + '\'' + '}';
    //    }
}
```





## 	③ enum 枚举类的方法

- **values() ：**返回枚举类型的对象数组。该方法可以很方便地遍历所有的 枚举值。
- **valueOf(String str) ：**可以把一个字符串转为对应的枚举类对象。要求字符 串必须是枚举类对象的“名字”。如不是，会有运行时异常：IllegalArgumentException。
- **toString()**：返回当前枚举类对象常量的名称。



**Demo：**（枚举类使用上面代码的枚举类Season1）

```java
package com.broky.EnumClass;

import java.util.Arrays;

/**
 * @author 13roky
 * @date 2021-05-13 18:32
 */
public class SeasonTest1 {
    public static void main(String[] args) {
        Season1 spring = Season1.SPRING;
        System.out.println(spring);
        System.out.println(Season1.class.getSuperclass());

        System.out.println("************************************");
        // values()方法：返回枚举类型的对象数组。该方法可以很方便地遍历所有的枚举值。
        Season1[] values = Season1.values();
        System.out.println(Arrays.toString(values));

        Thread.State[] values1 = Thread.State.values();
        System.out.println(Arrays.toString(values1));

        System.out.println("************************************");
        // valueOf(String str) ：返回枚举类中对象名是objName的对象。
        // 可以把一个字符串转为对应的枚举类对象。
        // 要求字符 串必须是枚举类对象的“名字”。如不是，会有运行时异常：IllegalArgumentException。
        Season1 winter1= Season1.valueOf("WINTER");
        System.out.println(winter1);

        System.out.println("************************************");

        // toString()：返回当前枚举类对象常量的名称。
        System.out.println(winter1.toString());
    }
}
```



## 	④ enum 枚举类实现接口

> enum 枚举类可以像正常类那样实现接口并重写接口中的方法
>
> 但是 enum 枚举类还有其独特的实现接口的方法,  接口类中的每个对象都可以独自重写实现接口的方法



**enum 对象特有的实现接口的方法 :**

- 对象名(构造器参数){ 需要重写的方法 }, 如:

```java
    AUTUMN("秋天", "秋高气爽"){
        @Override
        public void show() {
            System.out.println("秋高气爽");
        }
    },
    WINTER("冬天", "冰天雪地"){
        @Override
        public void show() {
            System.out.println("凛冬已至");
        }
    };
```



**Demo:** 

```java
package com.broky.EnumClass;

import org.junit.jupiter.api.Test;

import java.util.Arrays;

/**
 * @author 13roky
 * @date 2021-05-13 18:32
 */
public class SeasonTest1 {
    @Test
    public void test(){
        Season1 spring = Season1.SPRING;
        spring.show();
        Season1.SUMMER.show();
    }
}

interface info{
    void show();
}

enum Season1 implements info{
    // enum 独有的实现接口的方法
    // 1. 提供当前枚举类的对象, 多个对象之间用","隔开, 末尾对象用";"结束
    SPRING("春天", "春暖花开"){
        @Override
        public void show() {
            System.out.println("春天在哪里");
        }
    },
    SUMMER("夏天", "夏日炎炎"){
        @Override
        public void show() {
            System.out.println("夏天");
        }
    },
    AUTUMN("秋天", "秋高气爽"){
        @Override
        public void show() {
            System.out.println("秋高气爽");
        }
    },
    WINTER("冬天", "冰天雪地"){
        @Override
        public void show() {
            System.out.println("凛冬已至");
        }
    };

    // 2. 声明 Season 对象的属性 : private final 修饰
    private final String seasonName;
    private final String seasonDesc;

    // 3. 私有化类的构造器, 并给对象属性赋值
    private Season1(String seasonName, String seasonDesc) {
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    // 4. 其它诉求1 : 获取枚举类对象的属性
    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }

    @Override
    public void show() {
        System.out.println("这是一个季节");
    }

}
```



## 二、注解

- 从 JDK 5.0 开始, Java 增加了对元数据(MetaData) 的支持, 也就是 Annotation(注解)

- Annotation 其实就是代码里的特殊标记, 这些标记可以在编译, 类加 载, 运行时被读取, 并执行相应的处理。通过使用 Annotation, 程序员 可以在不改变原有逻辑的情况下, 在源文件中嵌入一些补充信息。代 码分析工具、开发工具和部署工具可以通过这些补充信息进行验证 或者进行部署。

- Annotation 可以像修饰符一样被使用, 可用于修饰包,类, 构造器, 方 法, 成员变量, 参数, 局部变量的声明, 这些信息被保存在 Annotation  的 “name=value” 对中。
- 在JavaSE中，注解的使用目的比较简单，例如标记过时的功能， 忽略警告等。在JavaEE/Android中注解占据了更重要的角色，例如 用来配置应用程序的任何切面，代替JavaEE旧版中所遗留的繁冗 代码和XML配置等。
- 未来的开发模式都是基于注解的，JPA是基于注解的，Spring2.5以 上都是基于注解的，Hibernate3.x以后也是基于注解的，现在的 Struts2有一部分也是基于注解的了，注解是一种趋势，一定程度上 可以说：**框架 = 注解 + 反射 + 设计模式**。

- 使用 Annotation 时要在其前面增加 @ 符号, 并把该 Annotation 当成 一个修饰符使用。用于修饰它支持的程序元素

## 	① 生成文档相关注解

**用法：**

- @author 标明开发该类模块的作者，多个作者之间使用,分割 
- @version 标明该类模块的版本 
- @see 参考转向，也就是相关主题 
- @since 从哪个版本开始增加的 
- @param 对方法中某参数的说明，如果没有参数就不能写 
- @return 对方法返回值的说明，如果方法的返回值类型是void就不能写 
- @exception 对方法可能抛出的异常进行说明 ，如果方法没有用throws显式抛出的异常就不能写 

**说明：**

- @param @return 和 @exception 这三个标记都是只用于方法的。 
- @param的格式要求：@param 形参名 形参类型 形参说明 
- @return 的格式要求：@return 返回值类型 返回值说明 
- @exception的格式要求：@exception 异常类型 异常说明 
- @param和@exception可以并列多个

**Demo：**

```java
package com.broky.EnumClass;

/**
 * @author 13roky
 * @version 1.0
 * @see Math.java
 */
public class JavadocTest {
    /**
     * 程序的主方法，程序的入口
     *
     * @param args String[] 命令行参数
     */
    public static void main(String[] args) {
    }

    /**
     * 求圆面积的方法
     *
     * @param radius double 半径值
     * @return double 圆的面积
     */
    public static double getArea(double radius) {
        return Math.PI * radius * radius;
    }
}
```



## 	②注解在编译时进行格式检查

> 编译时，会强制校验注解处的方法是否符合注解，如果不符合会报错

**JDK内置的三个基本注解：**

- @Override: 限定重写父类方法, 该注解只能用于方法 
- @Deprecated: 用于表示所修饰的元素(类, 方法, 属性等·已过时。通常是因为 所修饰的结构危险或存在更好的选择 
- @SuppressWarnings: 抑制编译器警告，消除某段代码在编译器中的警告

**Demo：**

```java
package com.broky.EnumClass;

public class AnnotationTest{
    public static void main(String[] args) {
        @SuppressWarnings("unused")
        int a = 10;
    }
    @Deprecated
    public void print(){
        System.out.println("过时的方法");
    }
    @Override
    public String toString() {
        return "重写的toString方法()";
    }
}
```



## 	③注解跟踪代码的依赖性，实现替代配置文件功能

-  Servlet3.0提供了注解(annotation),使得不再需要在web.xml文件中进行Servlet的部署。

```java
import java.io.IOException;

@WebServlet("/login")
public class LoginServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
}
```

```xml
<servlet>
<servlet-name>LoginServlet</servlet-name>
<servlet-class>com.servlet.LoginServlet</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>LoginServlet</servlet-name>
<url-pattern>/login</url-pattern>
</servlet-mapping>
```



-  spring框架中关于“事务”的管理

```java
 @Transactional(propagation = Propagation.REQUIRES_NEW, isolation = Isolation.READ_COMMITTED, readOnly = false, timeout = 3) public void buyBook(String username,String isbn){
        //1.查询书的单价
        int price=bookShopDao.findBookPriceByIsbn(isbn);
        //2. 更新库存
        bookShopDao.updateBookStock(isbn);
        //3. 更新用户的余额
        bookShopDao.updateUserAccount(username,price);
        }
```

```xml
<!-- 配置事务属性 -->
<tx:advice transaction-manager="dataSourceTransactionManager" id="txAdvice">
<tx:attributes>
<!-- 配置每个方法使用的事务属性 -->
<tx:method name="buyBook" propagation="REQUIRES_NEW"
isolation="READ_COMMITTED" read-only="false" timeout="3" />
</tx:attributes>
</tx:advice>
```



## 	④ 自定义注解



**说明：**

- 定义新的 Annotation 类型使用 @interface 关键字 
- 自定义注解自动继承了java.lang.annotation.Annotation接口 
-  Annotation 的成员变量在 Annotation 定义中以无参数方法的形式来声明。其 方法名和返回值定义了该成员的名字和类型。我们称为配置参数。类型只能 是八种基本数据类型、String类型、Class类型、enum类型、Annotation类型、 以上所有类型的数组。 
- 可以在定义 Annotation 的成员变量时为其指定初始值, 指定成员变量的初始 值可使用 default 关键字 
- 如果只有一个参数成员，建议使用参数名为value 
-  如果定义的注解含有配置参数，那么使用时必须指定参数值，除非它有默认 值。格式是“参数名 = 参数值” ，如果只有一个参数成员，且名称为value， 可以省略“value=” 
- 没有成员定义的 Annotation 称为标记; 包含成员变量的 Annotation 称为元数 据 Annotation 注意：自定义注解必须配上注解的信息处理流程才有意义。

**注意：自定义注解必须配上注解的信息处理流程才有意义。**（使用反射实现）

**Demo:**

```java
package com.broky.EnumClass;

/**
 * @author 13roky
 * @date 2021-05-14 8:36
 */
public @interface MyAnnotation {
    String value() default "test";
}

package com.broky.EnumClass;

/**
 * @author 13roky
 * @date 2021-05-14 8:16
 */
public class AnnotationTest {
    @MyAnnotation()
    void test(){
        
    }
}
```



## 	⑤ jdk提供的4种元注解



**说明：**

- JDK 的元 Annotation 用于修饰其他 Annotation 定义
- JDK5.0提供了4个标准的meta-annotation类型，分别是：
  - Retention 
  - Target 
  - Documented 
  - Inherited



**元注解说明：**

- **@Retention:** 只能用于修饰一个 Annotation 定义, 用于指定该 Annotation 的生命 周期, @Rentention 包含一个 RetentionPolicy 类型的成员变量, 使用 @Rentention 时必须为该 value 成员变量指定值:

  - **RetentionPolicy.SOURCE: **在源文件中有效（即源文件保留），编译器直接丢弃这种策略的 注释

  - **RetentionPolicy.CLASS（默认）: **在class文件中有效（即class保留） ， 当运行 Java 程序时, JVM  不会保留注解。 这是默认值

  - **RetentionPolicy.RUNTIME:** 在运行时有效（即运行时保留），当运行 Java 程序时, JVM 会 保留注释。程序可以通过反射获取该注释

    只有声明为RUNTIME生命周期的注解，才能通过反射获取。

```java
public enum RetentionPolicy{
SOURCE,
CLASS,
RUNTIME
}

@Retention(RetentionPolicy.SOURCE)
@interface MyAnnotation1{ }
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation2{ }

```

- @Target: 用于修饰 Annotation 定义, 用于指定被修饰的 Annotation 能用于 修饰哪些程序元素。 @Target 也包含一个名为 value 的成员变量。

![](https://i.vgy.me/JvS9L6.png)

```java
@Target({FIELD,METHOD,TYPE})
@Retention(RetentionPolicy.SOURCE)
@interface MyAnnotation1{ }
```



- @Documented: 用于指定被该元 Annotation 修饰的 Annotation 类将被 javadoc 工具提取成文档。默认情况下，javadoc是不包括注解的。 
  - **定义为Documented的注解必须设置Retention值为RUNTIME。**
- @Inherited: 被它修饰的 Annotation 将具有继承性。如果某个类使用了被 @Inherited 修饰的 Annotation, 则其子类将自动具有该注解。 
  - 比如：如果把标有@Inherited注解的自定义的注解标注在类级别上，子类则可以 继承父类类级别的注解 
  - 实际应用中，使用较少

**元数据的理解：**

String name ="13roky"

这个数据中13roky最为重要，String 和 name 都是对其进行修饰，那么String 和 name 就可以叫做元数据：用于修饰数据的数据



## ⑥ JKD8 新特性：可重复注解

**JDK8 之前重复注解的实现：**

JDK8 之前如果要同一位置加多个相同注解，需要使用数组来添加。

```java
package com.broky.EnumClass;

import java.lang.annotation.*;

import static java.lang.annotation.ElementType.*;

/**
 * @author 13roky
 * @date 2021-05-14 8:36
 */

@Retention(RetentionPolicy.SOURCE)
@Target({FIELD, METHOD})
public @interface MyAnnotation {
    String[] value();
}
```

```java
package com.broky.EnumClass;

/**
 * @author 13roky
 * @date 2021-05-14 8:16
 */
public class AnnotationTest {
    @MyAnnotation({"123","456"})
    void test(){

    }
}
```



**JKD8 新特性：可重复注解：**

- 在 MyAnnotation 上声明 @Repeatable，成员值为 Annotations.class
- MyAnnotation 的 Targe , Inherited 和 Retention 与Annotations相同。

**Demo：**

```java
package com.broky.EnumClass;

/**
 * @author 13roky
 * @date 2021-05-14 8:16
 */
public class AnnotationTest {
    @MyAnnotation()
    @MyAnnotation()
    void test(){

    }
}
```

```java
package com.broky.EnumClass;

import java.lang.annotation.*;

import static java.lang.annotation.ElementType.*;

/**
 * @author 13roky
 * @date 2021-05-14 8:36
 */

@Repeatable(MyAnnotations.class)
@Retention(RetentionPolicy.SOURCE)
@Target({FIELD, METHOD})
public @interface MyAnnotation {
    String value() default "test";
}
```

```java
package com.broky.EnumClass;

import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import static java.lang.annotation.ElementType.FIELD;
import static java.lang.annotation.ElementType.METHOD;

/**
 * @author 13roky
 * @date 2021-05-14 9:43
 */
@Retention(RetentionPolicy.SOURCE)
@Target({FIELD, METHOD})
public @interface MyAnnotations {
    MyAnnotation[] value();
}

```



## 	⑦ JDK8 新特性：类型注解

> 可以理解为，类型注解就是对元注解@Target，新增的两个参数类型
>
> TYPE_PARAMETER, TYPE_USE



**说明：**

- 在Java 8之前，注解只能是在声明的地方所使用，Java8开始，**注解可以应用 在任何地方**。
  - **ElementType.TYPE_PARAMETER** 表示该注解能写在类型变量的声明语 句中（如：泛型声明）。
  - **ElementType.TYPE_USE** 表示该注解能写在使用类型的任何语句中。



**Demo:**

```java
// 在自定义注解的@Target中加入参数TYPE_PARAMETER
class Generic<@MyAnnotation T>{
    // 在自定义注解的@Target中加入参数TYPE_USE
    public void show() throws @MyAnnotation RuntimeException{
        ArrayList<@MyAnnotation String> list = new ArrayList<>();

        int num = (@MyAnnotation int) 10L;
    }
}
```

```java
package com.broky.EnumClass;

import java.lang.annotation.*;

import static java.lang.annotation.ElementType.*;

/**
 * @author 13roky
 * @date 2021-05-14 8:36
 */

@Repeatable(MyAnnotations.class)
@Retention(RetentionPolicy.SOURCE)
@Target({FIELD, METHOD, TYPE_PARAMETER,TYPE_USE})
public @interface MyAnnotation {
    String value() default "test";
}
```




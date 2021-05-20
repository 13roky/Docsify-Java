## 多态

### 父类引用指向子类对象

> Father f1 = new Son();



**对于方法的调用**

- f1 可以调用父类独有的方法
- f1 可以调用子类重写过的父类的方法
- f1 无法直接调用子类特有的方法eat(), f1 可以通过强制类型转换调用子类特有的方法eat()

**对象能使用哪些方法主要看左边声明的引用类型**
**声明为父类的引用类型，无法调用子类独有的方法**
**但是如果子类重写了父类的方法，那么在父类的引用类型的对象去调用方法时，会优先调用子类重写的方法**



**对于 instanceof 的使用**

- instanceof 判断时和对象的指向类型有关，与对象的引用类型无关
- 如有 Father f1 = new Son();  System.out.println("f1 instanceof Son : " + (f1 instanceof Son)), 那么 f1 Instanceof 的时候是以f1的指向类型Son有关，与其引用类型Father无关
- 当判断的类型f1本类型或f1向上的父类时 则 返回 true
- 可以理解为判断 f1 的指向类型 f1 is Son，f1 is Father, f1 is Object 是否为 true





```java
public class Father {
    public void run(){
        System.out.println("执行父类方法");
    }

    public void name() {
        System.out.println("执行父类方法");
    }
}
```

```java
class Son extends Father{
    public void run(){
        System.out.println("执行子类方法");
    }

    public void eat(){
        System.out.println("执行子类方法");
    }
    
}
```

```java
public class App {

    static void test1(){
        //父类引用指向子类对象
        Father f1 = new Son();
        // 
        f1.name();
        //f1调用子类重写过的父类的方法
        f1.run();
        //f1无法直接调用子类特有的方法eat()
        
        //f1可以通过强制类型转换调用子类特有的方法
        ((Son)f1).eat();

        /**
         * 对象能使用哪些方法主要看左边声明的引用类型
         * 声明为父类的引用类型，无法调用子类独有的方法
         * 但是如果子类重写了父类的方法，那么在父类的引用类型的对象去调用方法时，会优先调用子类重写的方法
         */
    }

    static void test2(){
        Father father = new Father();
        Son son = new Son();
        Father f1 = new Son();

        System.out.println("father instanceof Son : " + (father instanceof Son)); //false
        System.out.println("father instanceof Father : " + (father instanceof Father)); //true
        System.out.println("father instanceof Object : " + (father instanceof Object)); //true
        System.out.println("f1 instanceof Son : " + (f1 instanceof Son));
        System.out.println("f1 instanceof Father : " + (f1 instanceof Father));
        System.out.println("f1 instanceof Object : " + (f1 instanceof Object));


        /** 
         * instanceof 为true的条件
         * instanceof 判断时和对象的指向类型有关，与对象的引用类型无关
         * 如有 Father f1 = new Son();  System.out.println("f1 instanceof Son : " + (f1 instanceof Son));
         * 那么 f1 Instanceof 的时候是以f1的指向类型Son有关，与其引用类型Father无关
         * 当判断的类型f1本类型或f1向上的父类时 则 返回 true
         * 可以理解为判断 f1 的指向类型 f1 is Son，f1 is Father, f1 is Object 是否为 true
         * 
        */
    }
    public static void main(String[] args) {
        // test1();
        test2();

    }
}
```


## 1.Java 程序基础结构

```java
/**
 * 可以用来自动创建文档的注释
 */
public class Hello {
    public static void main(String[] args) {
        // 向屏幕输出文本:
        System.out.println("Hello, world!");
        /* 多行注释开始
        注释内容
        注释结束 */
    }
}
```

java 是面向对象的编程语言，因此一个程序的基本单元就是 class, class 是关键字，这里定义的 class 名字就是 Hello。

另外，在 Java 中，对于类名也有一定要求：

- 类名必须以英文字母开头，后接字母，数字和下划线的组合
- 习惯以大写字母开头

上面的 public 是访问修饰符，表示这个 class 是公开的。如果不写 public，也能够编译，但这个类在命令行将无法执行。

在 class 内部可以定义若干的方法：

```java
public class Hello {
    public static void main(String[] args) { // 方法名是main
        // 方法代码...
    } // 方法定义结束
}
```

我们可以在方法中定义一组执行语句，方法内部的代码将会被依此顺序执行。如上代码中方法名是 main,返回值是 void,表示没有任何返回值。

值得注意的是，public 除了可以修饰 class 外，还可以修饰方法。而关键字 static 是另外一个修饰符，他表示静态方法。Java 入口程序规定的方法必须是静态方法，方法名必须是 main,括号内的仓鼠必须是 String 数组。方法名也有命名规则，和 class 差不多，不同的是首字母小写。

在方法内部，语句才是真正执行的代码，Java 的每一句语句都必须以分号结束。而注释也是 Java 中很重要的一环，在 Java 中一共分为三种注释：

1. 单行注释
2. 多行注释
3. 特殊多行注释，需要写在类和方法的定义处，可以用于自动创建文档

```java
/**
 * 这是特殊多行注释可以用来自动创建文档的注释
 *
 * @auther srtian
 */
public class Hello {
    public static void main(String[] args) {
        /*
    这是多行注释
    blablabla...
    这也是注释
    */
    } // 这是单行注释
}
```

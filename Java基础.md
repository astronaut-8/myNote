# Java基础

---

## 命令行参数

Java程序的入口是`main`方法，而`main`方法可以接受一个命令行参数，它是一个`String[]`数组。

这个命令行参数由JVM接收用户输入并传给`main`方法：

```
public class Main {
    public static void main(String[] args) {
        for (String arg : args) {
            System.out.println(arg);
        }
    }
}
```

我们可以利用接收到的命令行参数，根据不同的参数执行不同的代码。例如，实现一个`-version`参数，打印程序版本号：

```
public class Main {
    public static void main(String[] args) {
        for (String arg : args) {
            if ("-version".equals(arg)) {
                System.out.println("v 1.0");
                break;
            }
        }
    }
}
```

上面这个程序必须在命令行执行，我们先编译它：

```
$ javac Main.java
```

然后，执行的时候，给它传递一个`-version`参数：

```
$ java Main -version
v 1.0
```

这样，程序就可以根据传入的命令行参数，作出不同的响应。

## 注释与关键字

### 注释：

#### 单行注释 

格式：//巴拉巴拉

#### 多行注释

格式：/*  巴拉巴拉  */

**注意：**注释不要嵌套。

### 关键字

被赋予特定含义的英语单词

#### **特点**：

==1.关键字的字母全部小写。==

==2.代码编辑器中针对关键字有特殊的颜色标记，非常直观。==

#### class：

用于创建/定义一个类。==类是Java最基本的组成单元。==

**类名要和文件名保持一致**

```java
public class HellowWorld{
    //HellowWorld是类的名字
}
```

Javabean类：用来描述一类事物的类。比如：Student，Teacher，Dog，Cat等。

测试类：用来检查其他类是否书写正确，带有main方法的类，是程序的入口。

工具类：不是用来描述一些事物的，而是帮我们做一些事情的类。

工具类注意点

- 类名见名知意（根据作用）
- 私有化构造方法（防止创建它的对象，它描述的不是事物，创建它的对象没有意义）
- **方法定义为静态（方便调用）**

#### static：

==共享==

**static是==静态的==的意思，是一个修饰符，就像是一个形容词，是用来形容类，变量，方法的。**

**static修饰变量，这个变量就变成了静态变量，修饰方法这个方法就成了静态方法。**

**static关键字方便在没有创建对象的情况下来进行调用（方法/变量）**

作用：

###### **1.修饰变量**

**被static修饰的成员变量，叫做静态变量。使用static关键字修饰的变量可以通过  类名.变量名  直接访问**

不使用static关键字访问对象的属性

![image-20221117154656694](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221117154656694.png)

使用static关键字访问对象的属性

![image-20221117154825234](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221117154825234.png)

**==注意：如果一个类的成员变量被`static`修饰了，那么所有该类的对象都共享这个变量。无论这个类实例化多少对象，它的静态变量均相同。==**

在一个`class`中定义的字段，我们称之为实例字段。实例字段的特点是，每个实例都有独立的字段，各个实例的同名字段互不影响。

还有一种字段，是用`static`修饰的字段，称为静态字段：`static field`。

实例字段在每个实例中都有自己的一个独立“空间”，但是静态字段只有一个共享“空间”，所有实例都会共享该字段。举个例子：

```java
class Person {
    public String name;
    public int age;
    // 定义静态字段number:
    public static int number;
}
```

我们来看看下面的代码：

```java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person("Xiao Ming", 12);
        Person hong = new Person("Xiao Hong", 15);
        ming.number = 88;
        System.out.println(hong.number);
        hong.number = 99;
        System.out.println(ming.number);
    }
}

class Person {
    public String name;
    public int age;

    public static int number;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

对于静态字段，无论修改哪个实例的静态字段，效果都是一样的：所有实例的静态字段都被修改了，原因是静态字段并不属于实例：

```ascii
        ┌──────────────────┐
ming ──▶│Person instance   │
        ├──────────────────┤
        │name = "Xiao Ming"│
        │age = 12          │
        │number ───────────┼──┐    ┌─────────────┐
        └──────────────────┘  │    │Person class │
                              │    ├─────────────┤
                              ├───▶│number = 99  │
        ┌──────────────────┐  │    └─────────────┘
hong ──▶│Person instance   │  │
        ├──────────────────┤  │
        │name = "Xiao Hong"│  │
        │age = 15          │  │
        │number ───────────┼──┘
        └──────────────────┘
```

虽然实例可以访问静态字段，但是它们指向的其实都是`Person class`的静态字段。所以，所有实例共享一个静态字段。

因此，不推荐用`实例变量.静态字段`去访问静态字段，因为在Java程序中，实例对象并没有静态字段。在代码中，实例对象能访问静态字段只是因为编译器可以根据实例类型自动转换为`类名.静态字段`来访问静态对象。

推荐用类名来访问静态字段。可以把静态字段理解为描述`class`本身的字段（非实例字段）。对于上面的代码，更好的写法是：

```
Person.number = 99;
System.out.println(Person.number);
```

###### **2.修饰方法**

特点：

1. 多在测试类和工具类中
2. javabean类中很少会用

用`static`关键字修饰的方法叫做静态方法。静态方法我们已经用过，它有一个特点相信你已经很熟悉，那就是**不需要创建对象**就可以**直接使用**。==类名调用：==类名.方法名

![image-20221117155233234](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221117155233234.png)

**注意：**

- ==静态方法只能直接访问静态变量和静态方法，但是不可以直接访问非静态变量，如果一定要访问的话，可以去构建一个当前类的对象，因为非静态成员变量只能通过对象去访问。==

- ==非静态方法可以访问静态变量或者静态方法，也可以访问非静态的成员变量和非静态的成员方法==

- ==静态方法中没有this关键字，因为this代表当前对象，而静态方法中是可以不用声明对象的。==

###### **3.静态代码块**

![image-20221117160345793](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221117160345793.png)

输出结果：

```
我被调用了
```

上图中`static{ }`就是一个静态代码块。

我们在`main`方法中没有编写任何代码，可是运行的时候，程序还是会输出`我被调用了`。**==静态代码块是不需要依赖`main`方法就可以独立运行的。==**

关于静态代码块你只需要记住一句话：**在类被加载的时候运行且只运行一次**。

静态代码块中变量和方法的调用也遵守我们之前所说的规则，即**只能直接调用静态的属性和方法**。

###### **4.接口的静态字段**

因为`interface`是一个纯抽象类，所以它不能定义实例字段。但是，`interface`是可以有静态字段的，并且静态字段必须为`final`类型：

```
public interface Person {
    public static final int MALE = 1;
    public static final int FEMALE = 2;
}
```

实际上，因为`interface`的字段只能是`public static final`类型，所以我们可以把这些修饰符都去掉，上述代码可以简写为：

```
public interface Person {
    // 编译器会自动加上public statc final:
    int MALE = 1;
    int FEMALE = 2;
}
```

编译器会自动把该字段变为`public static final`类型。

---

---

---

## 字面量

**定义: 数据在程序中的书写格式**

#### 字面量类型

![](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221108224733428.png)

#### 常见数据在代码中的书写

```java
//整数
System.out.println(666);
//小数
System.out.println(3.14);
//字符串
System.out.println("王铭杰");
//字符
System.out.println('男');
//布尔类型
System.out.println(true);
System.out.println(flase);
//空类型
//null是不能直接打印的，只能以字符串的形式打印
System.out.println("null");
```

**注意：==null是不能直接打印的==，只能以字符串的形式打印**

#### 特殊字符类型

- '\t'  制表符

  在打印的时候，把前面字符串的长度补齐到8，或者8的整数倍。最少补一个空格，最多补8个空格。

  ![](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221109161748727.png)

  

在"abcd"后加四个空格

![](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221109161908742.png)

**使输出的内容对其，方便阅读。**

---

---

---

## 计算机的存储规则

**在计算机中，任意数据都是以二进制的形式来储存的。**

![image-20221121193246749](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221121193246749.png)

ASCII码表

![image-20221121194712516](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221121194712516.png)

![image-20221121195029164](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221121195029164.png)

![image-20221121195405773](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221121195405773.png)



![image-20221121195520973](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221121195520973.png)

三原色小结
1.计算机中的颜色采用光学三原色。
2.分别为：红，绿，蓝。也称之为RGB
3.可以写成十进制形式。（255,255,255)
4.也可以写成十六进制形式。（FFFFFF)

![image-20221121200041068](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221121200041068.png)

---

---

---

## 变量

​		变量的使用场景：当某个数据经常发生改变时，我们可以用变量来储存数据。当数据变化时，只要修改变量里面记录的值即可。

#### 变量的定义格式

**数据类型 变量名 = 数据值;**

- 数据类型：为空间中储存的数据，加入类型【限制】整数？小数？
- 变量名：为空间起的名字。（方便以后使用）
- 等号：赋值。把右边的数据值赋值给左边的变量。
- 数据值：存在空间里面的数值。

#### 变量的使用方法

**注意：参与计算。**

![](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221109163619286.png)

#### 变量的注意事项

- 只能存一个值
- 变量名不允许重复定义
- 一条语句可以定义多个变量
- 变量在使用之前一定要进行赋值
- 变量的作用域范围

**注意：在一条语句中可以定义多个变量**。

```java
int d=100,e=200,f=300
```

**在定义变量的时候，直接赋值。**

---

---

---

## 数据类型

**数据类型的分类**：基本数据类型

​								引用数据类型（数据和面向对象时学）

#### 基本数据类型

![image-20221109165729773](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221109165729773.png)

**整数默认使用：int**

==**浮点数默认使用：double**==

**注意：**

**1.定义==long类型==的变量时，在数据值的后面需要==加一个L作为后缀。==**

```java
long a=9999999999L;
System.out.println(a);
```

**2.定义==float类型==的变量时，在数据值的后面需要==加一个F (f也可)作为后缀。==**

```java
float f1 = 3.14f;
float f2 = 3.14e38f; // 科学计数法表示的3.14x10^38
float f3 = 1.0; // 错误：不带f结尾的是double类型，不能赋值给float

double d = 1.79e308;
double d2 = -1.79e308;
double d3 = 4.9e-324; // 科学计数法表示的4.9x10^-324
```

**3.布尔类型**

布尔类型`boolean`只有`true`和`false`两个值，布尔类型总是关系运算的计算结果：

```java
boolean b1 = true;
boolean b2 = false;
boolean isGreater = 5 > 3; // 计算结果为true
int age = 12;
boolean isAdult = age >= 18; // 计算结果为false
```

#### 引用数据类型

类，接口，数组，String

###### **字符串类型**

和`char`类型不同，字符串类型`String`是引用类型，我们用双引号`"..."`表示字符串。一个字符串可以存储0个到任意个字符：

```
String s = ""; // 空字符串，包含0个字符
String s1 = "A"; // 包含一个字符
String s2 = "ABC"; // 包含3个字符
String s3 = "中文 ABC"; // 包含6个字符，其中有一个空格
```

因为字符串使用双引号`"..."`表示开始和结束，那如果字符串本身恰好包含一个`"`字符怎么表示？例如，`"abc"xyz"`，编译器就无法判断中间的引号究竟是字符串的一部分还是表示字符串结束。这个时候，我们需要借助转义字符`\`：

```
String s = "abc\"xyz"; // 包含7个字符: a, b, c, ", x, y, z
```

因为`\`是转义字符，所以，两个`\\`表示一个`\`字符：

```
String s = "abc\\xyz"; // 包含7个字符: a, b, c, \, x, y, z
```

常见的转义字符包括：

- `\"` 表示字符`"`
- `\'` 表示字符`'`
- `\\` 表示字符`\`
- `\n` 表示换行符
- `\r` 表示回车符
- `\t` 表示Tab
- `\u####` 表示一个Unicode编码的字符

例如：

```
String s = "ABC\n\u4e2d\u6587"; // 包含6个字符: A, B, C, 换行符, 中, 文
```

###### **字符串连接**

Java的编译器对字符串做了特殊照顾，可以使用`+`连接任意字符串和其他数据类型，这样极大地方便了字符串的处理。例如：

```
// 字符串连接
public class Main {
    public static void main(String[] args) {
        String s1 = "Hello";
        String s2 = "world";
        String s = s1 + " " + s2 + "!";
        System.out.println(s);
    }
}
```

如果用`+`连接字符串和其他数据类型，会将其他数据类型先自动转型为字符串，再连接：

```
// 字符串连接
public class Main {
    public static void main(String[] args) {
        int age = 25;
        String s = "age is " + age;
        System.out.println(s);
    }
}
```

###### **多行字符串**

如果我们要表示多行字符串，使用+号连接会非常不方便：

```
String s = "first line \n"
         + "second line \n"
         + "end";
```

从Java 13开始，字符串可以用`"""..."""`表示多行字符串（Text Blocks）了。举个例子：

```
// 多行字符串
public class Main {
    public static void main(String[] args) {
        String s = """
                   SELECT * FROM
                     users
                   WHERE id > 100
                   ORDER BY name DESC
                   """;
        System.out.println(s);
    }
}
```

上述多行字符串实际上是5行，在最后一个`DESC`后面还有一个`\n`。如果我们不想在字符串末尾加一个`\n`，就需要这么写：

```
String s = """ 
           SELECT * FROM
             users
           WHERE id > 100
           ORDER BY name DESC""";
```

还需要注意到，多行字符串前面共同的空格会被去掉，即：

```
String s = """
...........SELECT * FROM
...........  users
...........WHERE id > 100
...........ORDER BY name DESC
...........""";
```

用`.`标注的空格都会被去掉。

如果多行字符串的排版不规则，那么，去掉的空格就会变成这样：

```
String s = """
.........  SELECT * FROM
.........    users
.........WHERE id > 100
.........  ORDER BY name DESC
.........  """;
```

即总是以最短的行首空格为基准。

###### **不可变特性**

Java的字符串除了是一个引用类型外，还有个重要特点，就是字符串不可变。考察以下代码：

```
// 字符串不可变
public class Main {
    public static void main(String[] args) {
        String s = "hello";
        System.out.println(s); // 显示 hello
        s = "world";
        System.out.println(s); // 显示 world
    }
}
```

观察执行结果，难道字符串`s`变了吗？其实变的不是字符串，而是变量`s`的“指向”。

执行`String s = "hello";`时，JVM虚拟机先创建字符串`"hello"`，然后，把字符串变量`s`指向它：

```ascii
      s
      │
      ▼
┌───┬───────────┬───┐
│   │  "hello"  │   │
└───┴───────────┴───┘
```

紧接着，执行`s = "world";`时，JVM虚拟机先创建字符串`"world"`，然后，把字符串变量`s`指向它：

```ascii
      s ──────────────┐
                      │
                      ▼
┌───┬───────────┬───┬───────────┬───┐
│   │  "hello"  │   │  "world"  │   │
└───┴───────────┴───┴───────────┴───┘
```

原来的字符串`"hello"`还在，只是我们无法通过变量`s`访问它而已。因此，字符串的不可变是指字符串内容不可变。至于变量，可以一会指向字符串`"hello"`，一会指向字符串`"world"`。

理解了引用类型的“指向”后，试解释下面的代码输出：

```
// 字符串不可变
public class Main {
    public static void main(String[] args) {
        String s = "hello";
        String t = s;
        s = "world";
        System.out.println(t); // t是"hello"还是"world"?
    }
}
```

###### **空值null**

引用类型的变量可以指向一个空值`null`，它表示不存在，即该变量不指向任何对象。例如：

```
String s1 = null; // s1是null
String s2 = s1; // s2也是null
String s3 = ""; // s3指向空字符串，不是null
```

注意要区分空值`null`和空字符串`""`，空字符串是一个有效的字符串对象，它不等于`null`。

###### 判断引用类型相等

在Java中，判断值类型的变量是否相等，可以使用`==`运算符。但是，判断引用类型的变量是否相等，`==`表示“引用是否相等”，或者说，是否指向同一个对象。例如，下面的两个String类型，它们的内容是相同的，但是，分别指向不同的对象，用`==`判断，结果为`false`：

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "HELLO".toLowerCase();
        System.out.println(s1);
        System.out.println(s2);
        if (s1 == s2) {
            System.out.println("s1 == s2");
        } else {
            System.out.println("s1 != s2");
        }
    }
}
```

**要判断引用类型的变量内容是否相等，必须使用`equals()`方法：**

```java
public class Main {
    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "HELLO".toLowerCase();
        System.out.println(s1);
        System.out.println(s2);
        if (s1.equals(s2)) {
            System.out.println("s1 equals s2");
        } else {
            System.out.println("s1 not equals s2");
        }
    }
}
```

**注意：执行语句`s1.equals(s2)`时，如果变量`s1`为`null`，会报`NullPointerException`：**

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        String s1 = null;
        if (s1.equals("hello")) {
            System.out.println("hello");
        }
    }
}
```

要避免`NullPointerException`错误，可以利用短路运算符`&&`：

```java
// 条件判断
public class Main {
    public static void main(String[] args) {
        String s1 = null;
        if (s1 != null && s1.equals("hello")) {
            System.out.println("hello");
        }
    }
}
```

还可以把一定不是`null`的对象`"hello"`放到前面：例如：`if ("hello".equals(s)) { ... }`。

---

---

---

## 标识符

**定义：给类，方法，变量等起的名字**

#### 标识符命名规则---硬性要求

- ==由数字、字母、下划线、和美元符（$）组成==
- ==不能以数字开头==
- ==不能是关键字==
- ==区分大小写==

#### 标识符命名规则---软性建议

**小驼峰命名法**

规范：

1.标识符是一个单词的时候，全部小写。

tip ：name

2.标识符由多个单词组成的时候，第一个单词首字母小写，第二个单词首字母大写。

tip : firstName

**大驼峰命名法**

规范：

1.标识符是一个单词的时候，首字母大写。

tip ：Name

2.标识符由多个单词组成的时候，每个单词的首字母大写。

tip ：FirstName

---

---

---

## 输入和输出

**Java帮我们写好一个类叫==Scanner==，这个类就可以接收键盘输入的数字。**

###### 键盘输入

![image-20221109173459174](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221109173459174.png)

```java
//导包，注意写在类定义的上面
import java.util.Scanner;
public class ScannerDemo1{
    public static void main(string[] args){
        //创建对象，准备用Scanner这个类
        Scanner sc = new Scanner(System.in);
        //用i接收键盘录入的数据
        int i = sc.nextInt();
        //输出i
        System.out.println(i);  
    }
}

```

首先，我们通过`import`语句导入`java.util.Scanner`，`import`是导入某个类的语句，必须放到Java源代码的开头，后面我们在Java的`package`中会详细讲解如何使用`import`。

然后，创建`Scanner`对象并传入`System.in`。`System.out`代表标准输出流，而`System.in`代表标准输入流。直接使用`System.in`读取用户输入虽然是可以的，但需要更复杂的代码，而通过`Scanner`就可以简化后续的代码。

**有了`Scanner`对象后，要读取用户输入的字符串，使用`scanner.nextLine()`，要读取用户输入的整数，使用`scanner.nextInt()`。**`Scanner`会自动转换数据类型，因此不必手动转换。

###### 屏幕输出

在前面的代码中，我们总是使用`System.out.println()`来向屏幕输出一些内容。

`println`是print line的缩写，表示输出并换行。因此，如果输出后不想换行，可以用`print()`：

```java
public class Main {
    public static void main(String[] args) {
        System.out.print("A,");
        System.out.print("B,");
        System.out.print("C.");
        System.out.println();
        System.out.println("END");
    }
}
```

###### 格式化输出

Java还提供了格式化输出的功能。为什么要格式化输出？因为计算机表示的数据不一定适合人来阅读：

```java
public class Main {
    public static void main(String[] args) {
        double d = 12900000;
        System.out.println(d); // 1.29E7
    }
}
```

如果要把数据显示成我们期望的格式，就需要使用格式化输出的功能。格式化输出使用`System.out.printf()`，通过使用占位符`%?`，`printf()`可以把后面的参数格式化成指定格式：

```java
// 格式化输出
public class Main {
    public static void main(String[] args) {
        double d = 3.1415926;
        System.out.printf("%.2f\n", d); // 显示两位小数3.14
        System.out.printf("%.4f\n", d); // 显示4位小数3.1416
    }
}
```

Java的格式化功能提供了多种占位符，可以把各种数据类型“格式化”成指定的字符串：

| 占位符 | 说明                             |
| :----- | :------------------------------- |
| %d     | 格式化输出整数                   |
| %x     | 格式化输出十六进制整数           |
| %f     | 格式化输出浮点数                 |
| %e     | 格式化输出科学计数法表示的浮点数 |
| %s     | 格式化字符串                     |

注意，由于%表示占位符，因此，连续两个%%表示一个%字符本身。

占位符本身还可以有更详细的格式化参数。下面的例子把一个整数格式化成十六进制，并用0补足8位：

```
// 格式化输出
public class Main {
    public static void main(String[] args) {
        int n = 12345000;
        System.out.printf("n=%d, hex=%08x", n, n); // 注意，两个%占位符必须传入两个数
    }
}
```

---

---

---

## 运算符和表达式

#### 运算符

**定义：对字面量或者变量进行操作的符号**

##### **1.算数运算符**

![image-20221110084953606](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110084953606.png)

在代码中，==如果有小数参加运算，结果有可能是不精确的。==

![image-20221110085330528](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110085330528.png)

**整数参加运算，结果只能得到整数。要想得到小数，必须有浮点数参与运算。**

![image-20221110085443269](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110085443269.png)

**取模的应用场景：1.用取模来判断A是否能被B整除**

​								**2.判断一个数是奇数还是偶数**

**练习：数值拆分**

![image-20221110085854802](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110085854802.png)

###### “+”操作的三种情况

###### **1.数字相加**

​	**数字进行运算时，数据类型不一样就不能进行运算，*需要转成一样的*，才能运算。**

###### 类型转换的分类

**隐式转换（自动类型提升）**

把一个取值范围小的数值，转成取值范围大的数据。

![image-20221110090822468](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110090822468.png)

**隐式转换的两种提升规则：**

1. **取值范围小的，和取值范围大的进行运算，小的会先提升为大的，再进行计算。**
2. **==byte  short  char  三种类型的数据在运算的时候，都会直接先提升为int ,然后再进行运算。==**

==**取值范围**==

![image-20221110090914232](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110090914232.png)

**示例：c是int类型**

![image-20221110091538231](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110091538231.png)

**强制转换**

如果把一个取值范围大的数值，赋值给取值范围小的变量。是不允许直接赋值的。如果一定要这么做就需要加入强制转换。

**格式：目标数据类型 变量名 = （目标数据类型）被强转的数据；**

![image-20221110092314059](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110092314059.png)

**示例：**

![image-20221110092811276](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110092811276.png)

**数据过大进行强转会导致发生错误**

![image-20221110092948038](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110092948038.png)



可以将浮点数强制转型为整数。在转型时，浮点数的小数部分会被丢掉。如果转型后超过了整型能表示的最大范围，将返回整型的最大值。例如：

```
int n1 = (int) 12.3; // 12
int n2 = (int) 12.7; // 12
int n2 = (int) -12.7; // -12
int n3 = (int) (12.7 + 0.5); // 13
int n4 = (int) 1.2e20; // 2147483647
```

如果要进行四舍五入，可以对浮点数加上0.5再强制转型：

```java
// 四舍五入
public class Main {
    public static void main(String[] args) {
        double d = 2.6;
        int n = (int) (d + 0.5);
        System.out.println(n);
    }
}
```

---

###### **2.字符串的“+”操作**

- ==**当“+“操作中出现字符串时，这个”+“是字符串连接符，而不是算数运算符。会将前后的数据进行拼接，并产生一个新的字符串。**==

  ![image-20221110093502532](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110093502532.png)

- ==**连续进行”+“操作时，从左到右逐个执行。**==

- 图中结果应该为“100年黑马”。

  ![image-20221110093609129](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110093609129.png)

**练习：重要（打印时的操作）**

![image-20221110133551701](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110133551701.png)

---

###### 3.字符的“+”操作

**当 字符 + 字符 或 字符  + 数字 时，会把字符通过ASCII码表查询到对应的数字再进行计算**

（byte  short  char  三种类型的数据在运算的时候，都会直接先提升为int ,然后再进行运算。）

![image-20221110134637613](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110134637613.png)

##### 2.自增自减运算符

基本用法

| 符号 | 作用 |    说明     |
| :--: | :--: | :---------: |
|  ++  |  加  | 变量的值加1 |
|  --  |  减  | 变量的值减1 |

**注意事项： **

**1.+ + 和 - - 无论是放在变量的前边还是后边，单独写一行结果是一样的。**

**2.当参与计算时**

==**a++表示a+1前的值；++a表示a+1后的值；**==

==**a--表示a-1前的值；--a表示a-1后的值；==**

![image-20221110135922855](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110135922855.png)

**b=10；b=11；**

##### 3.赋值运算符

**分类：**

![image-20221110140233900](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110140233900.png)

**扩展的赋值运算符隐含了强制类型转换**

![image-20221110140956167](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110140956167.png)

##### 4.关系运算符

**分类：**

![image-20221110141311971](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110141311971.png)

**注意：关系运算符的结果都是boolean类型，要么是true，要么是false。千万不要把“==”误写成“=”。**

**练习：注意输出的boolean类型**

![image-20221110142722721](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110142722721.png)

##### 5.逻辑运算符

![image-20221110143609931](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110143609931.png)

**分类：**

![image-20221110142907235](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110142907235.png)

**注意：逻辑运算符的结果都是boolean类型，要么是true，要么是false。**

---

**短路逻辑运算符&&**

**具有短路效果**

简单理解：当左边的表达式能确定最终的结果，那么右边就不会参加运行了。

![image-20221110143935805](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110143935805.png)

![image-20221110145004192](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110145004192.png)

##### 5.三元运算符

**格式：关系表达式？表达式1：表达式2；**

**实例：求两个数的较大值**

```java
//把三元运算符的结果赋值给一个变量
int max = a>b? a: b;
System.out.println(a>b? a: b);
```

==**注意：三元运算符的结果必须要被使用**==

==**三元运算符的两个结果的类型必须相同**==

**运算规则：**

- 首先计算关系表达式的值
- 如果值为true，表达式1的值就是运算结果
- 如果值为false，表达式2的值就是运算结果

**练习：比较三个和尚的身高**

![image-20221110151030781](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110151030781.png)

##### 6.移位运算符

在计算机中，整数总是以二进制的形式表示。例如，`int`类型的整数`7`使用4字节表示的二进制如下：

```ascii
00000000 0000000 0000000 00000111
```

可以对整数进行移位运算。对整数`7`左移1位将得到整数`14`，左移两位将得到整数`28`：

```
int n = 7;       // 00000000 00000000 00000000 00000111 = 7
int a = n << 1;  // 00000000 00000000 00000000 00001110 = 14
int b = n << 2;  // 00000000 00000000 00000000 00011100 = 28
int c = n << 28; // 01110000 00000000 00000000 00000000 = 1879048192
int d = n << 29; // 11100000 00000000 00000000 00000000 = -536870912
```

左移29位时，由于最高位变成`1`，因此结果变成了负数。

类似的，对整数28进行右移，结果如下：

```
int n = 7;       // 00000000 00000000 00000000 00000111 = 7
int a = n >> 1;  // 00000000 00000000 00000000 00000011 = 3
int b = n >> 2;  // 00000000 00000000 00000000 00000001 = 1
int c = n >> 3;  // 00000000 00000000 00000000 00000000 = 0
```

如果对一个负数进行右移，最高位的`1`不动，结果仍然是一个负数：

```
int n = -536870912;
int a = n >> 1;  // 11110000 00000000 00000000 00000000 = -268435456
int b = n >> 2;  // 11111000 00000000 00000000 00000000 = -134217728
int c = n >> 28; // 11111111 11111111 11111111 11111110 = -2
int d = n >> 29; // 11111111 11111111 11111111 11111111 = -1
```

还有一种无符号的右移运算，使用`>>>`，它的特点是不管符号位，右移后高位总是补`0`，因此，对一个负数进行`>>>`右移，它会变成正数，原因是最高位的`1`变成了`0`：

```
int n = -536870912;
int a = n >>> 1;  // 01110000 00000000 00000000 00000000 = 1879048192
int b = n >>> 2;  // 00111000 00000000 00000000 00000000 = 939524096
int c = n >>> 29; // 00000000 00000000 00000000 00000111 = 7
int d = n >>> 31; // 00000000 00000000 00000000 00000001 = 1
```

对`byte`和`short`类型进行移位时，会首先转换为`int`再进行位移。

仔细观察可发现，左移实际上就是不断地×2，右移实际上就是不断地÷2。

##### 运算符优先级

==注意记住（）优先于所有即可==

![image-20221110151159395](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110151159395.png)

#### 表达式

**定义：用运算符把字面量或者变量连接起来，符合java语法的式子就可以称为表达式。不同运算符连接的表达式体现的是不同类型的表达式。**

![image-20221110084730652](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110084730652.png)

---

---

---

## 流程控制语句

### 顺序结构

顺序结构是Java程序默认的执行流程，按照代码的先后顺序，从上到下依次执行。

#### 分支结构

##### if语句

![image-20221110152639188](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110152639188.png)

**注意：如果对一个布尔类型的的变量进行判断，建议不要用“==”号（但是也可以），直接把变量写在小括号中即可。**

```java
boolean flag = true;
if(flag){    //等于if(flag==true){};
    System.out.println("flat的值为true");
}
```

![image-20221110153952775](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110153952775.png)

![image-20221110154354081](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110154354081.png)

##### Switch语句

![image-20221110173317472](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110173317472.png)

![image-20221110173453260](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110173453260.png)

###### default的位置和省略

1. 位置：default不一定是写在最下面，我们可以写在任意位置。只不过习惯会写在最下面。
2. 省略：default可以省略，语法不会有问题，但是不建议省略。

**case穿透**

![image-20221110174235894](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110174235894.png)

**如果多个case的语句体重复了，那么我们考虑利用case穿透去简化代码**

###### 使用场景

if的第三种格式：一般用于对范围的判断

Switch：把有限个数据一一列举出来，让我们任选其一

###### Switch新语法

**注意新语法使用`->`，如果有多条语句，需要用`{}`括起来。不要写`break`语句，因为新语法只会执行匹配的语句，没有穿透效应。**

很多时候，我们还可能用`switch`语句给某个变量赋值。例如：

```
int opt;
switch (fruit) {
case "apple":
    opt = 1;
    break;
case "pear":
case "mango":
    opt = 2;
    break;
default:
    opt = 0;
    break;
}
```

使用新的`switch`语法，不但不需要`break`，还可以直接返回值。把上面的代码改写如下：

```java
public class Main {
    public static void main(String[] args) {
        String fruit = "apple";
        int opt = switch (fruit) {
            case "apple" -> 1;
            case "pear", "mango" -> 2;
            default -> 0;
        }; // 注意赋值语句要以;结束
        System.out.println("opt = " + opt);
    }
}
```

**yield**

大多数时候，在`switch`表达式内部，我们会返回简单的值。

但是，如果需要复杂的语句，我们也可以写很多语句，放到`{...}`里，然后，用`yield`返回一个值作为`switch`语句的返回值：

```java
public class Main {
    public static void main(String[] args) {
        String fruit = "orange";
        int opt = switch (fruit) {
            case "apple" -> 1;
            case "pear", "mango" -> 2;
            default -> {
                int code = fruit.hashCode();
                yield code; // switch语句返回值
            }
        };
        System.out.println("opt = " + opt);
    }
}
```











### 循环结构

##### for循环

![image-20221110175935574](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110175935574.png)

###### for each循环

`for`循环经常用来遍历数组，因为通过计数器可以根据索引来访问数组的每个元素：

```
int[] ns = { 1, 4, 9, 16, 25 };
for (int i=0; i<ns.length; i++) {
    System.out.println(ns[i]);
}
```

但是，很多时候，我们实际上真正想要访问的是数组每个元素的值。Java还提供了另一种`for each`循环，它可以更简单地遍历数组：

```java
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int n : ns) {
            System.out.println(n);
        }
    }
}
```

和`for`循环相比，`for each`循环的变量n不再是计数器，而是直接对应到数组的每个元素。`for each`循环的写法也更简洁。但是，`for each`循环无法指定遍历顺序，也无法获取数组的索引。

除了数组外，`for each`循环能够遍历所有“可迭代”的数据类型，包括后面会介绍的`List`、`Map`等。

##### while循环

![image-20221110193146553](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110193146553.png)

**两个循环的对比**

![image-20221110193328980](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110193328980.png)

![image-20221110193923737](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110193923737.png)

##### do while循环

![image-20221110194855838](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110194855838.png)

**构建无限循环（死循环）**

![image-20221110195303426](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221110195303426.png)

**跳转控制语句**

在循环的过程中，跳到其他语句上执行

==continue：结束本次循环，继续下次循环。==

==break：结束整个循环。==

#### 获取随机数

Java帮我们写好一个类叫Random，这个类就可以生成一个随机数。

![image-20221111141842114](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221111141842114.png)

**原型：获取0到任意数的随机数**

![image-20221111142402629](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221111142402629.png)

**生成任意数到任意数的随机数**

---

---

---

## 数组

数组介绍：数组指的是一种容器，可以用来储存==同种数据类型==的多个值。

![image-20221111143233822](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20221111143233822.png)

#### 数组的定义

**格式1：	数据类型 【】 数据名**

tip： int 【】array;

**格式2：	数据类型  数组名【】**

tip：int    array 【】;

#### 数组的初始化

初始化：就是在内存中，为数组容器开辟空间，并将数据存入容器的过程。（数组一旦初始化，大小不可变）

###### 静态初始化

**格式：数据类型 【】数组名=new 数据类型【】{元素1，元素2，元素3......}**;

**简化格式：数据类型 【】数组名={元素1，元素2，元素3......}**;

tip:  int[] array = {11,22,33};

#### 数组的地址值和元素访问

###### 数组动态初始化

**定义：初始化时只指定数组长度，由系统为数组分配默认初始值。**

**格式：数据类型【】数组名= new 数据类型【数组长度】**

tip：int【】arr = new int【3】；

![image-20221111154016879](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221111154016879.png)

数据动态初始化和静态初始化的区别

![image-20221111154154363](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221111154154363.png)

**可能遇到的问题**：索引越界异常
原因：访问了不存在的索引
避免：索引的范围

###### 数组的地址值

![image-20221111145431277](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221111145431277.png)		

![image-20221111150017481](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221111150017481.png)

###### 数组元素访问

**格式：数组名【索引】**

索引：也叫做下标，角标

索引的特点：从0开始，逐个+1增长，连续不间断

![image-20221111151218192](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221111151218192.png)

#### 数组的遍历

定义：将数组中所有的内容取出来，取出来之后可以（打印，求和，判断）

<img src="https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221111151726200.png" alt="image-20221111151726200" style="zoom:150%;" />

数组的长度

在Java中，关于数组的一个长度属性，length

**调用方式：数组名.length**

<img src="https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221111152506158.png" alt="image-20221111152506158" style="zoom:150%;" />

**==遍历的快捷方式==**

**==数组名.fori==**

<img src="https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221111152808787.png" alt="image-20221111152808787" style="zoom:150%;" />

**使用`for each`循环，直接迭代数组的每个元素：**

```java
// 遍历数组
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int n : ns) {
            System.out.println(n);
        }
    }
}
```

**注意：在`for (int n : ns)`循环中，变量`n`直接拿到`ns`数组的元素，而不是索引。**

显然`for each`循环更加简洁。但是，`for each`循环无法拿到数组的索引，因此，到底用哪一种`for`循环，取决于我们的需要。

###### 数组的打印 Arrays.toString()

**==Arrays.toString()==**

直接打印数组变量，得到的是数组在JVM中的引用地址：

```
int[] ns = { 1, 1, 2, 3, 5, 8 };
System.out.println(ns); // 类似 [I@7852e922
```

这并没有什么意义，因为我们希望打印的数组的元素内容。因此，使用`for each`循环来打印它：

```
int[] ns = { 1, 1, 2, 3, 5, 8 };
for (int n : ns) {
    System.out.print(n + ", ");
}
```

使用`for each`循环打印也很麻烦。幸好**Java标准库提供了`Arrays.toString()`**，可以快速打印数组内容：

```
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 1, 2, 3, 5, 8 };
        System.out.println(Arrays.toString(ns));
    }
}
```

**练习题：**

1.求最值

<img src="https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221111155004058.png" alt="image-20221111155004058" style="zoom:150%;" />

2.交换数组中的数据

![image-20221112130811347](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221112130811347.png)

<img src="https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221112131131878.png" alt="image-20221112131131878" style="zoom:150%;" />

3.打乱顺序

![](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221112131441644.png)

#### 数组的排序

###### 冒泡排序

```
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 28, 12, 89, 73, 65, 18, 96, 50, 8, 36 };
        // 排序前:
        System.out.println(Arrays.toString(ns));
        for (int i = 0; i < ns.length - 1; i++) {
            for (int j = 0; j < ns.length - i - 1; j++) {
                if (ns[j] > ns[j+1]) {
                    // 交换ns[j]和ns[j+1]:
                    int tmp = ns[j];
                    ns[j] = ns[j+1];
                    ns[j+1] = tmp;
                }
            }
        }
        // 排序后:
        System.out.println(Arrays.toString(ns));
    }
}
```

冒泡排序的特点是，每一轮循环后，最大的一个数被交换到末尾，因此，下一轮循环就可以“刨除”最后的数，每一轮循环都比上一轮循环的结束位置靠前一位。

###### Arrays.sort()

**默认为升序排序**

```
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] ns = { 28, 12, 89, 73, 65, 18, 96, 50, 8, 36 };
        Arrays.sort(ns);
        System.out.println(Arrays.toString(ns));
    }
}
```

**降序排序**

```
public static <T> void sort(T[] a,int fromIndex, int toIndex,  Comparator<? super T> c)
```

**要实现减序排序，得通过包装类型数组，基本类型数组是不行滴**

**用java自带的函数**==**Collections.reverseOrder（）**==

```
public class text1
{
    public static void main(String []args)
    {
        Integer[] integers=new Integer[]{2,324,4,4,6,1};
        Arrays.sort(integers, Collections.reverseOrder());
        for (Integer integer:integers)
        {
            System.out.print(integer+" ");
        }
    }
}
```

必须注意，对数组排序实际上修改了数组本身。例如，排序前的数组是：

```
int[] ns = { 9, 3, 6, 5 };
```

在内存中，这个整型数组表示如下：

```ascii
      ┌───┬───┬───┬───┐
ns───▶│ 9 │ 3 │ 6 │ 5 │
      └───┴───┴───┴───┘
```

当我们调用`Arrays.sort(ns);`后，这个整型数组在内存中变为：

```ascii
      ┌───┬───┬───┬───┐
ns───▶│ 3 │ 5 │ 6 │ 9 │
      └───┴───┴───┴───┘
```

即变量`ns`指向的数组内容已经被改变了。

如果对一个字符串数组进行排序，例如：

```
String[] ns = { "banana", "apple", "pear" };
```

排序前，这个数组在内存中表示如下：

```ascii
                   ┌──────────────────────────────────┐
               ┌───┼──────────────────────┐           │
               │   │                      ▼           ▼
         ┌───┬─┴─┬─┴─┬───┬────────┬───┬───────┬───┬──────┬───┐
ns ─────▶│░░░│░░░│░░░│   │"banana"│   │"apple"│   │"pear"│   │
         └─┬─┴───┴───┴───┴────────┴───┴───────┴───┴──────┴───┘
           │                 ▲
           └─────────────────┘
```

调用`Arrays.sort(ns);`排序后，这个数组在内存中表示如下：

```ascii
                   ┌──────────────────────────────────┐
               ┌───┼──────────┐                       │
               │   │          ▼                       ▼
         ┌───┬─┴─┬─┴─┬───┬────────┬───┬───────┬───┬──────┬───┐
ns ─────▶│░░░│░░░│░░░│   │"banana"│   │"apple"│   │"pear"│   │
         └─┬─┴───┴───┴───┴────────┴───┴───────┴───┴──────┴───┘
           │                              ▲
           └──────────────────────────────┘
```

原来的3个字符串在内存中均没有任何变化，但是`ns`数组的每个元素指向变化了。

#### Arrays

[Arrays常用方法（超详解）_arrays.tostring方法-CSDN博客](https://blog.csdn.net/qq_62939743/article/details/123435767?ops_request_misc=%7B%22request%5Fid%22%3A%22170167434216800227458923%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=170167434216800227458923&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-123435767-null-null.142^v96^pc_search_result_base8&utm_term=Arrays&spm=1018.2226.3001.4187)

#### 数组的内存图

![image-20221121200703238](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221121200703238.png)

![image-20221121201635221](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221121201635221.png)

![image-20221121201922797](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221121201922797.png)

![image-20221121202706400](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221121202706400.png)

**1.只要是new出来的一定是在堆里面开辟了一个小空间**
**2.如果new了多次，那么在堆里面有多个小空间，每个**
**小空间中都有各自的数据**

![image-20221122083602972](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122083602972.png)

​		**当两个数组指向同一个小空间时，其中一个数组对小空间中的值发生了改变，那么其他数组再次访问的时**
**候都是修改之后的结果了。**





---

---

---

## 方法

**方法（method）是程序中最小的执行单元**

**用法：重复的代码、具有独立功能的代码可以抽取到方法中。**

**好处：可以提高代码的复用性**

​			**可以提高代码的可维护性**

#### 方法定义和调用

把一些代码打包在一起，用到的时候就调用

**在实际开发中，我们一般把重复的代码或者具有独立功能的代码抽取到方法中，之后直接调用就可以。**

**方法定义：把一些代码打包在一起，该过程称为方法定义**

**方法调用：方法定义后不是直接运行的，需要手动调用才能执行，该过程称为方法调用。**

###### 方法定义格式

**最简单的方法定义**

![image-20221114171127256](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221114171127256.png)

**带参数的方法定义和调用**

![image-20221114172914254](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221114172914254.png)

![image-20221114173005442](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221114173005442.png)

**注意：方法调用时，参数的数量与类型必须与方法定义中小括号里面的变量一一对应，否则程序将报错。**

**示例：**

![image-20221114173419927](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221114173419927.png)

**带返回值方法的定义和调用**

如果在调用处要根据方法的结果，去编写另外一段代码逻辑。

为了在调用处拿到方法产生的结果，就需要定义有返回值的方法。

![image-20221114174922044](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221114174922044.png)

**带返回值方法的调用**

![image-20221114175334111](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221114175334111.png)

**1.定义方法的小技巧**

- **我要干什么？       方法体**
- **我干这件事需要什么才能完成？   形参（根据不同的需求，选择定义无参的方法，还是带参数的方法）**
- **方法的调用处是否需要继续使用结果？       返回值	**					

**2.方法的注意事项**

- 方法不调用就不执行
- 方法与方法之间是平级关系，不能互相嵌套定义
- 方法的编写顺序和执行顺序无关
- 方法的返回值是void，表示该方法没有返回值，没有返回值的方法可以省略return语句不写。如果要编写return，后面不能跟具体的数字。

**4.return关键字**

- 方法没有返回值：可以省略不写。如果书写，表示结束方法  return；
- 方法有返回值：必须要写。表示结束方法和返回结果。
- return和break关键字的区别
  - return：和循环没有关系，跟方法有关，表示结束方法和返回结果。如果方法执行到return，那么整个方法全部结束，里面的循环也随之结束。
  - break：和方法没有什么区别，结束循环或switch。

**5.形参和实参**

**形参：全称形式参数，是指方法定义中的参数**

**实参：全称实际参数，是指方法调用中的函数。**

###### 方法的重载

- 在同一个类中，定义了多个同名的方法，这些**同名的方法**具有同种的功能。
- 每个方法**具有不同的参数类型或参数个数**，这些同名的方法就构成了重载关系。

简单记：同一个类中，方法名相同，参数不同的方法。与返回值无关。参数不同：个数不同、类型不同、顺序不同。

**好处：**

1. 定义方法的时候可以不用那么多的单词。
2. 调用方法的时候不需要那么麻烦。

**练习：**

1.遍历数组

需求：设计一个方法用于数组遍历，要求遍历的结果是在一行上的。例如：[1，2，3，4，5]

```java
public class Tests{
    public static void main(String[] args){
		int [] arr = {1,2,3,4,5};
        printArr(arr);
    }
    public static void printArr(int [] arr){
        System.out.print("[");
        for(int i = 0;i<arr.length;i++){
            if(i==arr.length-1){
                System.out.print(arr[i]);
            }
            else{
				System.out.print(arr[i] + ",");
            }
        }
        System.out.print("]");
    }
        
}
```

2.求数组中的最大值

需求：设计一个方法求数组的最大值，并将最大值返回

![image-20221115143832238](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221115143832238.png)

3.判断数组中数的存在情况

需求：定义一个方法判断数组中的某一个数是否存在，将结果返回给调用处。

![image-20221115145018218](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221115145018218.png)

4.拷贝数组

![image-20221115145803691](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221115145803691.png)

###### 方法的基本内存原理

![image-20221122191038431](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122191038431.png)

![image-20221122191722032](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122191722032.png)

**tip：**

```java
// main方法
public static void main(String[] args) {
    guessNumberGame();
}
//方法一：guessNumberGame()
public static void guessNumberGame() {
    while(true){
        int choice = menu();
        if(choice == 1){
            game();
        }else if(choice == 0){
            System.out.println("白白~");
            break;
        }else{
            System.out.println("输入错误，请重试...");
        }
    }
}
//方法二：game()
public static void game() {
    Random random = new Random();
    int toGuess = random.nextInt(100)+1;
    Scanner scanner = new Scanner(System.in);
    while(true){
        System.out.println("请输入你猜测的数：");
        int num = scanner.nextInt();
        if(num>toGuess){
            System.out.println("猜大了");
        }else if(num<toGuess){
            System.out.println("猜小了");
        }else {
            System.out.println("恭喜你，猜对了！");
            break;
        }
    }
}
//方法三：menu()
public static int menu() {
    System.out.println("***********************");
    System.out.println("  1、play      0、exit  ");
    System.out.println("***********************");
    System.out.println("请输入您的选择：");
    Scanner scanner = new Scanner(System.in);
    int choice = scanner.nextInt();
    return choice;
}
```



(1) main方法是程序入口，先将main方法放入栈中，最先放入，所以在栈底

![img](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/20210109165801380.png)

(2) 在main方法中遇到guessNumberGame()语句，就会进入该方法，该方法进栈，继续执行该方法中的语句

![在这里插入图片描述](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/20210109163412366.png)

(3) 在guessNumberGame()方法中又遇到 menu() 方法，则进入该方法，menu()方法进栈

![在这里插入图片描述](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/20210109163924530.png)

(4) 在menu方法中有多个println方法，我们叫它们println1、println2、println3……它们都是println只是参数不同。执行println1时，它会进栈，执行完了以后，它就会被从栈中删除，这也就是所谓的入栈和出栈。
**入栈：调用某个方法，就会把该方法对应的一些信息，放到栈里面；**
**出栈：当某个方法执行完毕，就会把该方法对应的信息从栈中删除掉。**

![在这里插入图片描述](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/2021010916584612.png)

menu方法中的其他入栈出栈就不再过多赘述。
(5) menu方法执行完了之后，它也会被从栈中删除

![在这里插入图片描述](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/20210109170740354.png)

(6) 回到guessNumberGame方法中，接着执行碰到game方法，game方法再入栈，执行结束后出栈，以此类推到程序整个执行结束。

![在这里插入图片描述](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/20210109171813639.png)

###### 基本数据类型和引用数据类型

****

**基本数据类型**

- 整数类型
- 浮点数类型
- 布尔类型
- 字符类型

**引用数据类型**（只要是new出来的都是）

除以上其他所有类型

**基本数据类型**（变量中储存的是真实的数据）

![image-20221122193936489](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122193936489.png)

**引用数据类型**（变量中储存的是地址值）

![image-20221122194447314](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122194447314.png)

==从内存的角度去解释==

**基本数据类型：数据值是储存在自己的空间中**

**特点：赋值给其他变量，也是赋的真实的值**

![image-20221122194938022](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122194938022.png)

**引用数据类型：数据值是存储在其他空间中，自己空间中存储的是地址值。**

**特点：赋值给其他变量，赋的是地址值。**



![image-20221122195215545](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122195215545.png)

###### 方法传递基本数据类型的内存原理

![image-20221122201000315](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122201000315.png)

==传递基本数据类型时，传递的是真实的数据，形参的改变，不影响实际参数的值。==

可以通过返回值的方式改变number的值

![image-20221122201337351](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122201337351.png)

###### 方法传递引用数据类型的内存原理

![image-20221122202053759](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122202053759.png)

==传递引用数据类型时，传递的是地址值，形参的改变，影响实际参数的值。==

---

---

---

---

---

---

## 面向对象

面向：拿、找

对象：能干活的东西

面向对象编程：拿东西过来做对应的事情

**重点学习：学习获取已有对象并使用；学习如何自己设计对象并使用。**

#### class和instance

理解了class和instance的概念，基本上就明白了什么是面向对象编程。

class是一种对象模版，它定义了如何创建实例，因此，class本身就是一种数据类型：

![class](https://www.liaoxuefeng.com/files/attachments/1260571618658976/l)

而instance是对象实例，instance是根据class创建的实例，可以创建多个instance，每个instance类型相同，但各自属性可能不相同：

![instances](https://www.liaoxuefeng.com/files/attachments/1260571718581056/l)

1. 类（设计图）:是对象共同特征的描述（设计图）。
2. 对象：是真实存在的具体东西
3. 在Java中，必须先设计类，才能获得对象

**如何定义类**

````java
	public class 类名{
    1.成员变量（代表属性，一般是名词）
    2.成员方法（代表行为，一般是动词）
    3.构造器（后面学习）
    4.代码块（后面学习）
    5.内部块（后面学习）
}
````

```java
	public class Phone{
    //属性（成员变量）
    String brand;
    double price;
    //行为（方法）
    public void call(){
    }
    public void playGame(){
    }
}
```

**如何得到类的对象**

类名  对象名 = new 类名();

Phone p = new Phone();

**如何使用对象**

- 访问属性：对象名.成员变量
- 访问行为：对象名.方法名(...)

**定义类的补充注意事项**

- **用来描述一类事物的类，专业叫做：Javabean类。在Javabean类中，是不写main方法的。**
- 在以前，编写main方法的类，叫做测试类。我们可以在测试类中创建Javabean类的对象并进行赋值调用
- 类名首字母建议大写、英文、有意义、满足大驼峰模式，不能用关键字，满足标志符规定。
- **一个Java文件中可以定义多个class类，且只能一个类是public修饰，而且public修饰的类名必须成为代码文件名。**
- 成员变量的完整定义格式是：==修饰符  数据类型  变量名称  =  初始化值;==一般无需指定初始化值，存在默认值。

![image-20221115163601416](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221115163601416.png)

**示例：**

定义一个类

![image-20221115161212025](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221115161212025.png)

得到类的对象并使用

![image-20221115161415933](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221115161415933.png)

#### 封装

**封装的好处：**

- **原则：**==**对象代表什么，就得封装对应的数据，并提供数据对应的行为（行为导致哪个对象的属性改变，行为就定义在该对象的类中）**==
- **可以把类中的某些信息进行隐藏，从而使外部程序不能直接对这些信息进行直接的访问，只能通过类中定义的方法对这些隐藏的信息进行操作和访问。**
- **使其他类只能通过操控类中的对象来直接达到目的，不能看到具体的实现和属性，从而提高了程序的安全性和便利性。隐藏信息。**
- 降低我们的学习成本，可以少学，少记，或者是压根不用学，不用记对象有哪些方法，有需要时去找就行。

###### private 关键字

- 是一个权限修饰符
- 可以修饰成员（成员变量和成员方法）
- 被private修饰的成员==只能在本类中才能访问==（不能被外界调用）

![](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221115173230533.png)

![image-20221115173350371](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221115173350371.png)

###### this关键字

==区别成员变量和局部变量==

==使用this.age指的是成员变量==

**成员变量和局部变量**

**成员变量：在类中，方法外的变量**

**局部变量：在方法中的变量**

![image-20221115173531486](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221115173531486.png)

**这里打印出来的age是成员变量还是局部变量呢**

==就近原则：谁离我近，我就用谁==

打印出来的age是10。

![image-20221122225411426](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122225411426.png)

#### 构造方法

构造方法也叫做构造器，构造函数

**作用：在创建对象的时候给成员变量进行赋值的。**

![image-20221115190950311](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221115190950311.png)

![image-20221115191016526](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221115191016526.png)

特点：

1. **方法名与类名相同，大小写也要一致**
2. **没有返回值类型，连void也不能有**
3. **没有具体的返回值（不能由return带回结果数据）**

执行时机：

1. 创建对象的时候由虚拟机调用，不能手动调用构造方法。
2. **每次创建一次对象，就会调用一次构造方法。**

tip：

![ ](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221115202541976.png)

构造方法注意事项：

- 构造方法的定义
  - 如果没有定义构造方法，系统将给出一个默认的无参数构造方法。
  - 如果定义了构造方法，系统将不再提供默认的构造方法。

- 构造方法的重载
  - 带参构造方法，和无参数构造方法，两者方法名相同，但是参数不同，这叫做构造方法的重载。

- 推荐的使用方法
  - **==无论是否使用，都手动书写无参数构造方法，和带全部参数的构造方法。==**

#### JavaBean

- **类名需要见名知意**
- **成员变量使用private修饰（保证数据安全性）**
- **提供至少两个构造方法**
  - **无参构造方法**
  - **带全部参数的构造方法**

- **成员方法**
  - **提供每一位成员变量对应的setXXX() / getXXX()**
  - **如果还有其他行为，也需要写上**

==构造方法和get/set快捷键==

alt + insert

alt + fn + insert

在Java中，有很多`class`的定义都符合这样的规范：

- 若干`private`实例字段；
- 通过`public`方法来读写实例字段。

例如：

```
public class Person {
    private String name;
    private int age;

    public String getName() { return this.name; }
    public void setName(String name) { this.name = name; }

    public int getAge() { return this.age; }
    public void setAge(int age) { this.age = age; }
}
```

如果读写方法符合以下这种命名规范：

```
// 读方法:
public Type getXyz()
// 写方法:
public void setXyz(Type value)
```

那么这种`class`被称为`JavaBean`：

上面的字段是`xyz`，那么读写方法名分别以`get`和`set`开头，并且后接大写字母开头的字段名`Xyz`，因此两个读写方法名分别是`getXyz()`和`setXyz()`。

`boolean`字段比较特殊，它的读方法一般命名为`isXyz()`：

```
// 读方法:
public boolean isChild()
// 写方法:
public void setChild(boolean value)
```

我们通常把一组对应的读方法（`getter`）和写方法（`setter`）称为属性（`property`）。例如，`name`属性：

- 对应的读方法是`String getName()`
- 对应的写方法是`setName(String)`

只有`getter`的属性称为只读属性（read-only），例如，定义一个age只读属性：

- 对应的读方法是`int getAge()`
- 无对应的写方法`setAge(int)`

类似的，只有`setter`的属性称为只写属性（write-only）。

很明显，只读属性很常见，只写属性不常见。

属性只需要定义`getter`和`setter`方法，不一定需要对应的字段。例如，`child`只读属性定义如下：

```
public class Person {
    private String name;
    private int age;

    public String getName() { return this.name; }
    public void setName(String name) { this.name = name; }

    public int getAge() { return this.age; }
    public void setAge(int age) { this.age = age; }

    public boolean isChild() {
        return age <= 6;
    }
}
```

可以看出，`getter`和`setter`也是一种数据封装的方法。

###### JavaBean的作用

JavaBean主要用来传递数据，即把一组数据组合成一个JavaBean便于传输。此外，JavaBean可以方便地被IDE工具分析，生成读写属性的代码，主要用在图形界面的可视化设计中。

通过IDE，可以快速生成`getter`和`setter`。例如，在Eclipse中，先输入以下代码：

```
public class Person {
    private String name;
    private int age;
}
```

然后，点击右键，在弹出的菜单中选择“Source”，“Generate Getters and Setters”，在弹出的对话框中选中需要生成`getter`和`setter`方法的字段，点击确定即可由IDE自动完成所有方法代码。

###### 对象内存图

==方法区：当运行一个类时，这个类的字节码文件就会加载到方法区进行临时存储。==

![](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122203753512.png)

==创建多个对象时，class类不用重复加载到方法区中==

==当main方法出栈时，对象消失，堆内存中的对象的内容也消失==

###### this的内存原理

![image-20221122225228073](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122225228073.png)

---

---

---

#### 继承

**继承可以使得子类具有父类的和属性和方法或者重新定义、追加属性和方法等。**

- ==Java中提供一个关键字extends，用这个关键字，我们可以让一个类和另一个类建立起继承关系==

  ```java
  public class Student extends Person {}
  ```

- ==Student称为子类（派生类），Person称为父类（基类或超类）==

使用继承的好处

- 可以把多个子类中重复的代码抽取到父类中了，提高代码的复用性。
- 子类可以在父类的基础上，增加其他的功能，使子类更强大。

**什么时候用继承**

==当类与类之间，存在相同（共性）的内容，并满足子类是父类中的一种，就可以考虑使用继承，来优化代码时间。==

##### 继承的特点

**Java只支持单继承，不支持多继承，但支持多层继承。**

****

- **单继承：一个子类只能继承一个直接父类**

- **不支持多继承：子类不能同时继承多个直接父类**

- **多层继承：子类A继承父类B，父类B可以继承父类C（Java中的每一个类都直接或者间接的继承于Object）**

  ![image-20221118105659202](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221118105659202.png)

###### 子类到底能继承父类中的哪些内容

![image-20221118190527071](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221118190527071.png)

1、构造方法

父类的构造方法不能被子类继承。

但是可以通过super调用

子类中所有的构造方法默认都会访问父类中的无参构造方法。

2、成员变量

父类中的成员变量是非私有的，子类中可以直接访问，若父类中的成员变量私有了，子类是不能直接访问的。通常编码时，我们遵循封装的选择，使用private修饰成员变量，要访问父类的私有成员变量，可以在父类中提供公共的get/set方法。

3、成员方法

在父子类的继承关系当中，创建子类对象，访问成员方法的规则：

创建的对象是谁，就优先用谁，如果没有就向上找。



##### 继承中成员变量和成员方法的==访问特点==

###### 继承中成员变量的访问特点

![image-20221118191136807](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221118191136807.png)

==就近原则：谁离我进，我就用谁==

**先到局部变量位置找：“ziShow”。如果没有，再到成员变量位置找：“zi”。如果再没有：到父类中找：“Fu”。逐级往上**

![image-20221118191905348](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221118191905348.png)

![image-20221118192739948](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221118192739948.png)

###### **继承中成员变量的访问特点**

**直接调用满足就近原则：谁离我近，我就用谁。**

**super调用，直接访问父类。**

**==方法的重写==**

**应用场景：当父类的方法不能满足子类现在的需求时，需要进行方法重写**

**书写格式：在继承体系中，子类出现了和父类中一模一样的方法声明，我们就称子类这个方法是重写的方法。**

![image-20221118193537195](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221118193537195.png)

![image-20221118193844433](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221118193844433.png)

![image-20221118194249396](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221118194249396.png)

**方法重写注意事项和要求**
**1.重写方法的名称、形参列表必须与父类中的一致。**
**2.子类重写父类方法时，访问权限子类必须大于等于父类**
**3.子类重写父类方法时，返回值类型子类必须小于等于父类**
**4.建议：重写的方法尽量和父类保持一致。**
5.只有被添加到虚方法表中的方法才能被重写

###### 继承中构造方法的访问特点

- **==父类中的构造方法不会被子类继承==**
- **子类中所有的构造方法默认先访问父类中的无参构造，再执行自己。**
  - **子类在初始化的时候，有可能会使用到父类中的数据，如果父类没有完成初始化，子类将无法使用父类的数据。**
  - **子类初始化之前，一定要调用父类构造方法先完成父类数据空间的初始化。**

- ==**怎么调用父类构造方法**==
  - **子类构造方法的第一行语句默认都是：super(),不写也存在，且必须在第一行。**
  - **如果想调用父类有参构造，必须手动写super进行调用。**

![image-20221118200659592](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221118200659592.png)

==注意最下面的super（name，age）这是在调用父类带参构造方法==

**继承中构造方法的访问特点是什么**

- 子类不能继承父类的构造方法，但是可以通过super调用
- 子类构造方法的第一行，有一个默认的super()；
- 默认先访问父类中无参的构造方法，再执行自己。
- 如果想要在构造方法中调用父类有参构造，必须手动书写。

###### this、super使用总结

**this：理解为一个变量，表示当前方法调用者的地址值**、

**super关键字的用法如下：**

- **`super`可以用来引用直接父类的实例变量。**
- **`super`可以用来调用直接父类方法。**
- **`super()`可以用于调用直接父类构造函数。**

![image-20221118202045018](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221118202045018.png)

**这里画线的this是默认存在的，不用写出。**

![image-20221119064956572](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221119064956572.png)

![image-20221119065020920](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221119065020920.png)

![image-20221123161049761](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221123161049761.png)

上表对 `this `与 `super` 的差别进行了比较，从上表中不难发现，**==用 super 或this调用构造方法时都需要放在首行==**，所以`super` 与 `this` 调用构造方法的操作是不能同时出现的。

---

---

---

#### 多态

**==方法重载（overload）实现的是编译时的多态性（也称为前绑定），而方法重写（override）实现的是运行时的多态性（也称为后绑定）。==**

**同类型的对象，表现出的不同形态**

![image-20221119072046331](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221119072046331.png)

**这个学生对象有了两种形态，一种是学生形态，另一种是人的形态。**

**应用场景：代码的通用（注册）**

![](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221119123210552.png)

![image-20221119111218632](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221119111218632.png)

所以，多态的特性就是，运行期才能动态决定调用的子类方法。对某个类型调用某个方法，执行的实际方法可能是某个子类的覆写方法。这种不确定性的方法调用，究竟有什么作用？

我们还是来举栗子。

假设我们定义一种收入，需要给它报税，那么先定义一个`Income`类：

```
class Income {
    protected double income;
    public double getTax() {
        return income * 0.1; // 税率10%
    }
}
```

对于工资收入，可以减去一个基数，那么我们可以从`Income`派生出`SalaryIncome`，并覆写`getTax()`：

```
class Salary extends Income {
    @Override
    public double getTax() {
        if (income <= 5000) {
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}
```

如果你享受国务院特殊津贴，那么按照规定，可以全部免税：

```
class StateCouncilSpecialAllowance extends Income {
    @Override
    public double getTax() {
        return 0;
    }
}
```

现在，我们要编写一个报税的财务软件，对于一个人的所有收入进行报税，可以这么写：

```
public double totalTax(Income... incomes) {
    double total = 0;
    for (Income income: incomes) {
        total = total + income.getTax();
    }
    return total;
}
```

来试一下：

```java
public class Main {
    public static void main(String[] args) {
        // 给一个有普通收入、工资收入和享受国务院特殊津贴的小伙伴算税:
        Income[] incomes = new Income[] {
            new Income(3000),
            new Salary(7500),
            new StateCouncilSpecialAllowance(15000)
        };
        System.out.println(totalTax(incomes));
    }

    public static double totalTax(Income... incomes) {
        double total = 0;
        for (Income income: incomes) {
            total = total + income.getTax();
        }
        return total;
    }
}

class Income {
    protected double income;

    public Income(double income) {
        this.income = income;
    }

    public double getTax() {
        return income * 0.1; // 税率10%
    }
}

class Salary extends Income {
    public Salary(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        if (income <= 5000) {
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}

class StateCouncilSpecialAllowance extends Income {
    public StateCouncilSpecialAllowance(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        return 0;
    }
}
```

观察`totalTax()`方法：利用多态，`totalTax()`方法只需要和`Income`打交道，它完全不需要知道`Salary`和`StateCouncilSpecialAllowance`的存在，就可以正确计算出总的税。如果我们要新增一种稿费收入，只需要从`Income`派生，然后正确覆写`getTax()`方法就可以。把新的类型传入`totalTax()`，不需要修改任何代码。

==表现形式：父类类型 对象名称 = 子类对象==

==多态的前提：==

- 有继承关系

- 有父类引用指向子类对象

  ```java
  Fu f = new Zi();
  ```

- 有方法重写

**多态的好处：**

-  使用父类型作为参数，可以接收所有子类对象。
-  体现多态的扩展性与便利。

##### 多态调用成员的特点

- ==**变量调用：编译看左边，运行也看左边。**==
- ==**方法调用：编译看左边，运行看右边。**==

![image-20221119124834728](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221119124834728.png)

![image-20221119125157419](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221119125157419.png)

##### 方法的重写（`override`）

1. 方法的重写 子类从父类中继承方法，有时，子类需要修改父类中定义的方法的实现，这称做**方法的重写**(`method overriding`)。**当一个子类继承一父类，而子类中的方法与父类中的方法的名称、参数个数和类型都完全一致时，就称子类中的这个方法重写了父类中的方法。**“重写”又称为“复写”、“覆盖”。

2. 如何使用重写

   ```java
       class Super {
       	访问权限 方法返回值类型 方法1（参数1）{...}
       }
   	class Sub extends Super{
           访问权限 方法返回值类型 方法1（参数1） {复写父类中的方法...}
       }
   ```

**注意：**方法重写时必须遵循两个原则，否则编译器会指出程序出错。

- **重写的方法不能比被重写的方法有更严格的访问权限。**
- 重写的方法不能比被重写的方法产生更多的异常（关于异常，在后面会介绍）。

##### 方法的重载（`overload`）

1. **方法重载**是指多个方法可以享有相同的名字，但是**参数的数量或类型不能完全相同**。 **调用方法时，编译器根据参数的个数和类型来决定当前所使用的方法。**方法重载为程序的编写带来方便，是`OOP`多态性的具体变现。
2. 重载的规则

- **被重载的方法必须改变参数列表(参数个数或类型不一样)；**
- 被重载的方法可以改变返回类型；
- 被重载的方法可以改变访问修饰符；
- 被重载的方法可以声明新的或更广的检查异常；
- 方法能够在同一个类中或者在一个子类中被重载。
- 无法以返回值类型作为重载函数的区分标准。

##### 重写与重载之间的区别

![image-20221122090310464](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122090310464.png)

方法的重写和重载是`Java`多态性的不同表现，重写是父类与子类之间多态性的一种表现，重载可以理解成多态的具体表现形式。

- **方法重载是一个类中定义了多个方法名相同，而他们的参数的数量不同或数量相同而类型和次序不同，则称为方法的重载。**
- **方法重写是在子类存在方法与父类的方法的名字相同而且参数的个数与类型一样，返回值也一样的方法，就称为方法的重写。**
- **方法重载是一个类的多态性表现，而方法重写是子类与父类的一种多态性表现。**

![image-20221122090340570](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122090340570.png)

---

---

---

## 包和final

**包就是文件夹。用来管理不同功能的java类，方便后期代码维护**

==包名的规则：公司域名反写+包的作用，需要全部英文小写，见名知意。com.itheima.domain==

![image-20221122230124821](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122230124821.png)

事实上：全类名才是一个类真正的名字。

![image-20221122230217446](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122230217446.png)

###### 包

在前面的代码中，我们把类和接口命名为`Person`、`Student`、`Hello`等简单名字。

在现实中，如果小明写了一个`Person`类，小红也写了一个`Person`类，现在，小白既想用小明的`Person`，也想用小红的`Person`，怎么办？

如果小军写了一个`Arrays`类，恰好JDK也自带了一个`Arrays`类，如何解决类名冲突？

在Java中，我们使用`package`来解决名字冲突。

Java定义了一种名字空间，称之为包：`package`。一个类总是属于某个包，类名（比如`Person`）只是一个简写，真正的完整类名是`包名.类名`。

例如：

小明的`Person`类存放在包`ming`下面，因此，完整类名是`ming.Person`；

小红的`Person`类存放在包`hong`下面，因此，完整类名是`hong.Person`；

小军的`Arrays`类存放在包`mr.jun`下面，因此，完整类名是`mr.jun.Arrays`；

JDK的`Arrays`类存放在包`java.util`下面，因此，完整类名是`java.util.Arrays`。

在定义`class`的时候，我们需要在第一行声明这个`class`属于哪个包。

小明的`Person.java`文件：

```
package ming; // 申明包名ming

public class Person {
}
```

小军的`Arrays.java`文件：

```
package mr.jun; // 申明包名mr.jun

public class Arrays {
}
```

在Java虚拟机执行的时候，JVM只看完整类名，因此，只要包名不同，类就不同。

包可以是多层结构，用`.`隔开。例如：`java.util`。

 要特别注意：包没有父子关系。java.util和java.util.zip是不同的包，两者没有任何继承关系。

没有定义包名的`class`，它使用的是默认包，非常容易引起名字冲突，因此，不推荐不写包名的做法。

我们还需要按照包结构把上面的Java文件组织起来。假设以`package_sample`作为根目录，`src`作为源码目录，那么所有文件结构就是：

```ascii
package_sample
└─ src
    ├─ hong
    │  └─ Person.java
    │  ming
    │  └─ Person.java
    └─ mr
       └─ jun
          └─ Arrays.java
```

即所有Java文件对应的目录层次要和包的层次一致。

编译后的`.class`文件也需要按照包结构存放。如果使用IDE，把编译后的`.class`文件放到`bin`目录下，那么，编译的文件结构就是：

```ascii
package_sample
└─ bin
   ├─ hong
   │  └─ Person.class
   │  ming
   │  └─ Person.class
   └─ mr
      └─ jun
         └─ Arrays.class
```

###### 包作用域

位于同一个包的类，可以访问包作用域的字段和方法。不用`public`、`protected`、`private`修饰的字段和方法就是包作用域。例如，`Person`类定义在`hello`包下面：

```
package hello;

public class Person {
    // 包作用域:
    void hello() {
        System.out.println("Hello!");
    }
}
```

`Main`类也定义在`hello`包下面：

```
package hello;

public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        p.hello(); // 可以调用，因为Main和Person在同一个包
    }
}
```

###### import

在一个`class`中，我们总会引用其他的`class`。例如，小明的`ming.Person`类，如果要引用小军的`mr.jun.Arrays`类，他有三种写法：

第一种，直接写出完整类名，例如：

```
// Person.java
package ming;

public class Person {
    public void run() {
        mr.jun.Arrays arrays = new mr.jun.Arrays();
    }
}
```

很显然，每次写完整类名比较痛苦。

因此，第二种写法是用`import`语句，导入小军的`Arrays`，然后写简单类名：

```
// Person.java
package ming;

// 导入完整类名:
import mr.jun.Arrays;

public class Person {
    public void run() {
        Arrays arrays = new Arrays();
    }
}
```

在写`import`的时候，可以使用`*`，表示把这个包下面的所有`class`都导入进来（但不包括子包的`class`）：

```
// Person.java
package ming;

// 导入mr.jun包的所有class:
import mr.jun.*;

public class Person {
    public void run() {
        Arrays arrays = new Arrays();
    }
}
```

我们一般不推荐这种写法，因为在导入了多个包后，很难看出`Arrays`类属于哪个包。

###### 使用其他类的规则

==使用其他类时，需要使用全类名。==

![image-20221122230630901](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122230630901.png)

==倒包（import）==

![image-20221122230739196](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221122230739196.png)

**使用其他类的规则**

**1.使用同一个包中的类时，不需要导包。**
**2.使用java.lang包中的类时，不需要导包。（字符串类）。**
**3.其他情况都需要导包。**
**4.如果同时使用两个包中的同名类，需要用全类名。**

==自动倒包==

**当无法识别该类时，即需要倒包时，选中写下的类，alt+回车自动倒包**

###### final

==最终的，不能被改变的，可修饰方法，类和变量==

**方法：表明该方法是最终方法，不能被重写**

**类：表明该类是最终类，不能被继承**

**变量：叫做常量，只能被赋值一次**

###### 常量

实际开发中，常量一般作为系统的配置信息，方便维护，提高可读性。
常量的命名规范：
==**单个单词：全部大写**==
==**多个单词：全部大写，单词之间用下划线隔开**==
细节：
**final修饰的变量是基本类型：那么变量存储的数据值不能发生改变。**
**final修饰的变量是==引用类型：那么变量存储的地址值不能发生改变，对象内部的可以改变。==**

![image-20221123103130027](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221123103130027.png)

---

---

---

## 权限修饰符(作用域)

**用来控制一个成员能够被访问的范围**

**可以修饰成员变量，方法，构造方法，内部类**

###### 权限修饰符的分类

==四种作用范围由小到大（public > protected > 空着不写 >private）==

![image-20221123104039930](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221123104039930.png)

**public**

定义为`public`的`class`、`interface`可以被其他任何类访问。

定义为`public`的`field`、`method`可以被其他类访问，前提是首先有访问`class`的权限。

**private**

定义为`private`的`field`、`method`无法被其他类访问。

`private`访问权限被限定在`class`的内部，而且与方法声明顺序*无关*。

由于Java支持嵌套类，如果一个类内部还定义了嵌套类，那么，嵌套类拥有访问`private`的权限。

**protected**

`protected`作用于继承关系。定义为`protected`的字段和方法可以被子类访问，以及子类的子类：

###### 权限修饰符的使用规则

**实际开发中，一般只用private和public**
		==1.成员变量私有==
		==2.方法公开==
**特例：如果方法中的代码是抽取其他方法中共性代码，这个方法一般也私有。**

###### 代码块

**局部代码块，构造代码块，静态代码块**

- 局部代码块：在方法中出现，限定变量生命周期，及早释放，提高内存利用率

  ![image-20221123110316848](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221123110316848.png)

- 构造代码块（初始化块）:在类中方法外出现，随着对象的创建而加载，创建一次对象构造代码块执行一次

- ==**静态代码块：静态代码块：随着类的加载而加载，并且只执行一次主方法类中的静态代码块：优先于主方法执行**==
  ==**static{}**==

---

---

---

## 抽象类

###### 定义：

抽象方法：**将共性的行为（方法)抽取到父类之后。由于每一个子类执行的内容是不一样，**
**所以，在父类中不能确定具体的方法体。==抽象方法，是指没有方法体的方法==**
==抽象类：如果一个类中存在抽象方法，那么该类就必须声明为抽象类==

###### **抽象类和抽象方法的定义格式**

**抽象方法的定义格式：（abstract）**
**public abstract返回值类型方法名（参数列表）；**
**抽象类的定义格式：**
**public abstract class类名**

###### 抽象类和抽象方法的注意事项

- 抽象类不能实例化（不能创建对象）
- 抽象类中不一定有抽象方法，有抽象方法的类一定是抽象类
- 可以有构造方法（当创建子类对象时，给属性进行赋值）
- 抽象类的子类
  - **要么重写抽象类中的所有抽象方法**
  - **要么是抽象类**

###### 抽象类和抽象方法的意义

**疑问：把子类中共性的内容抽取到父类之后，由于方法体不确定，需要定义为抽象。子类使用时需要重写。那么我不抽取到父类，直接在子类写不是更节约代码？**

==强制子类必须按照这种格式进行书写==

---

---

---

## 接口

**接口：就是一种规则，是对行为的抽象**

**如果一个抽象类没有属性，所有方法全部都是抽象方法：就可以把该抽象类改写为接口：`interface`。**

###### 接口的定义和使用

- **接口用关键字interface来定义**

  **public interface 接口名{}**

- **接口不能实例化**

- **接口和类是实现关系，通过implements关键字表示**

  **public class 类名 implements 接口名{}**

- **接口的子类（实现类）**

  - **要么重写接口中的所有抽象方法**
  - **要么是抽象类**

**注意1.接口和类的实现关系，可以单实现，也可以多实现。**

**public class 类名 implements 接口名1，接口名2{}**

**注意2.实现类还可以在继承一个类的同时实现多个接口。**

**public class 类名 extends 父类 implements 接口名1，接口名2{}**

###### 接口中成员的特点

- **成员变量：**
  - **只能是常量**
  - **默认修饰符：public static final**

- **构造方法：没有**
- **成员方法：**
  - **只能是抽象方法**
  - **默认修饰符：public abstract**
  - **接口中只能定义抽象方法。**

###### 接口和类之间的关系

- 类和类的关系

  继承关系，只能单继承，不能多继承，但是可以多层继承

- 类和接口的关系

  实现关系，可以单实现，也可以多实现，还可以在继承一个类的同时实现多个接口

- 接口和接口的关系

  继承关系，可以单继承，也可以多继承（extends）

  细节：如果实现类实现了最下面的子接口，那么就需要重写所有的抽象方法

###### JDK8开始接口中新增的方法

- JDK7以前：接口中只能定义抽象方法
- JDK8的新特性：接口中可以定义有方法体的方法。（默认、静态）
- JDK9的新特性：接口中可以定义私有方法

==有方法体的方法意义：实现类就不需要立马修改了，等以后用到某个规则了，再重写就行了==

==默认方法==

- **允许在接口中定义默认方法，需要使用关键字default修饰**

  作用：**解决接口升级的问题（增加新方法）**

- 接口中默认方法的定义格式：

  - **格式：public default 返回值类型 方法名（参数列表）{}**
  - **范例：public default void show（）{}**

- 接口中默认方法的注意事项：

  - **默认方法不是抽象方法，所以不强制被重写。但是如果被重写，重写的时候去掉default关键字**
  - public可以省略，default不能省略
  - 如果实现了多个接口，多个接口中存在相同名字的默认方法，子类就必须对该方法进行重写

==静态方法==

- 允许在接口中定义静态方法，需要用static修饰
- 接口中静态方法的定义格式：
  - 格式：public static 返回值类型 方法名（参数列表）{}
  - 范例：public static void show（）{}
- 接口中静态方法的注意事项
  - 静态方法只能通过接口名调用，不能通过实现类名或者对象名调用。
  - public可以省略，static不能省略。

==私有方法==

**为接口内部提供服务（供抽象方法和默认方法调用），不需要外类访问。**

接口中私有方法的定义格式

**默认方法**

格式1：private 返回值类型 方法名（参数列表）{}

范例1：private void show(){}

**静态方法**

格式2：private static 返回值类型 方法名（参数类表）{}

范例2：private static void method(){}

###### 接口的应用

1.接口代表规则，是行为的抽象。想要让哪个类拥有一个行为，就让这个类实现对应的接口就可以了。
2.**当一个方法的参数是接口时，可以传递接口所有实现类的对象，这种方式称之为接口多态。**

###### 接口的引用

最近在学习java的过程中，遇到了一下代码。

```
代码1：

public interface Handler｛
  public void Hello();
｝
代码2：

import Handler;
public class OtherParser{
  Handler handler;
......
}
```

代码1说明了Handler是一个接口了，既接口不能直接实例化，必须经过实现类继承这个接口之后，实例化实现类。那为啥代码2可以直接声明Handler呢？原因是，代码2只是对Handler接口的引用（在对接口的引用时，采用的是实例化实现该接口的类，前提是你实现这个接口的类已经加上@Component注解，引用这个接口的时候才会自动注入相关的实现类），并不是实例化！

接口是永远不能被实例化的，而2中只是对接口做引用，并没有被实例化。
接口可以看成是高度抽象的抽象类，它描述的事物们所共有的方法（方法签名），也就是规定除了该接口的方法的调用参数与规则，仅仅而已，它的使用必须依赖于实现类。
例如：

```
 public class MyHandler implements Handler{
  public void Hellp(){
    System.out.println("my Handler implements");
  }  
 }
```

而在对接口的引用时，采用的是实例化实现该接口的类
Handler handler = new MyHander();

###### 适配器设计模式

设计模式（Design pattern)是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性、程序的重用性。

**简单理解：设计模式就是各种编写程序的套路**

**适配器设计模式：解决接口与接口实现类之间的矛盾问题。**

1.当一个接口中抽象方法过多，但是我只要使用其中一部分的
时候，就可以适配器设计模式
2.书写步骤：
		**编写中间类XXXAdapter,实现对应的接口**
		**对接口中的抽象方法进行空实现（方法体空着）让真正的实现类继承中间类，并重写需要用的方法
		为了避免其他类创建适配器类的对象，中间的适配器类用abstracti进行修饰**

---

---

---

## 内部类

类的五大成员：属性，方法，构造方法，代码块，内部类。

#### 内部类的定义

**写在一个类里面的类就叫做内部类**
**举例：在A类的内部定义B类，B类就被称为内部类**

![image-20221124151007126](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221124151007126.png)

==内部类的访问特点：==

**内部类可以直接访问外部类的成员，包括私有。**

**外部类要访问内部类的成员，必须创建对象。**

#### 内部类的分类

- 成员内部类
- 静态内部类
- 局部内部类
- 匿名内部类

###### 成员内部类

==成员内部类的代码如何书写==

- 写在成员位置的，属于外部类的成员

- 成员内部类可以被一些修饰符所修饰，比如：private，默认，protected，public，static等
- 在成员内部类里面，JDK16之前不能定义静态变量，JDK16开始才可以定义静态变量。

==如何创建成员内部类的对象==

方法1：

在外部类中编写方法，对外提供内部类的对象。（private修饰）

![image-20221124192729622](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221124192729622.png)

用object接收；

方法2：

直接创建格式：外部类名.内部类名 对象名 = 外部类对象.内部类对象；

范例：car.engine s = new car().new engine();

==成员内部类如何获取外部类的成员变量==

![image-20221124193946297](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221124193946297.png)

outer.this.         外部类的变量

outer是外部类的名字

###### 静态内部类

静态内部类只能访问外部类中的静态变量和静态方法，如果想要访问非静态的需要创建对象。

==创建静态内部类对象的格式：==

外部类名.内部类名 对象名 = new外部类名.内部类名();

==调用非静态方法的格式：==
先创建对象，用对象调用

==调用静态方法的格式：==

外部类名.内部类名.方法名();

###### 局部内部类

1. 将内部类定义在方法里面就叫做局部内部类，类似于方法里面的局部变量。
2. 外界是无法直接使用的，需要在方法内部创建对象并使用。
3. 该类可以直接访问外部类的成员，也可以访问方法内的局部变量。

###### 匿名内部类

匿名内部类是隐藏了名字的内部类，本质上是一个对象，是实现了该接口或继承了该抽象类的子类对象。

![image-20221124202205187](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221124202205187.png)

==为什么要使用匿名内部类==

在实际开发中，我们常常遇到这样的情况：**一个接口/类的**方法的**某个实现方式**在程序中**只会执行一次**，但为了使用它，我们需要创建它的实现类/[子类](https://so.csdn.net/so/search?q=子类&spm=1001.2101.3001.7020)去实现/重写。此时可以使用匿名内部类的方式，可以无需创建新的类，**减少代码冗余**。

【匿名内部类】是省略了 <实现类 / 子类>

**在创建对象是，只能使用唯一一次，一般用于书写接口的实现类。**

```java
/**
 * 格式：
 *      接口名称  对象名 = new 接口名称(){
 *          // 覆盖重写所有抽象方法
 *      };
 */
public class AnonymityTest2 {
    public static void main(String[] args) {
        MyInteface my = new MyInteface(){
            @Override
            public void method() {
                System.out.println("匿名内部类方法");
            }
        };
        my.method();
    }
}
```

对于"new MyInteface(){...};" 的解析：

 1.new  代表对象创建的动作；

2.接口名称   【匿名内部类】要实现的接口；

3.{...}   这才是【匿名内部类】的内容，里面重写着接口的所有抽象方法。它光秃秃的，的确没名没姓的。

4.而 MyInteface my = new MyInteface(){...} 中的 my 是对象名，它是供你调用匿名类方法的对象。

【匿名对象】是省略了 <对象名称>

**表示，在调用方法时，只能调用唯一一次。**

```java
public class AnonymityTest {
    public static void main(String[] args) {
        fun1();
    }

    private static void fun1() {
        // 对于 Thread 来说，这就是【匿名对象】
        // 对于 Runnable 来说，这就是【匿名内部类】
        new Thread( new Runnable(){
            @Override
            public void run() {

            }
        }).start();
    }
}

```

## Java核心类

#### Object类

**==Object是所有类的父类==**，如果一个类没有使用extends关键词标明继承另一个类，那么这个类就默认继承object类。所以==**Object类中的所有方法适用于所有类**==

```java
public class Person{}
```

等价于

```java
public class Person extends Objects{}
```

Object类的方法

![image-20221213221738671](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221213221738671.png)

###### toString()和equals()

**toString()方法**

- 在`Object`类里面定义`toString()`方法的时候返回的对象的哈希code码(对象地址字符串)；
- 可以通过重写`toString()`方法表示出对象的属性。

Object:返回对象地址字符串

```java
public class TestToStringDemo1 {
    public static void main(String[] args) {
        Person p = new Person();
        System.out.println(p);
    }
}	//输出结果：educoder.Person@7852e992
class Person extends Object {
    String name = "张三";
    int age = 18;
}
```

重写：复写了Object类中的toString()方法（快捷键alt + insert）

```java
public class TestToStringDemo2 {
    public static void main(String[] args) {
        Person p = new Person();
        System.out.println(p);
    }
}
class Person extends Object {
    String name = "张三";
    int age = 18;
    // 复写Object类中的toString()方法
    public String toString() {
        return "我是:" + this.name + ",今年:" + this.age + "岁";
    }
}		//输出结果：我是:张三,今年:18岁
```

**equals()方法**

Object：**==比较的是对象的引用是否指向同一块内存地址==**

```java
public class test {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.name = "jack";
        Dog dog1 = new Dog();
        dog1.name = "jack";
        System.out.println(dog);
        System.out.println(dog1);
        if (dog.equals(dog1)) {
            System.out.println("两个对象是相同的");
        } else {
            System.out.println("两个对象是不相同的");
        }
    }
}
class Animal {
}
class Dog extends Animal {
    int age = 20;
    String name = "rose";
    public String toString() {
        return "Dog [age=" + age + ", name=" + name + "]";
    }
}		
/*输出结果：Dog[age = 20,name = jack]
          Dog[age = 20,name = jack]
          两个对象是不相同的*/
		
```

**两个对象分别new了一次，开辟了两个不同的内存空间，故内存地址不同**

**重写：判断对象的属性值相等（快捷方式alt + insert）**

```java
public class test {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.name = "jack";
        Dog dog1 = new Dog();
        dog1.name = "jack";
        System.out.println(dog);
        System.out.println(dog1);
        if (dog.equals(dog1)) {
            System.out.println("两个对象是相同的");
        } else {
            System.out.println("两个对象是不相同的");
        }
    }
}
class Animal {
}
class Dog extends Animal {
    int age = 20;
    String name = "rose";
    public String toString() {
        return "Dog [age=" + age + ", name=" + name + "]";
    }
    /* getClass() 得到的是一个类对象 */
    @Override
    public boolean equals(Object obj) {
        if (this == obj)// 两个对象的引用是否相同，如果相同，说明两个对象就是同一个
            return true;
        if (obj == null)// 如果比较对象为空，不需要比较，肯定不相等
            return false;
        if (getClass() != obj.getClass())// 比较两个对象的类型是否相同，如果不同，肯定不相同
            return false;
        Dog other = (Dog) obj;// 转化成相同类型后，判断属性值是否相同
        if (name == null) {
            if (other.name != null)
                return false;
        } else if (!name.equals(other.name))
            return false;
        return true;
    }
}
```

###### java对象克隆

Object 类的clone()方法用于克隆对象。java.lang.Cloneable接口必须由我们要创建其对象克隆的类实现。如果我们不实现Cloneable接口，clone方法将生成CloneNoteSupportedException错误。

```java
package educoder;
public class Student implements Cloneable {
    int rollno;
    String name;
    Student(int rollno, String name) {
        this.rollno = rollno;
        this.name = name;
    }
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
    public static void main(String args[]) {
        try {
            Student s1 = new Student(101, "amit");
            Student s2 = (Student) s1.clone();
            System.out.println(s1.rollno + " " + s1.name);
            System.out.println(s2.rollno + " " + s2.name);
        } catch (CloneNotSupportedException c) {
        }
    }
}
```

**clone()将对象的值复制到另一个对象**

**==throw和throws==**

throw：代表动作，表示抛出一个异常的动作。用在方法实现中。只能用于抛出一种异常。

throws：代表一种状态，代表方法可能有异常抛出。用在方法声明中，可抛出多个异常。

---

#### 字符串

###### string 

在java中，**string是一个引用类型，它本身是一个class。**

java中可以直接表示字符串

```java
String str = "hellow";
```

实际上**字符串在string内部是通过一个char[]数组表示的。**

**==java字符串不可变==**，这种特点是通过内部的private final char[] 字段，以及没有任何修改char[]的方法实现的。

###### 字符串比较

**==两个字符串比较，必须使用equals方法。（不能使用\==）==**

```java
public class main{
    public void main(Sting args){
        String s1 = "hellow";
        String s2 = "hell";
        System.out.println(s1.equals(s2));
    }
}
```

基本数据类型byte,short,char,int,long,float,double,boolean。他们之间的比较，应用双等号（==）,比较的是他们的值。

引用数据类型：当他们用（==）进行比较的时候，比较的是他们在内存中的存放地址（确切的说，是堆内存地址）。在这点和object提供的equals是一样的。

注：对于第二种类型，除非是同一个new出来的对象，他们的比较后的结果为true，否则比较后结果为false。因为每new一次，都会重新开辟堆内存空间。

**==忽略大小写比较，使用equalsIgnoreCase()方法方法。==**

###### 搜索子串、提取子串

**是否包含子串**

boolean contains(CharSequence s)

```java
//是否包含子串
"hellow".contains("w");	//true
```

**==contains()方法的参数是CharSequence(字符序列)而不是String==**

**搜索子串**

返回指定子字符串在此字符串中第一次出现处的索引

int indexOf(String str)

```java
"hello".indexOf("l");   //2
```

返回指定子字符串在此字符串中最右边出现处的索引

int lastIndexOf(String str)

```java
"hello".lastIndexOf("l");	//3
```

测试此字符串是否以指定的前缀开始

boolean startsWith(String prefix)

```java
"hello".startsWith("he");	//true
```

测试此字符串是否以指定的后缀结束

boolean endsWith(String suffix)

```java
"hello".endWith("lo")	//true
```

**提取子串**

substring(int beginIndex)

返回从起始位置（beginIndex）至字符串末尾的字符串

```java
"Hello".substring(2); // "llo"
```

substring(int beginIndex, int endIndex)

返回从起始位置（beginIndex）到目标位置（endIndex）之间的字符串，但不包含目标位置（endIndex）的字符串

```java
"Hello".substring(2, 4); "ll"
```

**==注意索引号是从0开始的==**

###### 去处首尾空白字符

trim()

移除字符串首尾空白字符。

**==空白字符包括空格,\t,\r,\n==**

```c
"\tHellow\r\n".trim;	//Hellow
```

**==注意：trim没有改变字符串的内容，而是返回了一个新字符串。==**

strip()

移除字符串首尾空白字符。它和trim不同的是，类似中文的空格字符\u3000也会被移除

stripLeading()

只删除字符串开头的空格

stripTrailing()

只删除字符串开头的空格

```java
"\u3000Hello\u3000".strip(); // "Hello"
" Hello ".stripLeading(); // "Hello "
" Hello ".stripTrailing(); // " Hello"
```

isEmpty()

判断字符串是否为空（字符串长度为0）

isBlack()

判断字符串是否为空白字符串（只包含空白字符）

```java
"".isEmpty(); // true，因为字符串长度为0
"  ".isEmpty(); // false，因为字符串长度不为0
"  \n".isBlank(); // true，因为只包含空白字符
" Hello ".isBlank(); // false，因为包含非空白字符
```

###### 替换子串

**==根据字符或字符串替换==**

public String replace(char oldChar, char newChar)

```
String s = "hello";
s.replace('l', 'w'); // "hewwo"，所有字符'l'被替换为'w'
```

**==通过正则表达式替换==**

public String replaceAll(String reges String replacement)

```
String s = "A,,B;C ,D";
s.replaceAll("[\\,\\;\\s]+", ","); // "A,B,C,D"
```

###### 拼接字符串

拼接字符串使用静态方法join(),它用指定的字符串连接字符串数组

```java
String []arr = {"A","B","C"};
String s = String.join("***",arr);
//"A***B***C"
```

###### 格式化字符串

字符串提供了formatted()方法和format()静态方法，可以传入其他参数，替换站位符，然后生成新的字符串。

```java
public class Main {
    public static void main(String[] args) {
        String s = "Hi %s, your score is %d!";	
        System.out.println(s.formatted("Alice", 80));	//Hi Allice, your score is 80!
        System.out.println(String.format("Hi %s, your score is %.2f!", "Bob", 59.5));	//Hi Bob, your score is 59.5!
    }
}
```

- %s：显示字符串；
- %d：显示整数；
- %x：显示十六进制整数；
- %f：显示浮点数。

占位符还可以带格式，例如`%.2f`表示显示两位小数。如果你不确定用啥占位符，那就始终用`%s`，因为`%s`可以显示任何数据类型。

###### 类型转换

要把任意基本类型或引用类型转换为字符串，可以使用静态方法`valueOf()`。这是一个重载方法，编译器会根据参数自动选择合适的方法：

```java
String.valueOf(123); // "123"
String.valueOf(45.67); // "45.67"
String.valueOf(true); // "true"
```

 **==把字符串转换为其他类型==**

**把字符串转换为int类型**

```java
int n1 = Integer.parseInt("123"); // 123
int n2 = Integer.parseInt("ff", 16); // 按十六进制转换，255
```

**把字符串转换为boolean类型**

```java
boolean b1 = Boolean.parseBoolean("true"); // true
boolean b2 = Boolean.parseBoolean("FALSE"); // false
```

**转换为char[]**

```java
char[] cs = "Hello".toCharArray(); // String -> char[]
String s = new String(cs); // char[] -> String
```

#### String类

[Java-String类常用方法汇总_java string 常用方法-CSDN博客](https://blog.csdn.net/weixin_45265547/article/details/124420143?ops_request_misc=%7B%22request%5Fid%22%3A%22170167979016800182717650%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=170167979016800182717650&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-2-124420143-null-null.142^v96^pc_search_result_base8&utm_term=string类的常用方法&spm=1018.2226.3001.4187)

---

#### StringBuilder

**==Java编译器对String做了特殊处理，使得我们可以直接用“+”拼接字符串。==**

```java
String s = "";
for (int i = 0; i < 1000; i++) {
    s = s + "," + i;
}
```

虽然可以直接拼接字符串，但是，在循环中，每次循环都会创建新的字符串对象，然后扔掉旧的字符串。这样，绝大部分字符串都是临时对象，不但浪费内存，还会影响GC效率。

Java标准库提供了StringBuilder类，它是一个可变对象，可以预分配缓冲区，这样，往StringBuilder中新增字符时，不会创建新的临时对象：

```java
StringBuilder sb = new StringBuilder(1024);
for (int i = 0; i < 1000; i++) {
    sb.append(',');
    sb.append(i);
}
String s = sb.toString();
```

`StringBuilder`还可以进行链式操作：

```java
public class Main {
    public static void main(String[] args) {
        var sb = new StringBuilder(1024);
        sb.append("Mr ")
          .append("Bob")
          .append("!")
          .insert(0, "Hello, ");
        System.out.println(sb.toString());
    }
}
```

如果我们查看`StringBuilder`的源码，可以发现，进行链式操作的关键是，定义的`append()`方法会返回`this`，这样，就可以不断调用自身的其他方法。

###### 构造方法

创建为空

```java
StringBuilder str = new StringBuilder();
```

在创建时添加初始字符串

```java
StringBuilder str = new StringBuilder("abc");
```

在创建时添加初始长度

```java
StringBuilder str = new StringBuilder(初始长度);
```

###### 成员方法

**public StringBuilder append(任意类型)**

**追加数据：给原有的字符串尾部加入新字符串**

```java
str.append("just");
```

**insert()**

**向指定位置插入数据**

每次加入新字符串之后都会改变字符串中字符的地址。插入后原来指定位置的数据向后移。

```java'
str.insert(0,"you");
```

**deleteCharAt()**

**删除指定位置的数据**

```java
str.deleteCharAt(index);
```

**delete()**

**删除指定范围的数据左闭右开**

```java
str.delete(beginIndex,endIndex);
```

**public int length()**

**返回长度（字符出现的个数）**



**public String toString()**

**返回一个字符串（拼接后的结果），实现将StringBuilder转换为String**。



**public StringBuilder reverse()**

**返回相反的字符序列**

---

#### StringJoiner

###### 构造方法

指定拼接时的间隔符号

**public StringJoiner(间隔符号)**

```java
StringJoiner str = new StringJoiner(",");
```

指定拼接时的间隔符号、开始符号、结束符号

**public StringJoiner(间隔符号，开始符号，结束符号)**

```java
StringJoiner str = new StringJoiner(",","[","]");
```

###### 成员方法

**public StringJoiner add(添加的内容)**

**添加数据，并返回对象本身**

**public int length()**

**返回长度(字符出现的个数)**

**public String toString**

**返回一个字符串（拼接之后的结果），实现将StringJoiner转换为String。**

![image-20221214230123642](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221214230123642.png)

**aaa---bbb---ccc**

![image-20221214230232191](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221214230232191.png)

**=aaa,bbb,ccc.**

###### 链式操作

当我们在调用一个方法时，不需要用变量接收它的结果，可以继续调用其他方法

```java
public class Main {
    public static void main(String[] args) {
        var sb = new StringBuilder(1024);
        sb.append("Mr ").append("Bob").append("!").insert(0, "Hello, ");
    System.out.println(sb.toString());
    }
}
```

**进行链式操作的关键是，定义的append()方法会返回this，这样，就可以不断调用自身的其他方法。(add同理)**

#### 包装类型

Java的数据类型分两种：

基本类型：byte , short , int , long , boolean , float , double , char

引用类型：所有class和interface类型

**引用类型可以赋值为null，表示空，但基本类型不能赋值为null。**

**==如何把一个基本类型视为对象（引用类型）==**

**例如，要想把int基本类型变成一个引用类型，我们可以定义一个integer类，它只包含了一个实施字段int，这样，integer类就可视为int的包装类。（Wrapper  Class）**

```java
public class Integer{
    private int value;
    public Integer(int value){
        this.value = value;
    }
    public int intValue(){
        return this.value;
    }
}
```

定义好了Integer类，我们可以把int和integer类互相转换。

```java
Integer n = null;
Integer n2 = new Integer(99);
int n3 = n2.intValue();
```

Java核心库为每种基本类型都提供了对应的包装类型

| 基本类型 | 对应的引用类型      |
| -------- | ------------------- |
| boolean  | java.lang.Boolean   |
| byte     | java.lang.Byte      |
| short    | java.lang.Short     |
| int      | java.lang.Integer   |
| long     | java.lang.Long      |
| float    | java.lang.Float     |
| double   | java.lang.Double    |
| char     | java.lang.Character |

###### 创建实例

```java
public class Main{
    public static void main(String[] args){
        int i 100;
        //通过new操作符创建Integer实例（不推荐使用，会有编译警告）；
        Integer n1 = new Integer(i);
        //通过静态方法valueOf(int)创建Integer实例；
        Integer n2 = Integer.valueOf(i);
        //通过静态方法valueOf(String)创建Integer实例
        Integer n3 = Integer.valueOf("100");
    }
}
```

###### 自动装箱（Auto Boxing）

**==int和Integer可以互相转换：==**

```java
int i = 100;
Integer n = Integer.valueOf(i);
int x = n.intValue();
```

**==Java编译器可以帮助我们在int 和Integer之间转型==**

```java
Integer n = 100;	//编译器自动使用Integer.valueOf(int)
int x = n;			//编译器自动使用Integer.intValue()
```

**==这种直接把int变为Integer的赋值写法，称为自动装箱（Auto Unboxing），反过来，把Integer变为int的赋值写法，称为自动拆箱（Auto Unboxing）。==**

###### 不变类

**所有的包装类型都是不变类。Integer的源码为：**

```java
public final class Integer{
    private final int value;
}
```

**==一旦创建了Integer对象，该对象就是不可变的。==**

对两个Integer实例进行比较要注意：**绝对不能用==比较，应为Integer是引用类型，必须使用equals()比较。**

**我们自己创建Integer的时候，有以下两个方法**

- 方法1：`Integer n = new Integer(100);`
- 方法2：`Integer n = Integer.valueOf(100);`

方法2更好，这种方法就是静态工厂方法，它尽可能地返回缓存的实例以节约内存。

###### 进制转换

Integer类中的静态方法parseInt()可以把字符串解析成一个整数：

```java
int x1 = Integer.parseInt("100");	//x1 = 100,因为按10进制解析
int x2 = Integer.parseInt("100",16);
	//x2 = 256,因为按16进制解析
```

Integer还可以把整数格式化为指定进制的字符串。

```java
public class Main {
    public static void main(String[] args) {
        System.out.println(Integer.toString(100)); // "100",表示为10进制
        System.out.println(Integer.toString(100, 36)); // "2s",表示为36进制
        System.out.println(Integer.toHexString(100)); // "64",表示为16进制
        System.out.println(Integer.toOctalString(100)); // "144",表示为8进制
        System.out.println(Integer.toBinaryString(100)); // "1100100",表示为2进制
    }
}
```

**==注意：上述方法的输出都是String==**

Java的包装类型还定义了一些有用的静态变量

```java
// boolean只有两个值true/false，其包装类型只需要引用Boolean提供的静态字段:
Boolean t = Boolean.TRUE;
Boolean f = Boolean.FALSE;
// int可表示的最大/最小值:
int max = Integer.MAX_VALUE; // 2147483647
int min = Integer.MIN_VALUE; // -2147483648
// long类型占用的bit和byte数量:
int sizeOfLong = Long.SIZE; // 64 (bits)
int bytesOfLong = Long.BYTES; // 8 (bytes)
```

最后，所有的整数和浮点数的包装类型都继承自`Number`，因此，可以非常方便地直接通过包装类型获取各种基本类型：

```java
// 向上转型为Number:
Number num = new Integer(999);
// 获取byte, int, long, float, double:
byte b = num.byteValue();
int n = num.intValue();
long ln = num.longValue();
float f = num.floatValue();
double d = num.doubleValue();
```

###### 处理无符号整型

在Java中，并没有无符号整型（Unsigned）的基本数据类型。byte、short、int和long都是带符号整型，最高位是符号位。而C语言则提供了CPU支持的全部数据类型，包括无符号整型。**无符号整型和有符号整型的转换在Java中就需要借助包装类型的静态方法完成。**

```java
public class Main {
    public static void main(String[] args) {
        byte x = -1;
        byte y = 127;
        System.out.println(Byte.toUnsignedInt(x)); // 255
        System.out.println(Byte.toUnsignedInt(y)); // 127
    }
}
```

因为byte的-1的二进制表示是11111111，以无符号整型转换后的int就是255。

类似的，可以把一个short按unsigned转换为int，把一个int按unsigned转换为long。

#### Random

Random类位于java.util包下，**Random类中实现的随机算法是伪随机，也就是有规则的随机。在进行随机时**，随机算法的起源数字成为种子数（seed），在种子数的基础上进行一定的变换，从而产生需要的随机数字。

**相同种子数的Random对象，相同次数生成的随机数字是完全相同的。**这点在生成多个随机数字时需要特别注意。

###### 构造方法

1.

```java
public Random()
```

**该构造方法使用一个和当前系统时间对应的相对时间有关的数字作为种子数，然后使用这个种子数构造Random对象。**

```java
Random r = new Random();
```

2.

```java
public Random(long seed)
```

**该构造方法可以通过制定一个种子数进行创建**

```java
public r = new Random(10); 
```

###### Random类中的常用方法

1. public boolean nextBoolean()：是生成一个随机的boolean值，生成true和false的值几率相等，也就是都是50%的几率。

2. public double nextDouble()：是生成一个随机的double值，数值介于[0,1.0)之间。

3. public int nextInt()：是生成在-231到231-1之间int值。如果需要生成指定区间的int值，则需要进行一定的数学变换，具体可以参看下面的使用示例中的代码。

4. public int nextInt(int n)：是生成一个介于[0,n)的区间int值，包含0而不包含n。如果想生成指定区间int值，也需要进行一定的数学变换，具体参看下面的使用示例中的代码。

5. public void setSeed(long seed)：是重新设置Random对象中的种子数。设置完种子数以后的Random对象和相同种子数使用new关键字创建出的Random对象相同。

6. public float nextFloat(int n)：返回下一个伪随机数，它是取自此随机数生成器序列的、在 0.0 和 1.0 之间均匀分布的 float 值。

7. public long nextLong():返回下一个伪随机数，它是取自此随机数生成器序列的均匀分布的 long 值。

8. public double nextGaussian():返回下一个伪随机数，它是取自此随机数生成器序列的、呈高斯（“正态”）分布的 double 值，其平均值是 0.0，标准差是 1.0。

#### SecureRandom

有伪随机数，就有真随机数。实际上真正的真随机数只能通过量子力学原理来获取，而我们想要的是一个不可预测的安全的随机数，`SecureRandom`就是用来创建安全的随机数的：

```
SecureRandom sr = new SecureRandom();
System.out.println(sr.nextInt(100));
```

`SecureRandom`无法指定种子，它使用RNG（random number generator）算法。JDK的`SecureRandom`实际上有多种不同的底层实现，有的使用安全随机种子加上伪随机数算法来产生安全的随机数，有的使用真正的随机数生成器。实际使用的时候，可以优先获取高强度的安全随机数生成器，如果没有提供，再使用普通等级的安全随机数生成器：

```java
import java.util.Arrays;
import java.security.SecureRandom;
import java.security.NoSuchAlgorithmException;
public class Main {
    public static void main(String[] args) {
        SecureRandom sr = null;
        try {
            sr = SecureRandom.getInstanceStrong(); // 获取高强度安全随机数生成器
        } catch (NoSuchAlgorithmException e) {
            sr = new SecureRandom(); // 获取普通的安全随机数生成器
        }
        byte[] buffer = new byte[16];
        sr.nextBytes(buffer); // 用安全随机数填充buffer
        System.out.println(Arrays.toString(buffer));
    }
}
```

`SecureRandom`的安全性是通过操作系统提供的安全的随机种子来生成随机数。这个种子是通过CPU的热噪声、读写磁盘的字节、网络流量等各种随机事件产生的“熵”。

在密码学中，安全的随机数非常重要。如果使用不安全的伪随机数，所有加密体系都将被攻破。因此，时刻牢记必须使用`SecureRandom`来产生安全的随机数。

#### Math

###### 求绝对值：

```java
Math.abs(-100);		//100
```

###### 取最大或最小值：

```java
Math.max(100,99);		//100
Math.min(1.2,2.3);		//1.2
```

###### 计算x^y^次方：

```java
Math.pow(2,10);		//2的十次方 = 1024
```

###### 计算$\sqrt{x}$   ：

```java
Math.sqrt(4);		//2
```

###### 计算e^x^次方：

```java
Math.log(2)			//7.389...
```

###### 计算以e为底的对数：

```java
Math.log(4); // 1.386...
```

###### 计算以10为底的对数：

```java
Math.log10(100); // 2
```

###### 三角函数：

```java
Math.sin(3.14); // 0.00159...
Math.cos(3.14); // -0.9999...
Math.tan(3.14); // -0.0015...
Math.asin(1.0); // 1.57079...
Math.acos(1.0); // 0.0
```

###### 数学常量：

```java
double pi = Math.PI; // 3.14159...
double e = Math.E; // 2.7182818...
Math.sin(Math.PI / 6); // sin(π/6) = 0.5
```

生成一个随机数x，x的范围是`0 <= x < 1`：

```
Math.random(); // 0.53907... 每次都不一样
```

如果我们要生成一个区间在`[MIN, MAX)`的随机数，可以借助`Math.random()`实现，计算如下：

```java
// 区间在[MIN, MAX)的随机数
public class Main {
    public static void main(String[] args) {
        double x = Math.random(); // x的范围是[0,1)
        double min = 10;
        double max = 50;
        double y = x * (max - min) + min; // y的范围是[10,50)
        long n = (long) y; // n的范围是[10,50)的整数
        System.out.println(y);
        System.out.println(n);
    }
}
```

#### Date类和SimpleDateFormat类的用法

###### Java日期时间

java.util包提供了Date类来封装当前的日期和时间。

构造方法：

**Date()：此种形式表示分配 Date 对象并初始化此对象，以表示分配它的时间（精确到毫秒），使用该构造方法创建的对象可以获取本地的当前时间。**

**Date(long date)：此种形式表示从 GMT 时间（格林尼治时间）1970 年 1 月 1 日 0 时 0 分 0 秒开始经过参数 date 指定的毫秒数。**

这两个构造方法的使用示例如下：

```java
Date date1 = new Date();    // 调用无参数构造函数
System.out.println(date1.toString());    // 输出：Wed May 18 21:24:40 CST 2016
Date date2 = new Date(60000);    // 调用含有一个long类型参数的构造函数
System.out.println(date2);    // 输出：Thu Jan 0108:01:00 CST 1970
```

![image-20221218121516477](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221218121516477.png)

  获取当前日期时间：

Java中获取当前日期和时间很简单，使用`Date`对象的`toString()`方法来打印当前日期和时间，如下所示：

```java
import java.util.Date;
public class DateDemo {
	public static void main(String args[]) {        // 初始化 Date 对象        
		Date date = new Date();        // 使用 toString() 函数显示日期时间        
		System.out.println(date.toString());   
		}
}
```

日期比较：

Java使用以下三种方法来比较两个日期：

- 使用getTime()方法获取两个日期自1970年1月1日经历的毫秒数值，然后比较这两个值；
- 使用方法before()，after()和equals()。例如，一个月的12号比18号早，则 new Date(99, 2, 12).before(new Date (99, 2, 18))返回true；
- 使用compareTo()方法，它是由Comparable接口定义的，Date 类实现了这个接口。

#### 枚举类

在Java中，我们可以通过static final来定义常量。

定义int型常量

```java
public class {
    public static final int SUN = 0;
    public static final int MON = 1;
    public static final int TUE = 2;
    public static final int WED = 3;
    public static final int THU = 4;
    public static final int FRI = 5;
    public static final int SAT = 6;
}
```

引用

```java
if (day == Weekday.SAT || day == Weekday.SUN) {
    // TODO: work at home
}
```

定义字符串常量

```java
public class Color{
    public static final String RED = "r";
    public static final String GREEN = "g";
    public static final String BLUE = "b";
}
```

引用

```java
String read = "r";
if(Color.RED.equals(color)){}
```

在Java中，我们可以通过`static final`来定义常量。例如，我们希望定义周一到周日这7个常量，可以用7个不同的`int`表示：

```
public class Weekday {
    public static final int SUN = 0;
    public static final int MON = 1;
    public static final int TUE = 2;
    public static final int WED = 3;
    public static final int THU = 4;
    public static final int FRI = 5;
    public static final int SAT = 6;
}
```

使用常量的时候，可以这么引用：

```
if (day == Weekday.SAT || day == Weekday.SUN) {
    // TODO: work at home
}
```

也可以把常量定义为字符串类型，例如，定义3种颜色的常量：

```
public class Color {
    public static final String RED = "r";
    public static final String GREEN = "g";
    public static final String BLUE = "b";
}
```

使用常量的时候，可以这么引用：

```
String color = ...
if (Color.RED.equals(color)) {
    // TODO:
}
```

无论是`int`常量还是`String`常量，使用这些常量来表示一组枚举值的时候，有一个严重的问题就是，编译器无法检查每个值的合理性。例如：

```
if (weekday == 6 || weekday == 7) {
    if (tasks == Weekday.MON) {
        // TODO:
    }
}
```

上述代码编译和运行均不会报错，但存在两个问题：

- 注意到`Weekday`定义的常量范围是`0`~`6`，并不包含`7`，编译器无法检查不在枚举中的`int`值；
- 定义的常量仍可与其他变量比较，但其用途并非是枚举星期值。

###### enum类型

enum类型和其他的class没有任何区别。enum定义的类型就是class，只不过它有以下几个特点：

- 定义的enum类型总是继承自java.lang.Enum，且无法被继承；
- 只能定义出enum的实例，而无法通过new操作符创建enum的实例；
- 定义的每个实例都是引用类型的唯一实例；
- 可以将enum类型用于switch语句。

例如，我们定义的Color枚举类：

```
public enum Color {
    RED, GREEN, BLUE;
}
```

为了让编译器能自动检查某个值在枚举的集合内，并且，不同用途的枚举需要不同的类型来标记，不能混用，我们可以使用`enum`来定义枚举类：

```java
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        if (day == Weekday.SAT || day == Weekday.SUN) {
            System.out.println("Work at home!");
        } else {
            System.out.println("Work at office!");
        }
    }
}

enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT;
}
```

注意到定义枚举类是通过关键字`enum`实现的，我们只需依次列出枚举的常量名。

和`int`定义的常量相比，使用`enum`定义枚举有如下好处：

首先，`enum`常量本身带有类型信息，即`Weekday.SUN`类型是`Weekday`，编译器会自动检查出类型错误。例如，下面的语句不可能编译通过：

```
int day = 1;
if (day == Weekday.SUN) { // Compile error: bad operand types for binary operator '=='
}
```

其次，不可能引用到非枚举的值，因为无法通过编译。

最后，不同类型的枚举不能互相比较或者赋值，因为类型不符。例如，不能给一个`Weekday`枚举类型的变量赋值为`Color`枚举类型的值：

```
Weekday x = Weekday.SUN; // ok!
Weekday y = Color.RED; // Compile error: incompatible types
```

这就使得编译器可以在编译期自动检查出所有可能的潜在错误。

###### enum的比较

使用`enum`定义的枚举类是一种引用类型。前面我们讲到，引用类型比较，要使用`equals()`方法，如果使用`==`比较，它比较的是两个引用类型的变量是否是同一个对象。因此，引用类型比较，要始终使用`equals()`方法，但`enum`类型可以例外。

这是因为`enum`类型的每个常量在JVM中只有一个唯一实例，所以可以直接用`==`比较：

```
if (day == Weekday.FRI) { // ok!
}
if (day.equals(Weekday.SUN)) { // ok, but more code!
}
```

###### name()

返回常量名，例如：

```
String s = Weekday.SUN.name(); // "SUN"
```

###### ordinal()

返回定义的常量的顺序，从0开始计数，例如：

```
int n = Weekday.MON.ordinal(); // 1
```

改变枚举常量定义的顺序就会导致ordinal()返回值发生变化。例如：

```
public enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT;
}
```

和

```
public enum Weekday {
    MON, TUE, WED, THU, FRI, SAT, SUN;
}
```

的ordinal就是不同的。如果在代码中编写了类似if(x.ordinal()==1)这样的语句，就要保证enum的枚举顺序不能变。新增的常量必须放在最后。

###### switch

最后，枚举类可以应用在switch语句中。因为枚举类天生具有类型信息和有限个枚举常量，所以比int、String类型更适合用在switch语句中：

```java
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        switch(day) {
        case MON:
        case TUE:
        case WED:
        case THU:
        case FRI:
            System.out.println("Today is " + day + ". Work at office!");
            break;
        case SAT:
        case SUN:
            System.out.println("Today is " + day + ". Work at home!");
            break;
        default:
            throw new RuntimeException("cannot process " + day);
        }
    }
}

enum Weekday {
    MON, TUE, WED, THU, FRI, SAT, SUN;
    //无参数，默认调用无参方法
}

```

- 先看一个需求
  要求创建季节(Season) 对象，请设计并完成。

  ```java
  public class Enumeration01 {
      public static void main(String[] args) {
          //使用
          Season spring = new Season("春天", "温暖");
          Season summer = new Season("夏天", "炎热");
          Season autumn = new Season("秋天", "凉爽");
          Season winter = new Season("冬天", "寒冷");
  
          //autumn.setName("XXX");
          //autumn.setDesc("非常的热..");
  
  //因为对于季节而已，它们的对象(具体值)，是固定的四个，不会有更多
  //这个设计类的思路，不能体现季节是固定的四个对象
  //因此，这样的设计不好===> 枚举类[枚: 一个一个。 举： 例举。 即把具体的对象一个一个例举出来的类，就称为枚举类]
  //Season other = new Season("白天", "光明");
  
      }
  }
  
  class Season {//类
      private String name;
      private String desc;//描述
  
      public Season(String name, String desc) {
          this.name = name;
          this.desc = desc;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public String getDesc() {
          return desc;
      }
  
      public void setDesc(String desc) {
          this.desc = desc;
      }
  }
  ```

- 分析问题
  创建 Season 对象有如下特点
  季节的值是有限的几个值(spring, summer, autumn, winter)
  只读，不需要修改。
- 解决方案-枚举
  枚举对应英文(enumeration, 简写 enum)
  枚举是一组常量的集合。
  可以这里理解：枚举属于一种特殊的类，里面只包含一组有限的特定的对象。
- 枚举的两种种实现方式
  自定义类实现枚举
  使用 enum 关键字实现枚举
- 4.1 自定义类实现枚举-应用案例
  1.不需要提供setXxx方法，因为枚举对象值通常为只读。
  2.对枚举 对象/属性 使用final + static共同修饰，实现底层优化。
  3.枚举对象名通常使用全部大写，常量的命名规范。
  4.枚举对象根据需要，也可以有多个属性。

  ```java
  public class Enumeration02 {
      public static void main(String[] args) {
  
          System.out.println(Season.SPRING);
          System.out.println(Season.SUMMER);
          System.out.println(Season.AUTUMN);
          System.out.println(Season.WINTER);
  
      }
  }
  
  //演示自定义枚举实现
  class Season {//类
      private String name;
      private String desc;//描述
  
      //定义了四个对象，固定值
      public static final Season SPRING = new Season("春天", "温暖");
      public static final Season SUMMER = new Season("夏天", "炎热");
      public static final Season AUTUMN = new Season("秋天", "凉爽");
      public static final Season WINTER = new Season("冬天", "寒冷");
  
  
      //1. 将构造器私有化，目的是防止 直接 new
      //2. 去掉 setXxx 方法，防止属性被修改
      //3. 在 Season 内部，直接创固定的对象
      //4. 优化下：可以加入 final 修饰符
      private Season(String name, String desc) {
          this.name = name;
          this.desc = desc;
      }
  
      public String getName() {
          return name;
      }
  
      public String getDesc() {
          return desc;
      }
  
      @Override
      public String toString() {
          return "Season{" +
                  "name='" + name + '\'' +
                  ", desc='" + desc + '\'' +
                  '}';
      }
  }
  ```


4.2 自定义类实现枚举–小结
小结：进行自定义类实现枚举，有如下特点：
	1.构造器私有化
	2.本类内部创建一组对象【四个： 春夏秋冬】
	3.对外暴露对象（通过为对象添加 public final static 修饰符）
	4.可以提供 get 方法，但是不要提供 set 方法
4.3 enum 关键字实现枚举–快速入门
说明：使用 enum 来实现前面的枚举案例，主要体会和自定义类实现枚举不同的地方。

```java
public class Enumeration03 {
    public static void main(String[] args) {

        System.out.println(Season.SPRING);
        System.out.println(Season.SUMMER);
        System.out.println(Season.AUTUMN);
        System.out.println(Season.WINTER);

    }
}

//演示enum关键字来实现枚举类
enum Season {//类

    //如果使用 enum 来实现枚举类
    //1. 使用 enum 来替代 class
    //2.  public static final Season SPRING = new Season("春天", "温暖"); 等价于
    // SPRING("春天", "温暖") 解读：常量名(实参列表)
    //3. 如果有多个常量(对象), 使用 "," 间隔
    //4. 如果使用 enum 来实现枚举，要求将定义常量对象，写在最前面
    SPRING("春天", "温暖"),
    SUMMER("夏天", "炎热"),
    AUTUMN("秋天", "凉爽"),
    WINTER("冬天", "寒冷");
    
    private String name;
    private String desc;//描述

    private Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }

    public String getName() {
        return name;
    }

    public String getDesc() {
        return desc;
    }

    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```
#### 记录类

使用String、Integer等类型的时候，这些类型都是不变类。

不变类的特点有：

1. 定义class时使用final，无法派生子类。
2. 每个字段使用final，保证创建实例后无法修改任何字段。

###### record

`record`关键词的引入，主要是为了提供一种更为简洁、紧凑的`final`类的定义方式。

`record`申明的类，具备这些特点：

1. 它是一个`final`类
2. 自动实现`equals`、`hashCode`、`toString`函数
3. 成员变量均为`public`属性

几种申明方式：

```java
//单独文件申明：
public record range(int start, int end){}
//在类内部申明：
public class DidispaceTest {
    public record range(int start, int end){}
}
1
2
//函数内申明：
public class DidispaceTest {
  public void test() {
    public record range(int start, int end){}
  }
}
```

## 集合

什么是集合（Collection）？集合就是“由若干个确定的元素所构成的整体”。例如，5只小兔构成的集合：

```ascii
┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐

│   (\_(\     (\_/)     (\_/)     (\_/)      (\(\   │
    ( -.-)    (•.•)     (>.<)     (^.^)     (='.')
│  C(")_(")  (")_(")   (")_(")   (")_(")   O(_")")  │

└ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┘
```

在数学中，我们经常遇到集合的概念。例如：

- 有限集合：
  - 一个班所有的同学构成的集合；
  - 一个网站所有的商品构成的集合；
  - ...
- 无限集合：
  - 全体自然数集合：1，2，3，……
  - 有理数集合；
  - 实数集合；
  - ...

为什么要在计算机中引入集合呢？这是为了便于处理一组类似的数据，例如：

- 计算所有同学的总成绩和平均成绩；
- 列举所有的商品名称和价格；
- ……

在Java中，如果一个Java对象可以在内部持有若干其他Java对象，并对外提供访问接口，我们把这种Java对象称为集合。很显然，Java的数组可以看作是一种集合：

```
String[] ss = new String[10]; // 可以持有10个String对象
ss[0] = "Hello"; // 可以放入String对象
String first = ss[0]; // 可以获取String对象
```

既然Java提供了数组这种数据类型，可以充当集合，那么，我们为什么还需要其他集合类？**这是因为数组有如下限制：**

- **数组初始化后大小不可变；**
- **数组只能按索引顺序存取。**

**因此，我们需要各种不同类型的集合类来处理不同的数据，例如：**

- **可变大小的顺序链表；**
- **保证无重复元素的集合；**
- **...**

#### Collection

Java标准库自带的`java.util`包提供了集合类：**`Collection`，它是除`Map`外所有其他集合类的根接口。**Java的`java.util`包主要提供了以下三种类型的集合：

- **`List`：一种有序列表的集合，例如，按索引排列的`Student`的`List`；**
- **`Set`：一种保证没有重复元素的集合，例如，所有无重复名称的`Student`的`Set`；**
- **`Map`：一种通过键值（key-value）查找的映射表集合，例如，根据`Student`的`name`查找对应`Student`的`Map`。**

Java集合的设计有几个特点：一是实现了接口和实现类相分离，例如，**有序表的接口是`List`，具体的实现类有`ArrayList`，`LinkedList`等，二是支持泛型，我们可以限制在一个集合中只能放入同一种数据类型的元素**，例如：

```
List<String> list = new ArrayList<>(); // 只能放入String类型
```

最后，**Java访问集合总是通过统一的方式——迭代器（Iterator）来实现**，它最明显的好处在于无需知道集合内部元素是按什么方式存储的。

#### List

在集合类中，`List`是最基础的一种集合：它是一种有序列表。

`List`的行为和数组几乎完全相同：`List`内部按照放入元素的先后顺序存放，每个元素都可以通过索引确定自己的位置，`List`的索引和数组一样，从`0`开始。

数组和`List`类似，也是有序结构，如果我们使用数组，在添加和删除元素的时候，会非常不方便。例如，从一个已有的数组`{'A', 'B', 'C', 'D', 'E'}`中删除索引为`2`的元素：

```ascii
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ C │ D │ E │   │
└───┴───┴───┴───┴───┴───┘
              │   │
          ┌───┘   │
          │   ┌───┘
          │   │
          ▼   ▼
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ D │ E │   │   │
└───┴───┴───┴───┴───┴───┘
```

这个“删除”操作实际上是把`'C'`后面的元素依次往前挪一个位置，而“添加”操作实际上是把指定位置以后的元素都依次向后挪一个位置，腾出来的位置给新加的元素。这两种操作，用数组实现非常麻烦。

因此，在实际应用中，需要增删元素的有序列表，我们使用最多的是`ArrayList`。实际上，`ArrayList`在内部使用了数组来存储所有元素。例如，一个`ArrayList`拥有5个元素，实际数组大小为`6`（即有一个空位）：

```ascii
size=5
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ C │ D │ E │   │
└───┴───┴───┴───┴───┴───┘
```

当添加一个元素并指定索引到`ArrayList`时，`ArrayList`自动移动需要移动的元素：

```ascii
size=5
┌───┬───┬───┬───┬───┬───┐
│ A │ B │   │ C │ D │ E │
└───┴───┴───┴───┴───┴───┘
```

然后，往内部指定索引的数组位置添加一个元素，然后把`size`加`1`：

```ascii
size=6
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ F │ C │ D │ E │
└───┴───┴───┴───┴───┴───┘
```

继续添加元素，但是数组已满，没有空闲位置的时候，`ArrayList`先创建一个更大的新数组，然后把旧数组的所有元素复制到新数组，紧接着用新数组取代旧数组：

```ascii
size=6
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ A │ B │ F │ C │ D │ E │   │   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

现在，新数组就有了空位，可以继续添加一个元素到数组末尾，同时`size`加`1`：

```ascii
size=7
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ A │ B │ F │ C │ D │ E │ G │   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

可见，`ArrayList`把添加和删除的操作封装起来，让我们操作`List`类似于操作数组，却不用关心内部元素如何移动。

**我们考察`List<E>`接口，可以看到几个主要的接口方法：**

- **在末尾添加一个元素：`boolean add(E e)`**
- **在指定索引添加一个元素：`boolean add(int index, E e)`**
- **删除指定索引的元素：`E remove(int index)`**
- **删除某个元素：`boolean remove(Object e)`**
- **获取指定索引的元素：`E get(int index)`**
- **获取链表大小（包含元素的个数）：`int size()`**

但是，实现`List`接口并非只能通过数组（即`ArrayList`的实现方式）来实现，另一种`LinkedList`通过“链表”也实现了List接口。在`LinkedList`中，它的内部每个元素都指向下一个元素：

```ascii
        ┌───┬───┐   ┌───┬───┐   ┌───┬───┐   ┌───┬───┐
HEAD ──>│ A │ ●─┼──>│ B │ ●─┼──>│ C │ ●─┼──>│ D │   │
        └───┴───┘   └───┴───┘   └───┴───┘   └───┴───┘
```

我们来比较一下`ArrayList`和`LinkedList`：

|                     | ArrayList    | LinkedList           |
| :------------------ | :----------- | :------------------- |
| 获取指定元素        | 速度很快     | 需要从头开始查找元素 |
| 添加元素到末尾      | 速度很快     | 速度很快             |
| 在指定位置添加/删除 | 需要移动元素 | 不需要移动元素       |
| 内存占用            | 少           | 较大                 |

==**通常情况下，我们总是优先使用`ArrayList`。**==

###### List的特点

**==使用`List`时，我们要关注`List`接口的规范。`List`接口允许我们添加重复的元素，即`List`内部的元素可以重复：==**

```
public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple"); // size=1
        list.add("pear"); // size=2
        list.add("apple"); // 允许重复添加元素，size=3
        System.out.println(list.size());
    }
}
```

`List`还允许添加`null`：

```
public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple"); // size=1
        list.add(null); // size=2
        list.add("pear"); // size=3
        String second = list.get(1); // null
        System.out.println(second);
    }
}
```

###### 创建List

除了使用`ArrayList`和`LinkedList`，我们还可以通过`List`接口提供的`of()`方法，根据给定元素快速创建`List`：

```
List<Integer> list = List.of(1, 2, 5);
```

但是`List.of()`方法不接受`null`值，如果传入`null`，会抛出`NullPointerException`异常。

###### 遍历List

和数组类型一样，我们要遍历一个`List`，完全可以用`for`循环根据索引配合`get(int)`方法遍历：

```java
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "pear", "banana");
        for (int i=0; i<list.size(); i++) {
            String s = list.get(i);
            System.out.println(s);
        }
    }
}
```

但这种方式并不推荐，一是代码复杂，二是因为`get(int)`方法只有`ArrayList`的实现是高效的，换成`LinkedList`后，索引越大，访问速度越慢。

所以我们要始终坚持使用迭代器`Iterator`来访问`List`。`Iterator`本身也是一个对象，但它是由`List`的实例调用`iterator()`方法的时候创建的。`Iterator`对象知道如何遍历一个`List`，并且不同的`List`类型，返回的`Iterator`对象实现也是不同的，但总是具有最高的访问效率。

**`Iterator`对象有两个方法：`boolean hasNext()`判断是否有下一个元素，`E next()`返回下一个元素。**因此，使用`Iterator`遍历`List`代码如下：

```java
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "pear", "banana");
        for (Iterator<String> it = list.iterator(); it.hasNext(); ) {
            String s = it.next();
            System.out.println(s);
        }
    }
}
```

有童鞋可能觉得使用`Iterator`访问`List`的代码比使用索引更复杂。但是，要记住，**通过`Iterator`遍历`List`永远是最高效的方式。并且，由于`Iterator`遍历是如此常用，所以，Java的`for each`循环本身就可以帮我们使用`Iterator`遍历。**把上面的代码再改写如下：

```java
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "pear", "banana");
        for (String s : list) {
            System.out.println(s);
        }
    }
}
```

实际上，只要实现了`Iterable`接口的集合类都可以直接用`for each`循环来遍历，Java编译器本身并不知道如何遍历集合对象，但它会自动把`for each`循环变成`Iterator`的调用，原因就在于`Iterable`接口定义了一个`Iterator<E> iterator()`方法，强迫集合类必须返回一个`Iterator`实例。

###### List和Array转换

把`List`变为`Array`有三种方法，第一种是**调用`toArray()`方法直接返回一个`Object[]`数组：**

```
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "pear", "banana");
        Object[] array = list.toArray();
        for (Object s : array) {
            System.out.println(s);
        }
    }
}
```

这种方法会丢失类型信息，所以实际应用很少。

第二种方式是**给`toArray(T[])`传入一个类型相同的`Array`，`List`内部自动把元素复制到传入的`Array`中：**

```
public class Main {
    public static void main(String[] args) {
        List<Integer> list = List.of(12, 34, 56);
        Integer[] array = list.toArray(new Integer[3]);
        for (Integer n : array) {
            System.out.println(n);
        }
    }
}
```

注意到这个`toArray(T[])`方法的泛型参数`<T>`并不是`List`接口定义的泛型参数`<E>`，所以，我们实际上可以传入其他类型的数组，例如我们传入`Number`类型的数组，返回的仍然是`Number`类型

```java
public class Main {
    public static void main(String[] args) {
        List<Integer> list = List.of(12, 34, 56);
        Number[] array = list.toArray(new Number[3]);
        for (Number n : array) {
            System.out.println(n);
        }
    }
}
```

但是，如果我们传入类型不匹配的数组，例如，`String[]`类型的数组，由于`List`的元素是`Integer`，所以无法放入`String`数组，这个方法会抛出`ArrayStoreException`。

如果我们传入的数组大小和`List`实际的元素个数不一致怎么办？根据[List接口](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/List.html#toArray(T[]))的文档，我们可以知道：

如果传入的数组不够大，那么`List`内部会创建一个新的刚好够大的数组，填充后返回；如果传入的数组比`List`元素还要多，那么填充完元素后，剩下的数组元素一律填充`null`。

**==实际上，最常用的是传入一个“恰好”大小的数组：==**

```
Integer[] array = list.toArray(new Integer[list.size()]);
```

最后一种更简洁的写法是通过`List`接口定义的`T[] toArray(IntFunction<T[]> generator)`方法：

```
Integer[] array = list.toArray(Integer[]::new);
```

---

把`Array`变为`List`就简单多了，通过`List.of(T...)`方法最简单：

```
Integer[] array = { 1, 2, 3 };
List<Integer> list = List.of(array);
```

对于JDK 11之前的版本，可以使用`Arrays.asList(T...)`方法把数组转换成`List`。

要注意的是，返回的`List`不一定就是`ArrayList`或者`LinkedList`，因为`List`只是一个接口，如果我们调用`List.of()`，它返回的是一个只读`List`：

```
public class Main {
    public static void main(String[] args) {
        List<Integer> list = List.of(12, 34, 56);
        list.add(999); // UnsupportedOperationException
    }
}
```

对只读`List`调用`add()`、`remove()`方法会抛出`UnsupportedOperationException`。

###### 编写equals方法

我们知道**`List`是一种有序链表：`List`内部按照放入元素的先后顺序存放，并且每个元素都可以通过索引确定自己的位置。**

**==`List`还提供了`boolean contains(Object o)`方法来判断`List`是否包含某个指定元素。此外，`int indexOf(Object o)`方法可以返回某个元素的索引，如果元素不存在，就返回`-1`。==**

我们来看一个例子：

```java
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("A", "B", "C");
        System.out.println(list.contains("C")); // true
        System.out.println(list.contains("X")); // false
        System.out.println(list.indexOf("C")); // 2
        System.out.println(list.indexOf("X")); // -1
    }
}
```

这里我们注意一个问题，我们往`List`中添加的`"C"`和调用`contains("C")`传入的`"C"`是不是同一个实例？

如果这两个`"C"`不是同一个实例，这段代码是否还能得到正确的结果？我们可以改写一下代码测试一下：

```
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("A", "B", "C");
        System.out.println(list.contains(new String("C"))); // true or false?
        System.out.println(list.indexOf(new String("C"))); // 2 or -1?
    }
}
```

因为我们传入的是`new String("C")`，所以一定是不同的实例。结果仍然符合预期，这是为什么呢？

因为`List`内部并不是通过`==`判断两个元素是否相等，而是使用`equals()`方法判断两个元素是否相等，例如`contains()`方法可以实现如下

```
public class ArrayList {
    Object[] elementData;
    public boolean contains(Object o) {
        for (int i = 0; i < elementData.length; i++) {
            if (o.equals(elementData[i])) {
                return true;
            }
        }
        return false;
    }
}
```

**==因此，要正确使用`List`的`contains()`、`indexOf()`这些方法，放入的实例必须正确覆写`equals()`方法，否则，放进去的实例，查找不到。我们之所以能正常放入`String`、`Integer`这些对象，是因为Java标准库定义的这些类已经正确实现了`equals()`方法。==**

如何正确编写`equals()`方法？`equals()`方法要求我们必须满足以下条件：

- 自反性（Reflexive）：对于非`null`的`x`来说，`x.equals(x)`必须返回`true`；
- 对称性（Symmetric）：对于非`null`的`x`和`y`来说，如果`x.equals(y)`为`true`，则`y.equals(x)`也必须为`true`；
- 传递性（Transitive）：对于非`null`的`x`、`y`和`z`来说，如果`x.equals(y)`为`true`，`y.equals(z)`也为`true`，那么`x.equals(z)`也必须为`true`；
- 一致性（Consistent）：对于非`null`的`x`和`y`来说，只要`x`和`y`状态不变，则`x.equals(y)`总是一致地返回`true`或者`false`；
- 对`null`的比较：即`x.equals(null)`永远返回`false`。

上述规则看上去似乎非常复杂，但其实代码实现`equals()`方法是很简单的，我们以`Person`类为例：

```
public class Person {
    public String name;
    public int age;
}
```

首先，我们要定义“相等”的逻辑含义。对于`Person`类，如果`name`相等，并且`age`相等，我们就认为两个`Person`实例相等。

因此，编写`equals()`方法如下：

```
public boolean equals(Object o) {
    if (o instanceof Person p) {
        return this.name.equals(p.name) && this.age == p.age;
    }
    return false;
}
```

对于引用字段比较，我们使用`equals()`，对于基本类型字段的比较，我们使用`==`。

如果`this.name`为`null`，那么`equals()`方法会报错，因此，需要继续改写如下：

```
public boolean equals(Object o) {
    if (o instanceof Person p) {
        boolean nameEquals = false;
        if (this.name == null && p.name == null) {
            nameEquals = true;
        }
        if (this.name != null) {
            nameEquals = this.name.equals(p.name);
        }
        return nameEquals && this.age == p.age;
    }
    return false;
}
```

如果`Person`有好几个引用类型的字段，上面的写法就太复杂了。要简化引用类型的比较，我们使用`Objects.equals()`静态方法：

```
public boolean equals(Object o) {
    if (o instanceof Person p) {
        return Objects.equals(this.name, p.name) && this.age == p.age;
    }
    return false;
}
```

因此，我们总结一下`equals()`方法的正确编写方法：

1. 先确定实例“相等”的逻辑，即哪些字段相等，就认为实例相等；
2. 用`instanceof`判断传入的待比较的`Object`是不是当前类型，如果是，继续比较，否则，返回`false`；
3. 对引用类型用`Objects.equals()`比较，对基本类型直接用`==`比较。

使用`Objects.equals()`比较两个引用类型是否相等的目的是省去了判断`null`的麻烦。两个引用类型都是`null`时它们也是相等的。

如果不调用`List`的`contains()`、`indexOf()`这些方法，那么放入的元素就不需要实现`equals()`方法。

#### Map

我们知道，`List`是一种顺序列表，如果有一个存储学生`Student`实例的`List`，要在`List`中根据`name`查找某个指定的`Student`的分数，应该怎么办？

最简单的方法是遍历`List`并判断`name`是否相等，然后返回指定元素：

```
List<Student> list = ...
Student target = null;
for (Student s : list) {
    if ("Xiao Ming".equals(s.name)) {
        target = s;
        break;
    }
}
System.out.println(target.score);
```

这种需求其实非常常见，即**==通过一个键去查询对应的值。使用`List`来实现存在效率非常低的问题，因为平均需要扫描一半的元素才能确定，而`Map`这种键值（key-value）映射表的数据结构，作用就是能高效通过`key`快速查找`value`（元素）。==**

用`Map`来实现根据`name`查询某个`Student`的代码如下：

```java
public class Main {
    public static void main(String[] args) {
        Student s = new Student("Xiao Ming", 99);
        Map<String, Student> map = new HashMap<>();
        map.put("Xiao Ming", s); // 将"Xiao Ming"和Student实例映射并关联
        Student target = map.get("Xiao Ming"); // 通过key查找并返回映射的Student实例
        System.out.println(target == s); // true，同一个实例
        System.out.println(target.score); // 99
        Student another = map.get("Bob"); // 通过另一个key查找
        System.out.println(another); // 未找到返回null
    }
}

class Student {
    public String name;
    public int score;
    public Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
}
```

通过上述代码可知：**`Map<K, V>`是一种键-值映射表，当我们调用`put(K key, V value)`方法时，就把`key`和`value`做了映射并放入`Map`。当我们调用`V get(K key)`时，就可以通过`key`获取到对应的`value`。如果`key`不存在，则返回`null`。和`List`类似，`Map`也是一个接口，最常用的实现类是`HashMap`。**

**如果只是想查询某个`key`是否存在，可以调用`boolean containsKey(K key)`方法。**

如果我们在存储`Map`映射关系的时候，对同一个key调用两次`put()`方法，分别放入不同的`value`，会有什么问题呢？例如：

```
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        System.out.println(map.get("apple")); // 123
        map.put("apple", 789); // 再次放入apple作为key，但value变为789
        System.out.println(map.get("apple")); // 789
    }
}
```

重复放入`key-value`并不会有任何问题，但是**一个`key`只能关联一个`value`**。在上面的代码中，一开始我们把`key`对象`"apple"`映射到`Integer`对象`123`，然后再次调用`put()`方法把`"apple"`映射到`789`，这时，原来关联的`value`对象`123`就被“冲掉”了。实际上，`put()`方法的签名是`V put(K key, V value)`，**如果放入的`key`已经存在，`put()`方法会返回被删除的旧的`value`**，否则，返回`null`。

 **==始终牢记：Map中不存在重复的key，因为放入相同的key，只会把原有的key-value对应的value给替换掉。==**

此外，在一个`Map`中，虽然`key`不能重复，但`value`是可以重复的：

```
Map<String, Integer> map = new HashMap<>();
map.put("apple", 123);
map.put("pear", 123); // ok
```

###### 遍历Map

**对`Map`来说，要遍历`key`可以使用`for each`循环遍历`Map`实例的`keySet()`方法返回的`Set`集合，它包含不重复的`key`的集合：**

```java
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        map.put("banana", 789);
        for (String key : map.keySet()) {
            Integer value = map.get(key);
            System.out.println(key + " = " + value);
        }
    }
}
```

**同时遍历`key`和`value`可以使用`for each`循环遍历`Map`对象的`entrySet()`集合，它包含每一个`key-value`映射：**

```java
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        map.put("banana", 789);
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            String key = entry.getKey();
            Integer value = entry.getValue();
            System.out.println(key + " = " + value);
        }
    }
}
```

`Map`和`List`不同的是，`Map`存储的是`key-value`的映射关系，并且，它*不保证顺序*。在遍历的时候，遍历的顺序既不一定是`put()`时放入的`key`的顺序，也不一定是`key`的排序顺序。使用`Map`时，任何依赖顺序的逻辑都是不可靠的。以`HashMap`为例，假设我们放入`"A"`，`"B"`，`"C"`这3个`key`，遍历的时候，每个`key`会保证被遍历一次且仅遍历一次，但顺序完全没有保证，甚至对于不同的JDK版本，相同的代码遍历的输出顺序都是不同的！

 **==遍历Map时，不可假设输出的key是有序的！==**

###### 编写equals和hashCode

我们知道Map是一种键-值（key-value）映射表，可以通过key快速查找对应的value。

以HashMap为例，观察下面的代码：

```
Map<String, Person> map = new HashMap<>();
map.put("a", new Person("Xiao Ming"));
map.put("b", new Person("Xiao Hong"));
map.put("c", new Person("Xiao Jun"));

map.get("a"); // Person("Xiao Ming")
map.get("x"); // null
```

`HashMap`之所以能根据`key`直接拿到`value`，原因是它内部通过空间换时间的方法，用一个大数组存储所有`value`，并根据key直接计算出`value`应该存储在哪个索引：

```ascii
  ┌───┐
0 │   │
  ├───┤
1 │ ●─┼───> Person("Xiao Ming")
  ├───┤
2 │   │
  ├───┤
3 │   │
  ├───┤
4 │   │
  ├───┤
5 │ ●─┼───> Person("Xiao Hong")
  ├───┤
6 │ ●─┼───> Person("Xiao Jun")
  ├───┤
7 │   │
  └───┘
```

如果`key`的值为`"a"`，计算得到的索引总是`1`，因此返回`value`为`Person("Xiao Ming")`，如果`key`的值为`"b"`，计算得到的索引总是`5`，因此返回`value`为`Person("Xiao Hong")`，这样，就不必遍历整个数组，即可直接读取`key`对应的`value`。

当我们使用`key`存取`value`的时候，就会引出一个问题：

我们放入`Map`的`key`是字符串`"a"`，但是，当我们获取`Map`的`value`时，传入的变量不一定就是放入的那个`key`对象。

换句话讲，两个`key`应该是内容相同，但不一定是同一个对象。测试代码如下：

```
public class Main {
    public static void main(String[] args) {
        String key1 = "a";
        Map<String, Integer> map = new HashMap<>();
        map.put(key1, 123);

        String key2 = new String("a");
        map.get(key2); // 123

        System.out.println(key1 == key2); // false
        System.out.println(key1.equals(key2)); // true
    }
}
```

因为在`Map`的内部，对`key`做比较是通过`equals()`实现的，这一点和`List`查找元素需要正确覆写`equals()`是一样的，即正确使用`Map`必须保证：作为`key`的对象必须正确覆写`equals()`方法。

我们经常使用`String`作为`key`，因为`String`已经正确覆写了`equals()`方法。但如果我们放入的`key`是一个自己写的类，就必须保证正确覆写了`equals()`方法。

我们再思考一下`HashMap`为什么能通过`key`直接计算出`value`存储的索引。相同的`key`对象（使用`equals()`判断时返回`true`）必须要计算出相同的索引，否则，相同的`key`每次取出的`value`就不一定对。

通**过`key`计算索引的方式就是调用`key`对象的`hashCode()`方法，它返回一个`int`整数。`HashMap`正是通过这个方法直接定位`key`对应的`value`的索引，继而直接返回`value`。**

因此，正确使用`Map`必须保证：

1. **作为`key`的对象必须正确覆写`equals()`方法，相等的两个`key`实例调用`equals()`必须返回`true`；**
2. **作为`key`的对象还必须正确覆写`hashCode()`方法，且`hashCode()`方法要严格遵循以下规范：**

- **如果两个对象相等，则两个对象的`hashCode()`必须相等；**
- **如果两个对象不相等，则两个对象的`hashCode()`尽量不要相等。**

即对应两个实例`a`和`b`：

- 如果`a`和`b`相等，那么`a.equals(b)`一定为`true`，则`a.hashCode()`必须等于`b.hashCode()`；
- 如果`a`和`b`不相等，那么`a.equals(b)`一定为`false`，则`a.hashCode()`和`b.hashCode()`尽量不要相等。

上述第一条规范是正确性，必须保证实现，否则`HashMap`不能正常工作。

而第二条如果尽量满足，则可以保证查询效率，因为不同的对象，如果返回相同的`hashCode()`，会造成`Map`内部存储冲突，使存取的效率下降。

正确编写`equals()`的方法我们已经在[编写equals方法](https://www.liaoxuefeng.com/wiki/1252599548343744/1265116446975264)一节中讲过了，以`Person`类为例：

```
public class Person {
    String firstName;
    String lastName;
    int age;
}
```

把需要比较的字段找出来：

- firstName
- lastName
- age

然后，引用类型使用`Objects.equals()`比较，基本类型使用`==`比较。

在正确实现`equals()`的基础上，我们还需要正确实现`hashCode()`，即上述3个字段分别相同的实例，`hashCode()`返回的`int`必须相同：

```
public class Person {
    String firstName;
    String lastName;
    int age;

    @Override
    int hashCode() {
        int h = 0;
        h = 31 * h + firstName.hashCode();
        h = 31 * h + lastName.hashCode();
        h = 31 * h + age;
        return h;
    }
}
```

注意到`String`类已经正确实现了`hashCode()`方法，我们在计算`Person`的`hashCode()`时，反复使用`31*h`，这样做的目的是为了尽量把不同的`Person`实例的`hashCode()`均匀分布到整个`int`范围。

和实现`equals()`方法遇到的问题类似，如果`firstName`或`lastName`为`null`，上述代码工作起来就会抛`NullPointerException`。为了解决这个问题，我们在计算`hashCode()`的时候，经常借助`Objects.hash()`来计算：

```
int hashCode() {
    return Objects.hash(firstName, lastName, age);
}
```

所以，编写`equals()`和`hashCode()`遵循的原则是：

`equals()`用到的用于比较的每一个字段，都必须在`hashCode()`中用于计算；`equals()`中没有使用到的字段，绝不可放在`hashCode()`中计算。

另外注意，对于放入`HashMap`的`value`对象，没有任何要求。

既然`HashMap`内部使用了数组，通过计算`key`的`hashCode()`直接定位`value`所在的索引，那么第一个问题来了：hashCode()返回的`int`范围高达±21亿，先不考虑负数，`HashMap`内部使用的数组得有多大？

实际上`HashMap`初始化时默认的数组大小只有16，任何`key`，无论它的`hashCode()`有多大，都可以简单地通过：

```
int index = key.hashCode() & 0xf; // 0xf = 15
```

把索引确定在0～15，即永远不会超出数组范围，上述算法只是一种最简单的实现。

第二个问题：如果添加超过16个`key-value`到`HashMap`，数组不够用了怎么办？

添加超过一定数量的`key-value`时，`HashMap`会在内部自动扩容，每次扩容一倍，即长度为16的数组扩展为长度32，相应地，需要重新确定`hashCode()`计算的索引位置。例如，对长度为32的数组计算`hashCode()`对应的索引，计算方式要改为：

```
int index = key.hashCode() & 0x1f; // 0x1f = 31
```

由于扩容会导致重新分布已有的`key-value`，所以，频繁扩容对`HashMap`的性能影响很大。如果我们确定要使用一个容量为`10000`个`key-value`的`HashMap`，更好的方式是创建`HashMap`时就指定容量：

```
Map<String, Integer> map = new HashMap<>(10000);
```

虽然指定容量是`10000`，但`HashMap`内部的数组长度总是2n，因此，实际数组长度被初始化为比`10000`大的`16384`（214）。

最后一个问题：如果不同的两个`key`，例如`"a"`和`"b"`，它们的`hashCode()`恰好是相同的（这种情况是完全可能的，因为不相等的两个实例，只要求`hashCode()`尽量不相等），那么，当我们放入：

```
map.put("a", new Person("Xiao Ming"));
map.put("b", new Person("Xiao Hong"));
```

时，由于计算出的数组索引相同，后面放入的`"Xiao Hong"`会不会把`"Xiao Ming"`覆盖了？

当然不会！使用`Map`的时候，只要`key`不相同，它们映射的`value`就互不干扰。但是，在`HashMap`内部，确实可能存在不同的`key`，映射到相同的`hashCode()`，即相同的数组索引上，肿么办？

我们就假设`"a"`和`"b"`这两个`key`最终计算出的索引都是5，那么，在`HashMap`的数组中，实际存储的不是一个`Person`实例，而是一个`List`，它包含两个`Entry`，一个是`"a"`的映射，一个是`"b"`的映射：

```ascii
  ┌───┐
0 │   │
  ├───┤
1 │   │
  ├───┤
2 │   │
  ├───┤
3 │   │
  ├───┤
4 │   │
  ├───┤
5 │ ●─┼───> List<Entry<String, Person>>
  ├───┤
6 │   │
  ├───┤
7 │   │
  └───┘
```

在查找的时候，例如：

```
Person p = map.get("a");
```

HashMap内部通过`"a"`找到的实际上是`List<Entry<String, Person>>`，它还需要遍历这个`List`，并找到一个`Entry`，它的`key`字段是`"a"`，才能返回对应的`Person`实例。

我们把不同的`key`具有相同的`hashCode()`的情况称之为哈希冲突。在冲突的时候，一种最简单的解决办法是用`List`存储`hashCode()`相同的`key-value`。显然，如果冲突的概率越大，这个`List`就越长，`Map`的`get()`方法效率就越低，这就是为什么要尽量满足条件二：

 如果两个对象不相等，则两个对象的hashCode()尽量不要相等。

`hashCode()`方法编写得越好，`HashMap`工作的效率就越高。

#### EnumMap

因为`HashMap`是一种通过对key计算`hashCode()`，通过空间换时间的方式，直接定位到value所在的内部数组的索引，因此，查找效率非常高。

如果作为key的对象是`enum`类型，那么，还可以使用Java集合库提供的一种`EnumMap`，它在内部以一个非常紧凑的数组存储value，并且根据`enum`类型的key直接定位到内部数组的索引，并不需要计算`hashCode()`，不但效率最高，而且没有额外的空间浪费。

我们以`DayOfWeek`这个枚举类型为例，为它做一个“翻译”功能：

```java
public class Main {
    public static void main(String[] args) {
        Map<DayOfWeek, String> map = new EnumMap<>(DayOfWeek.class);
        map.put(DayOfWeek.MONDAY, "星期一");
        map.put(DayOfWeek.TUESDAY, "星期二");
        map.put(DayOfWeek.WEDNESDAY, "星期三");
        map.put(DayOfWeek.THURSDAY, "星期四");
        map.put(DayOfWeek.FRIDAY, "星期五");
        map.put(DayOfWeek.SATURDAY, "星期六");
        map.put(DayOfWeek.SUNDAY, "星期日");
        System.out.println(map);
        System.out.println(map.get(DayOfWeek.MONDAY));
    }
}
```

#### TreeMap

我们已经知道，`HashMap`是一种以空间换时间的映射表，它的实现原理决定了内部的Key是无序的，即遍历`HashMap`的Key时，其顺序是不可预测的（但每个Key都会遍历一次且仅遍历一次）。

**还有一种`Map`，它在内部会对Key进行排序，这种`Map`就是`SortedMap`。注意到`SortedMap`是接口，它的实现类是`TreeMap`。**

```ascii
       ┌───┐
       │Map│
       └───┘
         ▲
    ┌────┴─────┐
    │          │
┌───────┐ ┌─────────┐
│HashMap│ │SortedMap│
└───────┘ └─────────┘
               ▲
               │
          ┌─────────┐
          │ TreeMap │
          └─────────┘
```

**`SortedMap`保证遍历时以Key的顺序来进行排序。例如，放入的Key是`"apple"`、`"pear"`、`"orange"`，遍历的顺序一定是`"apple"`、`"orange"`、`"pear"`，因为`String`默认按字母排序：**

```java
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new TreeMap<>();
        map.put("orange", 1);
        map.put("apple", 2);
        map.put("pear", 3);
        for (String key : map.keySet()) {
            System.out.println(key);
        }
        // apple, orange, pear
    }
}
```

**==使用`TreeMap`时，放入的Key必须实现`Comparable`接口。`String`、`Integer`这些类已经实现了`Comparable`接口，因此可以直接作为Key使用。==**作为Value的对象则没有任何要求。

如果作为Key的class没有实现`Comparable`接口，那么，必须在创建`TreeMap`时同时指定一个自定义排序算法：

```java
public class Main {
    public static void main(String[] args) {
        Map<Person, Integer> map = new TreeMap<>(new Comparator<Person>() {
            public int compare(Person p1, Person p2) {
                return p1.name.compareTo(p2.name);
            }
        });
        map.put(new Person("Tom"), 1);
        map.put(new Person("Bob"), 2);
        map.put(new Person("Lily"), 3);
        for (Person key : map.keySet()) {
            System.out.println(key);
        }
        // {Person: Bob}, {Person: Lily}, {Person: Tom}
        System.out.println(map.get(new Person("Bob"))); // 2
    }
}

class Person {
    public String name;
    Person(String name) {
        this.name = name;
    }
    public String toString() {
        return "{Person: " + name + "}";
    }
}
```

注意到`Comparator`接口要求实现一个比较方法，它负责比较传入的两个元素`a`和`b`，如果`a<b`，则返回负数，通常是`-1`，如果`a==b`，则返回`0`，如果`a>b`，则返回正数，通常是`1`。`TreeMap`内部根据比较结果对Key进行排序。

从上述代码执行结果可知，打印的Key确实是按照`Comparator`定义的顺序排序的。如果要根据Key查找Value，我们可以传入一个`new Person("Bob")`作为Key，它会返回对应的`Integer`值`2`。

另外，注意到`Person`类并未覆写`equals()`和`hashCode()`，因为`TreeMap`不使用`equals()`和`hashCode()`。

我们来看一个稍微复杂的例子：这次我们定义了`Student`类，并用分数`score`进行排序，高分在前：

```java
public class Main {
    public static void main(String[] args) {
        Map<Student, Integer> map = new TreeMap<>(new Comparator<Student>() {
            public int compare(Student p1, Student p2) {
                return p1.score > p2.score ? -1 : 1;
            }
        });
        map.put(new Student("Tom", 77), 1);
        map.put(new Student("Bob", 66), 2);
        map.put(new Student("Lily", 99), 3);
        for (Student key : map.keySet()) {
            System.out.println(key);
        }
        System.out.println(map.get(new Student("Bob", 66))); // null?
    }
}

class Student {
    public String name;
    public int score;
    Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
    public String toString() {
        return String.format("{%s: score=%d}", name, score);
    }
}
```

在`for`循环中，我们确实得到了正确的顺序。但是，且慢！根据相同的Key：`new Student("Bob", 66)`进行查找时，结果为`null`！

这是怎么肥四？难道`TreeMap`有问题？遇到`TreeMap`工作不正常时，我们首先回顾Java编程基本规则：出现问题，不要怀疑Java标准库，要从自身代码找原因。

在这个例子中，`TreeMap`出现问题，原因其实出在这个`Comparator`上：

```
public int compare(Student p1, Student p2) {
    return p1.score > p2.score ? -1 : 1;
}
```

在`p1.score`和`p2.score`不相等的时候，它的返回值是正确的，但是，在`p1.score`和`p2.score`相等的时候，它并没有返回`0`！这就是为什么`TreeMap`工作不正常的原因：`TreeMap`在比较两个Key是否相等时，依赖Key的`compareTo()`方法或者`Comparator.compare()`方法。在两个Key相等时，必须返回`0`。因此，修改代码如下：

```
public int compare(Student p1, Student p2) {
    if (p1.score == p2.score) {
        return 0;
    }
    return p1.score > p2.score ? -1 : 1;
}
```

或者直接借助`Integer.compare(int, int)`也可以返回正确的比较结果。

#### Set

我们知道，`Map`用于存储key-value的映射，对于充当key的对象，是不能重复的，并且，不但需要正确覆写`equals()`方法，还要正确覆写`hashCode()`方法。

如果我们只需要**存储不重复的key，并不需要存储映射的value，那么就可以使用`Set`。**

**`Set`用于存储不重复的元素集合，它主要提供以下几个方法：**

- **将元素添加进`Set<E>`：`boolean add(E e)`**
- **将元素从`Set<E>`删除：`boolean remove(Object e)`**
- **判断是否包含元素：`boolean contains(Object e)`**

我们来看几个简单的例子：

```java
public class Main {
    public static void main(String[] args) {
        Set<String> set = new HashSet<>();
        System.out.println(set.add("abc")); // true
        System.out.println(set.add("xyz")); // true
        System.out.println(set.add("xyz")); // false，添加失败，因为元素已存在
        System.out.println(set.contains("xyz")); // true，元素存在
        System.out.println(set.contains("XYZ")); // false，元素不存在
        System.out.println(set.remove("hello")); // false，删除失败，因为元素不存在
        System.out.println(set.size()); // 2，一共两个元素
    }
}
```

**`Set`实际上相当于只存储key、不存储value的`Map`。我们经常用`Set`用于去除重复元素。**

因为**放入`Set`的元素和`Map`的key类似，都要正确实现`equals()`和`hashCode()`方法**，否则该元素无法正确地放入`Set`。

**最常用的`Set`实现类是`HashSet`**，实际上，`HashSet`仅仅是对`HashMap`的一个简单封装，它的核心代码如下：

```
public class HashSet<E> implements Set<E> {
    // 持有一个HashMap:
    private HashMap<E, Object> map = new HashMap<>();

    // 放入HashMap的value:
    private static final Object PRESENT = new Object();

    public boolean add(E e) {
        return map.put(e, PRESENT) == null;
    }

    public boolean contains(Object o) {
        return map.containsKey(o);
    }

    public boolean remove(Object o) {
        return map.remove(o) == PRESENT;
    }
}
```

**`Set`接口并不保证有序，而`SortedSet`接口则保证元素是有序的：**

- **`HashSet`是无序的，因为它实现了`Set`接口，并没有实现`SortedSet`接口；**
- **`TreeSet`是有序的，因为它实现了`SortedSet`接口。**

用一张图表示：

```ascii
       ┌───┐
       │Set│
       └───┘
         ▲
    ┌────┴─────┐
    │          │
┌───────┐ ┌─────────┐
│HashSet│ │SortedSet│
└───────┘ └─────────┘
               ▲
               │
          ┌─────────┐
          │ TreeSet │
          └─────────┘
```

我们来看`HashSet`的输出：

```
public class Main {
    public static void main(String[] args) {
        Set<String> set = new HashSet<>();
        set.add("apple");
        set.add("banana");
        set.add("pear");
        set.add("orange");
        for (String s : set) {
            System.out.println(s);
        }
    }
}
```

注意输出的顺序既不是添加的顺序，也不是`String`排序的顺序，在不同版本的JDK中，这个顺序也可能是不同的。

```java
public class Main {
    public static void main(String[] args) {
        Set<String> set = new TreeSet<>();
        set.add("apple");
        set.add("banana");
        set.add("pear");
        set.add("orange");
        for (String s : set) {
            System.out.println(s);
        }
    }
}
```

==**使用`TreeSet`和使用`TreeMap`的要求一样，添加的元素必须正确实现`Comparable`接口，如果没有实现`Comparable`接口，那么创建`TreeSet`时必须传入一个`Comparator`对象。**==

#### Queue

队列（`Queue`）是一种经常使用的集合。`Queue`实际上是实现了一个先进先出（FIFO：First In First Out）的有序表。它和`List`的区别在于，`List`可以在任意位置添加和删除元素，而`Queue`只有两个操作：

- 把元素添加到队列末尾；
- 从队列头部取出元素。

超市的收银台就是一个队列：

![queue](https://www.liaoxuefeng.com/files/attachments/1285667604660289/l)

在Java的标准库中，队列接口`Queue`定义了以下几个方法：

- `int size()`：获取队列长度；
- `boolean add(E)`/`boolean offer(E)`：添加元素到队尾；
- `E remove()`/`E poll()`：获取队首元素并从队列中删除；
- `E element()`/`E peek()`：获取队首元素但并不从队列中删除。

对于具体的实现类，有的Queue有最大队列长度限制，有的Queue没有。注意到添加、删除和获取队列元素总是有两个方法，这是因为在添加或获取元素失败时，这两个方法的行为是不同的。我们用一个表格总结如下：

|                    | throw Exception | 返回false或null    |
| :----------------- | :-------------- | ------------------ |
| 添加元素到队尾     | add(E e)        | boolean offer(E e) |
| 取队首元素并删除   | E remove()      | E poll()           |
| 取队首元素但不删除 | E element()     | E peek()           |

![image-20240129115909846](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20240129115909846.png)举个栗子，假设我们有一个队列，对它做一个添加操作，如果调用`add()`方法，当添加失败时（可能超过了队列的容量)，它会抛出异常：

```
Queue<String> q = ...
try {
    q.add("Apple");
    System.out.println("添加成功");
} catch(IllegalStateException e) {
    System.out.println("添加失败");
}
```

如果我们调用`offer()`方法来添加元素，当添加失败时，它不会抛异常，而是返回`false`：

```
Queue<String> q = ...
if (q.offer("Apple")) {
    System.out.println("添加成功");
} else {
    System.out.println("添加失败");
}
```

当我们需要从`Queue`中取出队首元素时，如果当前`Queue`是一个空队列，调用`remove()`方法，它会抛出异常：

```
Queue<String> q = ...
try {
    String s = q.remove();
    System.out.println("获取成功");
} catch(IllegalStateException e) {
    System.out.println("获取失败");
}
```

如果我们调用`poll()`方法来取出队首元素，当获取失败时，它不会抛异常，而是返回`null`：

```
Queue<String> q = ...
String s = q.poll();
if (s != null) {
    System.out.println("获取成功");
} else {
    System.out.println("获取失败");
}
```

因此，两套方法可以根据需要来选择使用。

注意：不要把`null`添加到队列中，否则`poll()`方法返回`null`时，很难确定是取到了`null`元素还是队列为空。

**接下来我们以`poll()`和`peek()`为例来说说“获取并删除”与“获取但不删除”的区别。对于`Queue`来说，每次调用`poll()`，都会获取队首元素，并且获取到的元素已经从队列中被删除了：**

```java
public class Main {
    public static void main(String[] args) {
        Queue<String> q = new LinkedList<>();
        // 添加3个元素到队列:
        q.offer("apple");
        q.offer("pear");
        q.offer("banana");
        // 从队列取出元素:
        System.out.println(q.poll()); // apple
        System.out.println(q.poll()); // pear
        System.out.println(q.poll()); // banana
        System.out.println(q.poll()); // null,因为队列是空的
    }
}
```

如果用`peek()`，因为获取队首元素时，并不会从队列中删除这个元素，所以可以反复获取：

```java
public class Main {
    public static void main(String[] args) {
        Queue<String> q = new LinkedList<>();
        // 添加3个元素到队列:
        q.offer("apple");
        q.offer("pear");
        q.offer("banana");
        // 队首永远都是apple，因为peek()不会删除它:
        System.out.println(q.peek()); // apple
        System.out.println(q.peek()); // apple
        System.out.println(q.peek()); // apple
    }
}
```

**==从上面的代码中，我们还可以发现，`LinkedList`即实现了`List`接口，又实现了`Queue`接口，但是，在使用的时候，如果我们把它当作List，就获取List的引用，如果我们把它当作Queue，就获取Queue的引用：==**

```java
// 这是一个List:
List<String> list = new LinkedList<>();
// 这是一个Queue:
Queue<String> queue = new LinkedList<>();
```

#### PriorityQueue

我们知道，`Queue`是一个先进先出（FIFO）的队列。

在银行柜台办业务时，我们假设只有一个柜台在办理业务，但是办理业务的人很多，怎么办？

可以每个人先取一个号，例如：`A1`、`A2`、`A3`……然后，按照号码顺序依次办理，实际上这就是一个`Queue`。

如果这时来了一个**VIP客户，他的号码是`V1`，虽然当前排队的是`A10`、`A11`、`A12`……但是柜台下一个呼叫的客户号码却是`V1`。**

这个时候，我们发现，**要实现“VIP插队”的业务，用`Queue`就不行了，因为`Queue`会严格按FIFO的原则取出队首元素。我们需要的是优先队列：`PriorityQueue`。**

==**`PriorityQueue`和`Queue`的区别在于，它的出队顺序与元素的优先级有关，对`PriorityQueue`调用`remove()`或`poll()`方法，返回的总是优先级最高的元素。**==

==**要使用`PriorityQueue`，我们就必须给每个元素定义“优先级”。**==我们以实际代码为例，先看看`PriorityQueue`的行为：

```java
public class Main {
    public static void main(String[] args) {
        Queue<String> q = new PriorityQueue<>();
        // 添加3个元素到队列:
        q.offer("apple");
        q.offer("pear");
        q.offer("banana");
        System.out.println(q.poll()); // apple
        System.out.println(q.poll()); // banana
        System.out.println(q.poll()); // pear
        System.out.println(q.poll()); // null,因为队列为空
    }
}
```

**我们放入的顺序是`"apple"`、`"pear"`、`"banana"`，但是取出的顺序却是`"apple"`、`"banana"`、`"pear"`，这是因为从字符串的排序看，`"apple"`排在最前面，`"pear"`排在最后面。**

**==因此，放入`PriorityQueue`的元素，必须实现`Comparable`接口==**，`PriorityQueue`会根据元素的排序顺序决定出队的优先级。

**==如果我们要放入的元素并没有实现`Comparable`接口怎么办？`PriorityQueue`允许我们提供一个`Comparator`对象来判断两个元素的顺序。我们以银行排队业务为例，实现一个`PriorityQueue`：==**

```java
public class Main {
    public static void main(String[] args) {
        Queue<User> q = new PriorityQueue<>(new UserComparator());
        // 添加3个元素到队列:
        q.offer(new User("Bob", "A1"));
        q.offer(new User("Alice", "A2"));
        q.offer(new User("Boss", "V1"));
        System.out.println(q.poll()); // Boss/V1
        System.out.println(q.poll()); // Bob/A1
        System.out.println(q.poll()); // Alice/A2
        System.out.println(q.poll()); // null,因为队列为空
    }
}

class UserComparator implements Comparator<User> {
    public int compare(User u1, User u2) {
        if (u1.number.charAt(0) == u2.number.charAt(0)) {
            // 如果两人的号都是A开头或者都是V开头,比较号的大小:
            return u1.number.compareTo(u2.number);
        }
        if (u1.number.charAt(0) == 'V') {
            // u1的号码是V开头,优先级高:
            return -1;
        } else {
            return 1;
        }
    }
}

class User {
    public final String name;
    public final String number;

    public User(String name, String number) {
        this.name = name;
        this.number = number;
    }

    public String toString() {
        return name + "/" + number;
    }
}
```

[compareTo比较大小-CSDN博客](https://blog.csdn.net/m0_58138734/article/details/121741893?ops_request_misc=%7B%22request%5Fid%22%3A%22170142110216800185832515%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=170142110216800185832515&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-121741893-null-null.142^v96^pc_search_result_base8&utm_term=compareto比较大小&spm=1018.2226.3001.4187)

![image-20240215202901067](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20240215202901067.png)

#### Deque

我们知道，`Queue`是队列，只能一头进，另一头出。

如果把条件放松一下，允许两头都进，两头都出，这种队列叫双端队列（Double Ended Queue），学名`Deque`。

Java集合提供了接口`Deque`来实现一个双端队列，它的功能是：

- 既可以添加到队尾，也可以添加到队首；
- 既可以从队首获取，又可以从队尾获取。

我们来比较一下`Queue`和`Deque`出队和入队的方法：

| 功能               | Queue                  | Deque                           |
| :----------------- | :--------------------- | :------------------------------ |
| 添加元素到队尾     | add(E e) / offer(E e)  | addLast(E e) / offerLast(E e)   |
| 取队首元素并删除   | E remove() / E poll()  | E removeFirst() / E pollFirst() |
| 取队首元素但不删除 | E element() / E peek() | E getFirst() / E peekFirst()    |
| 添加元素到队首     | 无                     | addFirst(E e) / offerFirst(E e) |
| 取队尾元素并删除   | 无                     | E removeLast() / E pollLast()   |
| 取队尾元素但不删除 | 无                     | E getLast() / E peekLast()      |

![image-20240129120025055](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20240129120025055.png)

![image-20240129120117779](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20240129120117779.png)

对于添加元素到队尾的操作，`Queue`提供了`add()`/`offer()`方法，而`Deque`提供了`addLast()`/`offerLast()`方法。添加元素到队首、取队尾元素的操作在`Queue`中不存在，在`Deque`中由`addFirst()`/`removeLast()`等方法提供。

注意到`Deque`接口实际上扩展自`Queue`：

```
public interface Deque<E> extends Queue<E> {
    ...
}
```

因此，`Queue`提供的`add()`/`offer()`方法在`Deque`中也可以使用，但是，使用`Deque`，最好不要调用`offer()`，而是调用`offerLast()`：

```java
public class Main {
    public static void main(String[] args) {
        Deque<String> deque = new LinkedList<>();
        deque.offerLast("A"); // A
        deque.offerLast("B"); // A <- B
        deque.offerFirst("C"); // C <- A <- B
        System.out.println(deque.pollFirst()); // C, 剩下A <- B
        System.out.println(deque.pollLast()); // B, 剩下A
        System.out.println(deque.pollFirst()); // A
        System.out.println(deque.pollFirst()); // null
    }
}
```

如果直接写`deque.offer()`，我们就需要思考，`offer()`实际上是`offerLast()`，我们明确地写上`offerLast()`，不需要思考就能一眼看出这是添加到队尾。

因此，使用`Deque`，推荐总是明确调用`offerLast()`/`offerFirst()`或者`pollFirst()`/`pollLast()`方法。

`Deque`是一个接口，它的实现类有`ArrayDeque`和`LinkedList`。

我们发现`LinkedList`真是一个全能选手，它即是`List`，又是`Queue`，还是`Deque`。但是我们在使用的时候，总是用特定的接口来引用它，这是因为持有接口说明代码的抽象层次更高，而且接口本身定义的方法代表了特定的用途。

```
// 不推荐的写法:
LinkedList<String> d1 = new LinkedList<>();
d1.offerLast("z");
// 推荐的写法：
Deque<String> d2 = new LinkedList<>();
d2.offerLast("z");
```

==**可见面向抽象编程的一个原则就是：尽量持有接口，而不是具体的实现类。**==

#### Stack

栈（Stack）是一种后进先出（LIFO：Last In First Out）的数据结构。

什么是LIFO呢？我们先回顾一下`Queue`的特点FIFO：

```ascii
          ────────────────────────
  (\(\      (\(\    (\(\    (\(\      (\(\
 (='.') ─> (='.')  (='.')  (='.') ─> (='.')
O(_")")   O(_")") O(_")") O(_")")   O(_")")
          ────────────────────────
```

所谓FIFO，是最先进队列的元素一定最早出队列，而LIFO是最后进`Stack`的元素一定最早出`Stack`。如何做到这一点呢？只需要把队列的一端封死：

```ascii
           ───────────────────────────────┐
  (\(\       (\(\    (\(\    (\(\    (\(\ │
 (='.') <─> (='.')  (='.')  (='.')  (='.')│
O(_")")    O(_")") O(_")") O(_")") O(_")")│
           ───────────────────────────────┘
```

因此，`Stack`是这样一种数据结构：只能不断地往`Stack`中压入（push）元素，最后进去的必须最早弹出（pop）来：

![donuts-stack](https://www.liaoxuefeng.com/files/attachments/1285759053070401/l)

`Stack`只有入栈和出栈的操作：

- 把元素压栈：`push(E)`；
- 把栈顶的元素“弹出”：`pop()`；
- 取栈顶元素但不弹出：`peek()`。

在Java中，我们用`Deque`可以实现`Stack`的功能：

- 把元素压栈：`push(E)`/`addFirst(E)`；
- 把栈顶的元素“弹出”：`pop()`/`removeFirst()`；
- 取栈顶元素但不弹出：`peek()`/`peekFirst()`。

为什么Java的集合类没有单独的`Stack`接口呢？因为有个遗留类名字就叫`Stack`，出于兼容性考虑，所以没办法创建`Stack`接口，只能用`Deque`接口来“模拟”一个`Stack`了。

**当我们把`Deque`作为`Stack`使用时，注意只调用`push()`/`pop()`/`peek()`方法，不要调用`addFirst()`/`removeFirst()`/`peekFirst()`方法，这样代码更加清晰。**

###### Stack的作用

Stack在计算机中使用非常广泛，JVM在处理Java方法调用的时候就会通过栈这种数据结构维护方法调用的层次。例如：

```
static void main(String[] args) {
    foo(123);
}

static String foo(x) {
    return "F-" + bar(x + 1);
}

static int bar(int x) {
    return x << 2;
}
```

JVM会创建方法调用栈，每调用一个方法时，先将参数压栈，然后执行对应的方法；当方法返回时，返回值压栈，调用方法通过出栈操作获得方法返回值。

因为方法调用栈有容量限制，嵌套调用过多会造成栈溢出，即引发`StackOverflowError`：

```
public class Main {
    public static void main(String[] args) {
        increase(1);
    }

    static int increase(int x) {
        return increase(x) + 1;
    }
}
```

我们再来看一个`Stack`的用途：对整数进行进制的转换就可以利用栈。

例如，我们要把一个`int`整数`12500`转换为十六进制表示的字符串，如何实现这个功能？

首先我们准备一个空栈：

```ascii
│   │
│   │
│   │
│   │
│   │
│   │
│   │
│   │
└───┘
```

然后计算12500÷16=781…4，余数是`4`，把余数`4`压栈：

```ascii
│   │
│   │
│   │
│   │
│   │
│   │
│   │
│ 4 │
└───┘
```

然后计算781÷16=48…13，余数是`13`，`13`的十六进制用字母`D`表示，把余数`D`压栈：

```ascii
│   │
│   │
│   │
│   │
│   │
│ D │
│   │
│ 4 │
└───┘
```

然后计算48÷16=3…0，余数是`0`，把余数`0`压栈：

```ascii
│   │
│   │
│   │
│ 0 │
│   │
│ D │
│   │
│ 4 │
└───┘
```

最后计算3÷16=0…3，余数是`3`，把余数`3`压栈：

```ascii
│   │
│ 3 │
│   │
│ 0 │
│   │
│ D │
│   │
│ 4 │
└───┘
```

当商是`0`的时候，计算结束，我们把栈的所有元素依次弹出，组成字符串`30D4`，这就是十进制整数`12500`的十六进制表示的字符串。

###### 计算中缀表达式

在编写程序的时候，我们使用的带括号的数学表达式实际上是中缀表达式，即运算符在中间，例如：`1 + 2 * (9 - 5)`。

但是计算机执行表达式的时候，它并不能直接计算中缀表达式，而是通过编译器把中缀表达式转换为后缀表达式，例如：`1 2 9 5 - * +`。

这个编译过程就会用到栈。我们先跳过编译这一步（涉及运算优先级，代码比较复杂），看看如何通过栈计算后缀表达式。

计算后缀表达式不考虑优先级，直接从左到右依次计算，因此计算起来简单。首先准备一个空的栈：

```ascii
│   │
│   │
│   │
│   │
│   │
│   │
│   │
│   │
└───┘
```

然后我们依次扫描后缀表达式`1 2 9 5 - * +`，遇到数字`1`，就直接扔到栈里：

```ascii
│   │
│   │
│   │
│   │
│   │
│   │
│   │
│ 1 │
└───┘
```

紧接着，遇到数字`2`，`9`，`5`，也扔到栈里：

```ascii
│   │
│ 5 │
│   │
│ 9 │
│   │
│ 2 │
│   │
│ 1 │
└───┘
```

接下来遇到减号时，弹出栈顶的两个元素，并计算`9-5=4`，把结果`4`压栈：

```ascii
│   │
│   │
│   │
│ 4 │
│   │
│ 2 │
│   │
│ 1 │
└───┘
```

接下来遇到`*`号时，弹出栈顶的两个元素，并计算`2*4=8`，把结果`8`压栈：

```ascii
│   │
│   │
│   │
│   │
│   │
│ 8 │
│   │
│ 1 │
└───┘
```

接下来遇到`+`号时，弹出栈顶的两个元素，并计算`1+8=9`，把结果`9`压栈：

```ascii
│   │
│   │
│   │
│   │
│   │
│   │
│   │
│ 9 │
└───┘
```

扫描结束后，没有更多的计算了，弹出栈的唯一一个元素，得到计算结果`9`。

#### Iterator

ava的集合类都可以使用`for each`循环，`List`、`Set`和`Queue`会迭代每个元素，`Map`会迭代每个key。以`List`为例：

```
List<String> list = List.of("Apple", "Orange", "Pear");
for (String s : list) {
    System.out.println(s);
}
```

实际上，Java编译器并不知道如何遍历`List`。上述代码能够编译通过，只是因为编译器把`for each`循环通过`Iterator`改写为了普通的`for`循环：

```
for (Iterator<String> it = list.iterator(); it.hasNext(); ) {
     String s = it.next();
     System.out.println(s);
}
```

我们把这种通过`Iterator`对象遍历集合的模式称为迭代器。

使用迭代器的好处在于，调用方总是以统一的方式遍历各种集合类型，而不必关心它们内部的存储结构。

#### Collections

[使用Collections - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/1252599548343744/1299919855943714)

## 泛型

#### 泛型概述

###### 背景：

JAVA推出泛型以前，程序员可以构建一个元素类型为Object
的集合，该集合能够存储任意的数据类型对象，而在使用该集合的过程中，需要程序员明确知道存储每个元素的数据类型，否则很容易引发ClassCastException异常（类型转化异常），在对集合中的元素处理时极其不方便。

###### 泛型的概念

java泛型提供了编译时类型安全检测机制，该机制允许我们在编译时检测到非法的类型数据结构。**泛型的本质就是参数化类型，也就是所操作的数据类型被指定为一个参数**

好处：

- 类型安全
- 消除了强制类型转换

#### 泛型类、接口

###### 泛型类的定义语法：

class 类名称 <泛型标志，泛型标志，...>{

​	private 泛型标志 变量名;

​	......

}

**常用的泛型标识：T,E,K,V**

**==泛型标识--类型形参==**

**==T	创建对象的时候里指定具体的数据类型（是由外部使用类的时候来指定）==**

###### 使用语法

![image-20221226090949838](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226090949838.png)

**泛型类在创建对象的时候，来指定操作的具体数据类型。**

![image-20221226091925994](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226091925994.png)

**泛型类在创建对象的时候，没有指定类型，将按照Object类型来操作。**

![](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226091925994.png)

**==泛型类不支持基本数据类型。==**

**==同一泛型类，根据不同的数据类型创建的对象，本质上都是这一泛型类的类型==**

###### 抽奖系统

```java
public class ProductGetter<T>{
    Random random = new Random();
    //奖品
    private T product;
    //奖品池
    ArrayList<T> list = new ArrayList<>();
    //添加奖品
    public void addProduct(T t){
        list.add(t);
    }
    //抽奖
    public T getProduct(){
		product = list.get(random.nextInt(list.size()));
        return product;
    }
}
```

#### 泛型类派生

###### 泛型类派生子类

![image-20221226094216703](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226094216703.png)

**泛型类派生子类，子类也是泛型类，那么子类的泛型标识要和父类一致。**

![image-20221226100847543](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226100847543.png)

**泛型类派生子类，如果子类不是泛型类，那么父类要明确数据类型**

![image-20221226100600248](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226100600248.png)

###### 泛型接口

![image-20221226101027484](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226101027484.png)

![image-20221226101052906](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226101052906.png)

#### 泛型方法

![image-20221226101257130](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226101257130.png)

![image-20221226101515781](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226101515781.png)

**==泛型方法的调用，类型是通过调用方法的时候来指定==**

#### 类型通配符

![image-20221226103256256](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226103256256.png)

![image-20221226103312325](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226103312325.png)

###### 类型通配符的上限

![image-20221226103421943](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226103421943.png)

**类型通配符的下限**

![image-20221226103636484](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226103636484.png)

## 异常处理

异常是程序出现了不正常的情况。

因为Java的异常是class，它的继承关系如下：

![image-20221221094752724](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221221094752724.png)

Throwable的父类是Object类

- Error：严重问题，不需要处理
- **Exception：称为异常类，它表示程序本身可以处理的问题**
  - **RuntimeException：在编译前是不检查的，出现问题后，需要我们回来修改代码。**
  - **非RuntimeException:编译期就必须处理的，否则程序不能通过编译，就更不能正常运行了。**

某些异常是应用程序逻辑处理的一部分，应该捕获并处理。例如：

- NumberFormatException：数值类型的格式错误
- FileNotFoundException：未找到文件
- SocketException：读取网络失败

还有一些异常是程序逻辑编写不对造成的，应该修复程序本身。例如：

- NullPointerException：对某个null的对象调用方法或字段
- IndexOutOfBoundsException：数组索引越界

#### JVM的默认处理方案

如果程序出现了问题，我们没有做任何处理，最终JVM会做默认的处理

- 把异常的名称，异常原因及异常出现的位置等信息输出在了控制台。
- 程序停止执行

#### 异常处理之try...catch...

###### 格式：

```java
try{
    可能出现问题的代码；
}catch(异常类名 变量名){
    异常的处理代码；
}
```

###### 执行流程

**程序从try里面的代码开始执行**

**出现异常，会自动生成一个异常类对象，该异常对象将被提价给Java运行时的系统**

**当Java运行时的系统接收到异常对象时，会到catch中去找匹配的异常类，找到后进行异常的处理**

**执行完毕后，程序还可以继续往下执行**

```java
public class ExceptionDemo01{
    public static void main(String[] args){
        System.out.println("开始");
        method();
        System.out.println("结束");
    }
    public static void methods(){
        int []arr = {1,2,3};
        System.out.println(arr[3]);
    }
}
```

![image-20221221160656976](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221221160656976.png)

因为数组越界访问报错并且程序结束

```java
public class ExceptionDemo01{
    public static void main(String[] args){
        System.out.println("开始");
        method();
        System.out.println("结束");
    }
    public static void methods(){
        try{
            int []arr = {1,2,3};
     		System.out.println(arr[3]);	//new 了一个异常对象e
        }catch(ArrayIndexOutOfBoundsException e){
            System.out.println("你访问的数组的索引不存在");
        }
        
    }
}
```

![](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221221161105834.png)*

###### 多catch语句

可以使用多个catch语句，每个catch分别捕获对应的Exception及其子类。JVM在捕获到异常后，会从上到下匹配catch语句，匹配到某个catch后，执行catch代码块，然后不再继续匹配。

**==多个catch语句只有一个能被执行，子类必须写在前面，因为如果父类写在前面，父类是永远不会被捕获到的。==**

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
    } catch (IOException e) {
        System.out.println("IO error");
    }
}
```

###### finally语句

无论是否有异常发生，如果我们都希望执行一些语句。

可以把执行语句写若干遍：正常执行的放到`try`中，每个`catch`再写一遍。

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
        System.out.println("END");
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
        System.out.println("END");
    } catch (IOException e) {
        System.out.println("IO error");
        System.out.println("END");
    }
}
```

**等价于**

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
    } catch (IOException e) {
        System.out.println("IO error");
    } finally {
        System.out.println("END");
    }
}
```

**==finally保证一些代码必须执行且最后执行==**

###### 捕获多种异常

如果某些异常的处理逻辑相同，但是异常本身不存在继承关系，那么就得编写多条`catch`子句：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println("Bad input");
    } catch (NumberFormatException e) {
        System.out.println("Bad input");
    } catch (Exception e) {
        System.out.println("Unknown error");
    }
}
```

因为处理`IOException`和`NumberFormatException`的代码是相同的，所以我们可以把它两用`|`合并到一起：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException | NumberFormatException e) { // IOException或NumberFormatException
        System.out.println("Bad input");
    } catch (Exception e) {
        System.out.println("Unknown error");
    }
}
```

#### Throwable的成员方法

![image-20221221161724232](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221221161724232.png)

public String getMessage()	**异常的原因**

public String toString()			**异常的类名，原因**

public void printStackTrace	**异常的类名，原因，位置**

```java
public class ExceptionDemo01{
    public static void main(String[] args){
        System.out.println("开始");
        method();
        System.out.println("结束");
    }
    public static void methods(){
        try{
            int []arr = {1,2,3};
     		System.out.println(arr[3]);	//new 了一个异常对象e
        }catch(ArrayIndexOutOfBoundsException e){
            System.out.println("e.getMessage");
            System.out.println("e.toString");
            e.printStackTrace;
        }        
    }
}
```

![image-20221221163211819](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221221163211819.png)

![image-20221221163953678](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221221163953678.png)

![image-20221221163935410](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221221163935410.png)

###### 编译时异常和运行时异常的区别

**Java中的异常被分为两大类：编译时异常和运行时异常，也被称为受检异常和非受检异常**

**所有的RuntimeException类及其子类被称为运行时异常，其他的类都是编译时异常**

- **编译时异常：必须显示处理，否则程序就会发生错误，无法通过编译**
- **运行时异常：无需显示处理，也可以和编译时异常一起处理**

**==运行时异常：==**

```java
public class ExceptionDemo03{
	public static void main(String args){
		method();
	}
	public static void method(){
		int []arr = {1,2,3};
		System.out.println(arr[3]);
	}
}
```

![image-20221221165014101](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221221165014101.png)

**编译时异常：**

```java
public class ExceptionDemo03{
	public static void main(String args){
		method();
	}
	public static void method(){
		try{
            String s = "2048-08-09"
        SimpleDateFormat sdf = new SimleDateFormat("yyyy-MM-dd");
        Date d = sdf.parse(s);
        System.out.println(d);
        }catch(ParseException e){
			e.printStackTrace;
        }        
	}
}
```

#### 抛出异常

虽然我们通过try...catch...可以对异常进行处理，但是并不是所有的情况我们都有权限进行异常的处理。

针对这种情况，Java提供了throws处理方案

==**在方法定义的时候，使用`throws Xxx`表示该方法可能抛出的异常类型。调用方在调用的时候，必须强制捕获这些异常，否则编译器会报错。**==

###### throws

**格式：**

```java
throws 异常类名;
```

```java
public class ExceptionDemo03{
	public static void main(String args){
		method();
	}
	public static void method() throws ArrayIndexOutOfBoudsException{
		int []arr = {1,2,3};
		System.out.println(arr[3]);
	}
}
```

![image-20221221224348696](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221221224348696.png)

- **==编译时异常必须要进行处理，两种处理方案：try...catch...或者throws，如果采用throws这种方案，谁来调用谁处理。==**
- **==运行时异常可以不处理，出现问题后，需要我们回来修改代码。==**

###### throw

1. 创建某个Exception的实例；
2. 用throw语句抛出。

```java
void process2(String s) {
    if (s==null) {
        throw new NullPointerException();
    }
}
```

###### throws和throw的区别

**==throws：==**

- 用在方法声明后面，跟的是异常类名

- 表示抛出异常，由该方法的调用者来处理

- 表示出现异常的一种可能性，并不一定会发生这些异常

**==throw：==**

- 用在方法体内，跟的是异常对象名
- 表示抛出异常，由方法体内的语句处理
- 执行throw一定抛出了某种异常

#### 自定义异常

Java标准库定义的常用异常包括：

```ascii
Exception
│
├─ RuntimeException
│  │
│  ├─ NullPointerException
│  │
│  ├─ IndexOutOfBoundsException
│  │
│  ├─ SecurityException
│  │
│  └─ IllegalArgumentException
│     │
│     └─ NumberFormatException
│
├─ IOException
│  │
│  ├─ UnsupportedCharsetException
│  │
│  ├─ FileNotFoundException
│  │
│  └─ SocketException
│
├─ ParseException
│
├─ GeneralSecurityException
│
├─ SQLException
│
└─ TimeoutException
```

当我们在代码中需要抛出异常时，尽量使用JDK已定义的异常类型。例如，参数检查不合法，应该抛出`IllegalArgumentException`：

```
static void process1(int age) {
    if (age <= 0) {
        throw new IllegalArgumentException();
    }
}
```

在一个大型项目中，可以自定义新的异常类型，但是，保持一个合理的异常继承体系是非常重要的。

一个常见的做法是自定义一个`BaseException`作为“根异常”，然后，派生出各种业务类型的异常。

`BaseException`需要从一个适合的`Exception`派生，通常建议从`RuntimeException`派生：

```
public class BaseException extends RuntimeException {
}
```

其他业务类型的异常就可以从`BaseException`派生：

```
public class UserNotFoundException extends BaseException {
}

public class LoginFailedException extends BaseException {
}

...
```

自定义的`BaseException`应该提供多个构造方法：

```
public class BaseException extends RuntimeException {
    public BaseException() {
        super();
    }

    public BaseException(String message, Throwable cause) {
        super(message, cause);
    }

    public BaseException(String message) {
        super(message);
    }

    public BaseException(Throwable cause) {
        super(cause);
    }
}
```

上述构造方法实际上都是原样照抄`RuntimeException`。这样，抛出异常的时候，就可以选择合适的构造方法。通过IDE可以根据父类快速生成子类的构造方法。

#### NullPointerException

在所有的`RuntimeException`异常中，Java程序员最熟悉的恐怕就是`NullPointerException`了。

`NullPointerException`即空指针异常，俗称NPE。如果一个对象为`null`，调用其方法或访问其字段就会产生`NullPointerException`，这个异常通常是由JVM抛出的，例如：

```java
public class Main {
    public static void main(String[] args) {
        String s = null;
        System.out.println(s.toLowerCase());
    }
}
```

指针这个概念实际上源自C语言，Java语言中并无指针。我们定义的变量实际上是引用，Null Pointer更确切地说是Null Reference，不过两者区别不大。

###### 处理NullPointerException

如果遇到`NullPointerException`，我们应该如何处理？首先，必须明确，`NullPointerException`是一种代码逻辑错误，遇到`NullPointerException`，遵循原则是早暴露，早修复，严禁使用`catch`来隐藏这种编码错误：

```
// 错误示例: 捕获NullPointerException
try {
    transferMoney(from, to, amount);
} catch (NullPointerException e) {
}
```

好的编码习惯可以极大地降低`NullPointerException`的产生，例如：

成员变量在定义时初始化：

```
public class Person {
    private String name = "";
}
```

使用空字符串`""`而不是默认的`null`可避免很多`NullPointerException`，编写业务逻辑时，用空字符串`""`表示未填写比`null`安全得多。

返回空字符串`""`、空数组而不是`null`：

```
public String[] readLinesFromFile(String file) {
    if (getFileSize(file) == 0) {
        // 返回空数组而不是null:
        return new String[0];
    }
    ...
}
```

这样可以使得调用方无需检查结果是否为`null`。

如果调用方一定要根据`null`判断，比如返回`null`表示文件不存在，那么考虑返回`Optional<T>`：

```
public Optional<String> readFromFile(String file) {
    if (!fileExist(file)) {
        return Optional.empty();
    }
    ...
}
```

这样调用方必须通过`Optional.isPresent()`判断是否有结果。

## SL4J

前面介绍了Commons Logging和Log4j这一对好基友，它们一个负责充当日志API，一个负责实现日志底层，搭配使用非常便于开发。

有的童鞋可能还听说过SLF4J和Logback。这两个东东看上去也像日志，它们又是啥？

其实SLF4J类似于Commons Logging，也是一个日志接口，而Logback类似于Log4j，是一个日志的实现。

为什么有了Commons Logging和Log4j，又会蹦出来SLF4J和Logback？这是因为Java有着非常悠久的开源历史，不但OpenJDK本身是开源的，而且我们用到的第三方库，几乎全部都是开源的。开源生态丰富的一个特定就是，同一个功能，可以找到若干种互相竞争的开源库。

因为对Commons Logging的接口不满意，有人就搞了SLF4J。因为对Log4j的性能不满意，有人就搞了Logback。

我们先来看看SLF4J对Commons Logging的接口有何改进。在Commons Logging中，我们要打印日志，有时候得这么写：

```
int score = 99;
p.setScore(score);
log.info("Set score " + score + " for Person " + p.getName() + " ok.");
```

拼字符串是一个非常麻烦的事情，所以SLF4J的日志接口改进成这样了：

```
int score = 99;
p.setScore(score);
logger.info("Set score {} for Person {} ok.", score, p.getName());
```

我们靠猜也能猜出来，SLF4J的日志接口传入的是一个带占位符的字符串，用后面的变量自动替换占位符，所以看起来更加自然。

如何使用SLF4J？它的接口实际上和Commons Logging几乎一模一样：

```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

class Main {
    final Logger logger = LoggerFactory.getLogger(getClass());
}
```

对比一下Commons Logging和SLF4J的接口：

| Commons Logging                       | SLF4J                   |
| :------------------------------------ | :---------------------- |
| org.apache.commons.logging.Log        | org.slf4j.Logger        |
| org.apache.commons.logging.LogFactory | org.slf4j.LoggerFactory |

不同之处就是Log变成了Logger，LogFactory变成了LoggerFactory。

使用SLF4J和Logback和前面讲到的使用Commons Logging加Log4j是类似的，先分别下载[SLF4J](https://www.slf4j.org/download.html)和[Logback](https://logback.qos.ch/download.html)，然后把以下jar包放到classpath下：

- slf4j-api-1.7.x.jar
- logback-classic-1.2.x.jar
- logback-core-1.2.x.jar

然后使用SLF4J的Logger和LoggerFactory即可。和Log4j类似，我们仍然需要一个Logback的配置文件，把`logback.xml`放到classpath下，配置如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

	<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
		<encoder>
			<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
		</encoder>
	</appender>

	<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<encoder>
			<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
			<charset>utf-8</charset>
		</encoder>
		<file>log/output.log</file>
		<rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
			<fileNamePattern>log/output.log.%i</fileNamePattern>
		</rollingPolicy>
		<triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
			<MaxFileSize>1MB</MaxFileSize>
		</triggeringPolicy>
	</appender>

	<root level="INFO">
		<appender-ref ref="CONSOLE" />
		<appender-ref ref="FILE" />
	</root>
</configuration>
```

**==暂时认为配置文件可有可无，参考下面博客配置==**

[Sl4J的使用_Sharry洗手溢的博客-CSDN博客](https://blog.csdn.net/weixin_43779187/article/details/128408869?ops_request_misc=%7B%22request%5Fid%22%3A%22170046350716800185891685%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=170046350716800185891685&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-128408869-null-null.142^v96^pc_search_result_base3&utm_term=使用SL4j&spm=1018.2226.3001.4187)

## IO流

#### IO流原理及流的分类

###### Java IO流原理

1. I/O是Input/Output的缩写，I/O技术是非常实用的技术，用于**处理数据传输如读/写文件，网络通讯等。**
2. Java程序中，**对于数据的输入/输出操作以”流(stream)”的方式进行。**
3. java.io包下提供了各种“流”类和接口，用以获取不同种类的数据，并通过方法输入或输出数据
4. **输入input:读取外部数据(磁盘、光盘等存储设备的数据)到程序（内存）中。**
5. **输出output:将程序（内存）数据输出到磁盘、光盘等存储设备中**

###### 流的分类

- **按操作数据单位不同分为:字节流（8 bit）二进制文件，字符流（按字符）文本文件**
- **按数据流的流向不同分为：输入流，输出流**
- **按流的角色的不同分为：节点流，处理流/包装流**

![image-20221226144122676](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226144122676.png)

**以上4个类都是抽象类，Java的IO流所涉及的40多个类都是从以上四个抽象类派生出来的。**

对流的理解

![image-20221226145011775](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226145011775.png)

#### InputStream:字节输入流

**InputStream抽象类是所有类字节输入流的超类**

**==常用的子类==**

1. **FilelnputStream：文件输入流**
2. **BufferedlnputStream：缓冲字节输入流**
3. **ObjectlnputStream：对象字节输入流**

![image-20221226145749749](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221226145749749.png)

###### FileInputStream

最重要的方法就是`int read()`，如下：

```java
public abstract int read() throws IOException;
```

通过`close()`方法来关闭流。关闭流就会释放对应的底层资源。

我们需要用`try ... finally`来保证`InputStream`在无论是否发生IO错误的时候都能够正确地关闭：

这个方法会读取输入流的下一个字节，并返回字节表示的int值（0~255）。如果已读到末尾，返回-1表示不能继续读取了。

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class fileInputStream {
    public static void main(String[] args) {
        readFile1();
    }

    public static void readFile1(){
        String filePath = "C:\\Users\\DELL\\Desktop\\新建 文本文档.txt";
        int readDate = 0;
        FileInputStream fileInputStream = null;
        try {
            //创建FileInputStream对象，用于读取文件
            fileInputStream = new FileInputStream(filePath);
            //从该输入流依次读取一个字节的数据。如果没有输入可用，此方法将停止
            //如果返回1，读取完毕
            while((readDate = fileInputStream.read()) !=-1){
                System.out.print((char)readDate); //转成catch显示
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //关闭文件流，释放资源
            try {
                fileInputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

在读取流的时候，一次读取一个字节并不是最高效的方法。很多流支持一次性读取多个字节到缓冲区，对于文件和网络流来说，利用缓冲区一次性读取多个字节效率往往要高很多。`InputStream`提供了两个重载方法来支持读取多个字节：

- `int read(byte[] b)`：读取若干字节并填充到`byte[]`数组，返回读取的字节数
- `int read(byte[] b, int off, int len)`：指定`byte[]`数组的偏移量和最大填充数

利用上述方法一次读取多个字节时，需要先定义一个`byte[]`数组作为缓冲区，`read()`方法会尽可能多地读取字节到缓冲区， 但不会超过缓冲区的大小。`read()`方法的返回值不再是字节的`int`值，而是返回实际读取了多少个字节。如果返回`-1`，表示没有更多的数据了。

```java
public void readFile2(){
    String filePath = "C:\\Users\\DELL\\Desktop\\新建 文本文档.txt";
    int readDate = 0;
    //字节数组
    byte[] buf = new byte[8];
    FileInputStream fileInputStream = null;
    try {
        fileInputStream = new FileInputStream(filePath);
        //从该输入流读取最多b.length字节的数据到字节数组。此方法将阻塞，直到某些输入可用
        //如果返回-1，表示读取完毕
        //读过读取正常，返回实际读取的字节数
        while ((readDate = fileInputStream.read(buf)) !=-1){
            System.out.print(new String(buf,0,readDate));
        }
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    }finally {
        try {
            fileInputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### OutputStream:字节输出流

![image-20221227093735813](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221227093735813.png)

###### FileoutputStream

```java
public static void writeFile(){
        //创建FileOutputStream对象
        String filePath = "e:\\a.txt";	//如果没有这个文件会自动创建
        FileOutputStream fileOutputStream = null;
        //得到 FileOutputStream对象
        //1.new FileOutputStream(filePath) 创建方式，当写入内容式，会覆盖原来的内容
        //2.new FileOutputStream(filePath,true)创建方式，当写入内容时，内容追加到原来文件后边
        try {
            fileOutputStream = new FileOutputStream(filePath,true);
            //写入一个字节的内容
            //fileOutStteam.write('H');
            //写入字符串
            String str = "hello world";
            //str.getBytes() 可以把 字符串->字节数组
            //fileOutputStream.write(str.getBytes());
            //write(byte[] b,int off,int len)将len字节从位于偏移量off的指定字节数组写入此文件输出流
            fileOutputStream.write(str.getBytes(),0,3);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                fileOutputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
```

###### 文件拷贝

![image-20221227103426004](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221227103426004.png)

```java
public static void copy(){
    //完成文件拷贝，将e:\\Koala.jpg拷贝到c:\\中
    //步骤：1.创建文件的输入流，将文件读入到程序
    //2.创建文件的输出流。将读取到的文件数据，写入到指定的文件
    String srcFilePath = "e:\\Koala.jpg";
    String destFilePath = "c:\\koala.jpg";
    FileInputStream fileInputStream = null;
    FileOutputStream fileOutputStream = null;
    try {
        fileInputStream = new FileInputStream(srcFilePath);
        fileOutputStream = new FileOutputStream(destFilePath,true);
        //定义一个字节数组，提高读取效果
        byte[] buf = new byte [256];
        int readLen = 0;
        while((readLen = fileInputStream.read(buf)) != -1){
            //读取到后，就写入到文件，一边读，一边写
            fileOutputStream.write(buf,0,readLen);
            //防止最后一次读取时未读满字符数组
        }
        System.out.println("拷贝成功");
    } catch (IOException e) {
        e.printStackTrace();
    }finally {
        try {
            if(fileInputStream != null){
                fileInputStream.close();
            }
            if(fileOutputStream != null){
                fileInputStream .close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### FileReader:字符输入流

![image-20221227110110574](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221227110110574.png)

###### FileReader相关方法：

1. **new FileReader(File/String)**
2. **read：每次读取单个字符，返回该字符，如果到文件末尾返回-1**
3. **read(char[])：批量读取多个字符到数组，返回读取到的字符数，如果到文件末尾返回-1**

**相关：**

1. **new String(char[]):将char[]转换成String**
2. **new String(char[],off,len):将char[]的指定部分转换成String**

```java
public static void FileRead1(){
    String filePath = "e:\\s.txt";
    FileReader fileReader = null;
    int date;
    try {
        fileReader = new FileReader(filePath);
        //循环读取，使用read，单个字符读取
        while ((date = fileReader.read()) != -1){
            System.out.print((char)date);
        }
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        if(fileReader != null){
            try {
                fileReader.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

```java
public static void FileRead2(){
    String filePath = "e:\\s.txt";
    int readLen = 0;
    FileReader fileReader = null;
    char []arr = new char[128];
    try {
        fileReader = new FileReader(filePath);
        //循环读取，使用read(arr),返回的是实际读取到的字符数
        //如果返回-1，说明文件结束
        while((readLen = fileReader.read(arr)) != -1){
            System.out.print(new String(arr,0,readLen));
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if (fileReader != null){
            try {
                fileReader.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### FileWriter:字符输出流

![image-20221227111205365](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221227111205365.png)

###### FileWrite常用方法

1. **new FileWrite(File/String):覆盖模式，相当于流的指针在首端。**

2. **new FileWrite(File/String,true):追加模式，相当于流的指针在尾端**

3. **write(int):写入单个字符**

4. **write(char[]):写入指定数组**

5. **write(char[],off,len)写入指定数组的指定部分**

6. **write(String):写入整个字符串**

7. **write(string,off,len):写入字符串的指定部分**

   **String类：toCharArray:将String转换成char[]**
   
   **==对应的FileWriter，一定要关闭流，或者flush才能真正的吧数据写入到文件中==**

```java
public static void FileWrite1(){
    String filePath = "e:\\s.txt";
    FileWriter fileWriter = null;
    char []arr = {'a','b','c'};
    try {
        fileWriter = new FileWriter(filePath);
        //write(int):写入单个字符
        fileWriter.write('H');
        //write(char[]):写入指定数组
        fileWriter.write(arr);
        //write(char[],off,len)写入指定数组的指定部分
        fileWriter.write("韩顺平教育".toCharArray(),0,3);
        //write(String):写入整个字符串
        fileWriter.write("hello world");
        //write(string,off,len):写入字符串的指定部分
        fileWriter.write("上海天津",0,2);
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            fileWriter.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 节点流处理流

**数据源：存放数据的地方**

###### 节点流

![image-20221228103704418](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228103704418.png)

![image-20221228103614416](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228103614416.png)

###### 处理流（包装流）

![image-20221228103750661](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228103750661.png)

![image-20221228103946421](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228103946421.png)

![image-20221228104132956](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228104132956.png)

**节点流和输出流一览图**

![image-20221228104245125](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228104245125.png)

**==区别和联系==**

![image-20221228104819380](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228104819380.png)



**==BufferedReader和BufferedWriter属于字符流，是按照字符来读取数据的。==**

**==关闭时，只需要关闭外层流（处理流）即可。（在关闭处理流式，底层会自动关闭它封装的节点流）==**

###### BufferedReader

==**BufferedReader类中，有属性Reader，即可以封装一个节点流（Reader的子类）**==

![image-20221228104109770](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228104109770.png)

```java
public static void BufferedReader1(){
    String filePath = "e:\\s.txt";
    BufferedReader bufferedReader = null;
    try {
        bufferedReader = new BufferedReader(new FileReader(filePath));
        //读取
        String line;    //按行读取，效率高
        //说明
        //bufferedReader.readLine是按行读取文件
        //正常读取返回String，返回null表示文件读取完毕
        while((line = bufferedReader.readLine()) != null){
            System.out.println(line);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }finally {
        try {
            //关闭流，这里注意，只需要关闭BufferedReader,因为底层会自动关闭节点流
            bufferedReader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

}
```

###### BufferedWriter

**BufferedWriter类中，有属性Writer，即可以封装一个节点流（Writer的子类）**

```java
public static void BufferedWriter1(){
    String filePath = "e:\\s.txt";
    BufferedWriter bufferedWriter = null;
    try {
        bufferedWriter = new BufferedWriter(new FileWriter(filePath,true));
        bufferedWriter.write("hello world");
        bufferedWriter.newLine();   //插入一个和系统相关的换行
        bufferedWriter.write("hello china");
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            bufferedWriter.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

**==文件转存==**

```java
public static void readFile(){
    String srcFilePath = "e:\\s.txt";
    String destFilePath = "e:\\s1.txt";
    BufferedReader bufferedReader = null;
    BufferedWriter bufferedWriter = null;
    String line;
    try {
        bufferedReader = new BufferedReader(new FileReader(srcFilePath));
        bufferedWriter = new BufferedWriter(new FileWriter(destFilePath,true));
        while ((line = bufferedReader.readLine()) != null){
            bufferedWriter.write(line);
            bufferedWriter.newLine();
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if (bufferedReader != null){
            try {
                bufferedReader.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if (bufferedWriter != null){
            try {
                bufferedWriter.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

###### BufferedInputStream

**BufferedInputStream类中，有属性InputStream，即可以封装一个节点流（InputStream的子类）**

BufferedInputStream是字节流，在创建BufferedInputStream时，会创建一个内部缓冲区数组

###### BufferedOutputStream

**BufferedOutputStream类中，有属性OutputStream，即可以封装一个节点流（OutputStream的子类）**

BufferedOutputStream是字节流，实现缓冲的输出流，可以将多个字节写入底层输出流中，而不必对每个字节写入调用底层系统

###### 二进制文件拷贝

```java
public static void Bufferedcopy(){
    String srcFilePath ="C:\\Users\\DELL\\Pictures\\Screenshots\\屏幕截图_20221205_173725.png" ;
    String destFilePath = "e:\\s.png";
    BufferedInputStream bufferedInputStream = null;
    BufferedOutputStream bufferedOutputStream = null;

    try {
        bufferedInputStream = new BufferedInputStream(new FileInputStream(srcFilePath));
        bufferedOutputStream = new BufferedOutputStream(new FileOutputStream(destFilePath));
        //循环读取文件，并写入
        byte[] buff = new byte[1024];
        int readLen = 0;
        while ((readLen = bufferedInputStream.read(buff)) != -1){
            bufferedOutputStream.write(buff,0,readLen);
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        //关闭流，关闭外层的处理流即可，底层会去关闭节点流
        try {
            if (bufferedInputStream != null){
                bufferedInputStream.close();
            }
            if (bufferedOutputStream != null){
                bufferedOutputStream.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 对象处理流

**==对象流 ObjectInputStream和ObjectOutputStream==**

**看一个需求**

1. **将int num = 100这个int数据保存到文件中，注意不是100数字，而是int 100，并且，能够将文件中直接恢复int 100**
2. **将Dog dog = new Dog("小黄",3)这个Dog对象保存到文件中，并且能够从文件中恢复**
3. **上面的要求，就是能够将基本数据类型或者对象进行序列化和反序列化操作**

**序列化和反序列化**

1. **序列化就是在保存数据时，保存数据的值和数据类型**

2. **反序列化就是在恢复数据时，恢复数据的值和数据类型**

3. **需要让某个对象支持序列化序列化机制，则必须让其类是可序列化的，为了让某个类是可序列化的，该类必须实现如下两个接口之一：**

   **Serializable	//这是一个标记接口（里面没有任何方法）,使用这个**

   **Externalizable	//该接口有方法实现，一般使用上面的方法**

![image-20221228152009619](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228152009619.png)

1. **==功能：提供了对基本类型或对象类型的序列化和反序列化的方法==**
2. **==ObjectOutputStream提供序列化功能==**
3. **==ObjectOutputStream提供反序列化功能==**

###### ObjectOutputStream

![image-20221228152514346](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228152514346.png)

![image-20221228153609431](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228153609431.png)

```java
public static void ObjectOutStream(){
        //序列化后，保存的文件格式，不是存文本，而是按照它的格式来保存
        String filePath = "e:\\data.txt";
        ObjectOutputStream objectOutputStream = null;
        try {
            objectOutputStream = new ObjectOutputStream(new FileOutputStream(filePath));
            //序列化数据到文件中
            objectOutputStream.writeInt(100);  //int -> Integer(实现了Serializable)
            objectOutputStream.writeBoolean(true);  //boolean -> Boolean(实现了Serializable)
            objectOutputStream.writeChar('a');  //char -> Character(实现了Serializable)
            objectOutputStream.writeDouble(9.5);  //double -> Double(实现了Serializable)
            objectOutputStream.writeUTF("韩顺平教育");   //String
            objectOutputStream.writeObject(new Dog("旺柴",10));
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                objectOutputStream.close();
                System.out.println("数据保存完毕（序列化格式）");
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    public static class Dog implements Serializable{
        private String name;
        private int age;

        public Dog(String name, int age) {
            this.name = name;
            this.age = age;
        }

        @Override
        public String toString() {
            return "Dog{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    '}';
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
    }
```

###### ObjectInputStream

![image-20221228152435771](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228152435771.png)

![image-20221228162532836](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228162532836.png)

```java
public static void ObjectInputStream(){
    //指定反序列化的文件
    String filePath = "e:\\data.txt";
    ObjectInputStream objectInputStream = null;
    try {
        objectInputStream = new ObjectInputStream(new FileInputStream(filePath));
        //读取
        //读取（反序列化）的顺序需要和你保存数据（序列化）的顺序一致
        //否则会出现异常
        System.out.println(objectInputStream.readInt());
        System.out.println(objectInputStream.readBoolean());
        System.out.println(objectInputStream.readChar());
        System.out.println(objectInputStream.readDouble());
        System.out.println(objectInputStream.readUTF());
        Object dog = objectInputStream.readObject();
        System.out.println(dog);
        //dog的编译类型为object，运行类型为Dog
        //如果我们需要调用Dog的方法，需要向下转型
        //需要我们将Dog类的定义，拷贝到能够引用的地方
        afa.Dog dog2 = (afa.Dog)dog;
        System.out.println(dog2.getName());
    } catch (IOException e) {
        e.printStackTrace();
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } finally {
        try {
            if (objectInputStream != null) {
                objectInputStream.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

###### 对象处理流使用细节

1. 读写顺序要一致

2. 要求实现序列化或反序列化对象，需要实现Serializable

3. 序列化的类中建议添加SerialVersionUID，为了提高版本的兼容性（了解就行）

   ![image-20221228171952764](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228171952764.png)

4. 序列化对象时，默认将里面所有的类型都进行序列化，但除了static或transient修饰的成员（这些属性不会被保存）

   ![image-20221228172308710](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228172308710.png)

5. 序列化对象时，要求里面属性的类型也需要实现序列化接口，**（Dog类中有属性Master（主人），则Mastrer类应该实现序列化接口）**

6. 序列化具备可继承性，也就是如果某类已经实现了序列化，则它的所有子类也已经默认实现了序列化。67 

#### 标准输入输出流

![image-20221228214047845](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221228214047845.png)

**System.in是System类的public final static InputStream in = null**

**System.in 编译类型 InputStream**

**System.in 运行类型 BufferedInputStream**

**表示标准输入 键盘**

![image-20221229091115083](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229091115083.png)

**System.out是Stream类的public final static PrintStream out = null;**

**编译类型 PrintfStream**

**运行类型 PrintfStream**

**表示标准输出 显示器**

#### 转换流

![image-20221229091748607](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229091748607.png)

![image-20221229092115594](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229092115594.png)

![image-20221229092100742](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229092100742.png)

**==默认情况下，读取文件是按照utf-8编码==**

![image-20221229092225358](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229092225358.png)

**==文件编码如果是其他编码，就会出现乱码==**

###### 转化流InputStreamReader和OutPutStreamWriter

1. InputStreamReader:Reader的子类，可以将InputStream（字节流）包装（转换）成Reader（字符流）
2. OutputStreamWriter:Writer的子类，实现将OutputStream（字节流）包装（转换）成Writer（字符流）
3. 当处理纯文本数据时。如果用字符流效率更高，并且可以有效解决中文问题，所以建议将字节流转换成字符流
4. 可以在使用时指定编码格式（比如utf-8,gbk,gb2312,ISO8859-1等）

###### InputStreamReader

![image-20221229094128252](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229094128252.png)

**==Charset指的是编码==**

**应用案例**

![image-20221229094506829](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229094506829.png)

![image-20221229094856796](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229094856796.png)

###### OutputStreamWriter

![image-20221229094338948](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229094338948.png)

**==Charset指的是编码==**

**应用案例**

![image-20221229095046605](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229095046605.png)

![image-20221229095329903](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229095329903.png)

###### 打印流

**==PrintStream和PrintWriter==**

**==打印流只有输出流，没有输入流==**

![image-20221229100606337](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229100606337.png)

![image-20221229100717876](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229100717876.png)

###### PrintStream（字节打印流）

![image-20221229101204162](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229101204162.png)

![image-20221229101435912](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229101435912.png)

###### printWriter（字符打印流）

![image-20221229101721793](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221229101721793.png)

## 多线程

#### 多线程基础

现代操作系统（Windows，macOS，Linux）都可以执行多任务。多任务就是同时运行多个任务，例如：

CPU执行代码都是一条一条顺序执行的，但是，即使是单核cpu，也可以同时运行多个任务。**因为操作系统执行多任务实际上就是让CPU对多个任务轮流交替执行。**

例如，假设我们有语文、数学、英语3门作业要做，每个作业需要30分钟。我们把这3门作业看成是3个任务，可以做1分钟语文作业，再做1分钟数学作业，再做1分钟英语作业：

这样轮流做下去，在某些人眼里看来，做作业的速度就非常快，看上去就像同时在做3门作业一样

**类似的，操作系统轮流让多个任务交替执行，例如，让浏览器执行0.001秒，让QQ执行0.001秒，再让音乐播放器执行0.001秒，在人看来，CPU就是在同时执行多个任务。**

即使是多核CPU，因为通常任务的数量远远多于CPU的核数，所以任务也是交替执行的。

###### 进程

在计算机中，我们把一个任务称为一个进程，浏览器就是一个进程，视频播放器是另一个进程，类似的，音乐播放器和Word都是进程。

某些进程内部还需要同时执行多个子任务。例如，我们在使用Word时，Word可以让我们一边打字，一边进行拼写检查，同时还可以在后台进行打印，我们把子任务称为线程。

进程和线程的关系就是：一个进程可以包含一个或多个线程，但至少会有一个线程。

```ascii
                        ┌──────────┐
                        │Process   │
                        │┌────────┐│
            ┌──────────┐││ Thread ││┌──────────┐
            │Process   ││└────────┘││Process   │
            │┌────────┐││┌────────┐││┌────────┐│
┌──────────┐││ Thread ││││ Thread ││││ Thread ││
│Process   ││└────────┘││└────────┘││└────────┘│
│┌────────┐││┌────────┐││┌────────┐││┌────────┐│
││ Thread ││││ Thread ││││ Thread ││││ Thread ││
│└────────┘││└────────┘││└────────┘││└────────┘│
└──────────┘└──────────┘└──────────┘└──────────┘
┌──────────────────────────────────────────────┐
│               Operating System               │
└──────────────────────────────────────────────┘
```

操作系统调度的最小任务单位其实不是进程，而是线程。常用的Windows、Linux等操作系统都采用抢占式多任务，如何调度线程完全由操作系统决定，程序自己不能决定什么时候执行，以及执行多长时间。

因为同一个应用程序，既可以有多个进程，也可以有多个线程，因此，实现多任务的方法，有以下几种：

多进程模式（每个进程只有一个线程）：

```ascii
┌──────────┐ ┌──────────┐ ┌──────────┐
│Process   │ │Process   │ │Process   │
│┌────────┐│ │┌────────┐│ │┌────────┐│
││ Thread ││ ││ Thread ││ ││ Thread ││
│└────────┘│ │└────────┘│ │└────────┘│
└──────────┘ └──────────┘ └──────────┘
```

**==多线程模式（一个进程有多个线程）：==**

```ascii
┌────────────────────┐
│Process             │
│┌────────┐┌────────┐│
││ Thread ││ Thread ││
│└────────┘└────────┘│
│┌────────┐┌────────┐│
││ Thread ││ Thread ││
│└────────┘└────────┘│
└────────────────────┘
```

多进程＋多线程模式（复杂度最高）：

```ascii
┌──────────┐┌──────────┐┌──────────┐
│Process   ││Process   ││Process   │
│┌────────┐││┌────────┐││┌────────┐│
││ Thread ││││ Thread ││││ Thread ││
│└────────┘││└────────┘││└────────┘│
│┌────────┐││┌────────┐││┌────────┐│
││ Thread ││││ Thread ││││ Thread ││
│└────────┘││└────────┘││└────────┘│
└──────────┘└──────────┘└──────────┘
```

###### 进程 vs 线程

进程和线程是包含关系，但是多任务既可以由多进程实现，也可以由单进程内的多线程实现，还可以混合多进程＋多线程。

具体采用哪种方式，要考虑到进程和线程的特点。

和多线程相比，多进程的缺点在于：

- 创建进程比创建线程开销大，尤其是在Windows系统上；
- 进程间通信比线程间通信要慢，因为线程间通信就是读写同一个变量，速度很快。

而多进程的优点在于：

多进程稳定性比多线程高，因为在多进程的情况下，一个进程崩溃不会影响其他进程，而在多线程的情况下，任何一个线程崩溃会直接导致整个进程崩溃。

###### 多线程

**Java语言内置了多线程支持：一个Java程序实际上是一个JVM进程，JVM进程用一个主线程来执行`main()`方法，在`main()`方法内部，我们又可以启动多个线程。此外，JVM还有负责垃圾回收的其他工作线程等。**

因此，对于大多数Java程序来说，我们说多任务，实际上是说如何使用多线程实现多任务。

和单线程相比，多线程编程的特点在于：多线程经常需要读写共享数据，并且需要同步。例如，播放电影时，就必须由一个线程播放视频，另一个线程播放音频，两个线程需要协调运行，否则画面和声音就不同步。因此，多线程编程的复杂度高，调试更困难。

Java多线程编程的特点又在于：

- 多线程模型是Java程序最基本的并发模型；
- 后续读写网络、数据库、Web开发等都依赖Java多线程模型。

因此，必须掌握Java多线程编程才能继续深入学习其他内容。

#### 创建新线程

**Java语言内置了多线程支持。当Java程序启动的时候，实际上是启动了一个JVM进程，然后，JVM启动主线程来执行`main()`方法。在`main()`方法中，我们又可以启动其他线程。**

要**==创建一个新线程非常容易，我们需要实例化一个`Thread`实例，然后调用它的`start()`方法==**：

```java
public class Main {
    public static void main(String[] args) {
        Thread t = new Thread();
        t.start(); // 启动新线程
    }
}
```

但是**这个线程启动后实际上什么也不做就立刻结束了。我们希望新线程能执行指定的代码，有以下几种方法：**

==方法一：从`Thread`派生一个自定义类，然后覆写`run()`方法：==

```java
public class Main {
    public static void main(String[] args) {
        Thread t = new MyThread();
        t.start(); // 启动新线程
    }
}

class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("start new thread!");
    }
}
```

执行上述代码，注意到**==`start()`方法会在内部自动调用实例的`run()`方法。==**

方法二：创建`Thread`实例时，传入一个`Runnable`实例：

```java
public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable());
        t.start(); // 启动新线程
    }
}

class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("start new thread!");
    }
}
```

或者用Java8引入的lambda语法进一步简写为：

```java
public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(() -> {
            System.out.println("start new thread!");
        });
        t.start(); // 启动新线程
    }
}
```

###### 自定义线程和main方法执行的区别

使用线程执行的打印语句，和直接在`main()`方法执行有区别吗？

区别大了去了。我们看以下代码：

![image-20231203172404470](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20231203172404470.png)

我们用蓝色表示主线程，也就是`main`线程，`main`线程执行的代码有4行，**首先打印`main start`，然后创建`Thread`对象，紧接着调用`start()`启动新线程。当`start()`方法被调用时，JVM就创建了一个新线程，我们通过实例变量`t`来表示这个新线程对象，并开始执行。**

**接着，`main`线程继续执行打印`main end`语句，而`t`线程在`main`线程执行的同时会并发执行，打印`thread run`和`thread end`语句。**

**当`run()`方法结束时，新线程就结束了。而`main()`方法结束时，主线程也结束了。**

我们再来看线程的执行顺序：

1. `main`线程肯定是先打印`main start`，再打印`main end`；
2. `t`线程肯定是先打印`thread run`，再打印`thread end`。

但是，除了可以肯定，`main start`会先打印外，**`main end`打印在`thread run`之前、`thread end`之后或者之间，都无法确定。因为从`t`线程开始运行以后，两个线程就开始同时运行了，并且由操作系统调度，程序本身无法确定线程的调度顺序。**

**要模拟并发执行的效果，我们可以在线程中调用`Thread.sleep()`，强迫当前线程暂停一段时间**：

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("main start...");
        Thread t = new Thread() {
            public void run() {
                System.out.println("thread run...");
                try {
                    Thread.sleep(10);		//t线程sleep10ms，main线程sleep时间到，此时只有main一个线程，执行main end...
                } catch (InterruptedException e) {}
                System.out.println("thread end.");	//t线程苏醒，执行thread end。
            }
        };
        t.start();
        try {
            Thread.sleep(20);		//main线程sleep20ms，此时只有t一个线程，执行t的run方法 thread rum...
        } catch (InterruptedException e) {}
        System.out.println("main end...");
    }
}
```

![image-20231203173744439](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20231203173744439.png)

**`sleep()`传入的参数是毫秒。调整暂停时间的大小，我们可以看到`main`线程和`t`线程执行的先后顺序。**

**==要特别注意：直接调用`Thread`实例的`run()`方法是无效的：==**

```
public class Main {
    public static void main(String[] args) {
        Thread t = new MyThread();
        t.run();
    }
}

class MyThread extends Thread {
    public void run() {
        System.out.println("hello");
    }
}
```

直接调用`run()`方法，相当于调用了一个普通的Java方法，当前线程并没有任何改变，也不会启动新线程。上述代码实际上是在`main()`方法内部又调用了`run()`方法，打印`hello`语句是在`main`线程中执行的，没有任何新线程被创建。

必须调用`Thread`实例的`start()`方法才能启动新线程，如果我们查看`Thread`类的源代码，会看到`start()`方法内部调用了一个`private native void start0()`方法，`native`修饰符表示这个方法是由JVM虚拟机内部的C代码实现的，不是由Java代码实现的。

###### 线程的优先级

可以对线程设定优先级，设定优先级的方法是：

```
Thread.setPriority(int n) // 1~10, 默认值5
```

JVM自动把1（低）~10（高）的优先级映射到操作系统实际优先级上（不同操作系统有不同的优先级数量）。优先级高的线程被操作系统调度的优先级较高，操作系统对高优先级线程可能调度更频繁，但我们决不能通过设置优先级来确保高优先级的线程一定会先执行。

#### 线程的状态

**==在Java程序中，一个线程对象只能调用一次`start()`方法启动新线程，并在新线程中执行`run()`方法。一旦`run()`方法执行完毕，线程就结束了。==**因此，Java线程的状态有以下几种：

- **New：新创建的线程，尚未执行；**
- **Runnable：运行中的线程，正在执行`run()`方法的Java代码；**
- **Blocked：运行中的线程，因为某些操作被阻塞而挂起；**
- **Waiting：运行中的线程，因为某些操作在等待中；**
- **Timed Waiting：运行中的线程，因为执行`sleep()`方法正在计时等待；**
- **Terminated：线程已终止，因为`run()`方法执行完毕。**

用一个状态转移图表示如下：

```ascii
         ┌─────────────┐
         │     New     │
         └─────────────┘
                │
                ▼
┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐
 ┌─────────────┐ ┌─────────────┐
││  Runnable   │ │   Blocked   ││
 └─────────────┘ └─────────────┘
│┌─────────────┐ ┌─────────────┐│
 │   Waiting   │ │Timed Waiting│
│└─────────────┘ └─────────────┘│
 ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─
                │
                ▼
         ┌─────────────┐
         │ Terminated  │
         └─────────────┘
```

当线程启动后，它可以在`Runnable`、`Blocked`、`Waiting`和`Timed Waiting`这几个状态之间切换，直到最后变成`Terminated`状态，线程终止。

**线程终止的原因有：**

- **线程正常终止：`run()`方法执行到`return`语句返回；**
- **线程意外终止：`run()`方法因为未捕获的异常导致线程终止；**
- **对某个线程的`Thread`实例调用`stop()`方法强制终止（强烈不推荐使用）。**

**==一个线程还可以等待另一个线程直到其运行结束。例如，`main`线程在启动`t`线程后，可以通过`t.join()`等待`t`线程结束后再继续运行：==**

```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new Thread(() -> {
            System.out.println("hello");
        });
        System.out.println("start");
        t.start();
        t.join();
        System.out.println("end");
    }
}
```

![image-20231203175224052](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20231203175224052.png)

**当`main`线程对线程对象`t`调用`join()`方法时，主线程将等待变量`t`表示的线程运行结束，即`join`就是指等待该线程结束，然后才继续往下执行自身线程。所以，上述代码打印顺序可以肯定是`main`线程先打印`start`，`t`线程再打印`hello`，`main`线程最后再打印`end`。**

如果`t`线程已经结束，对实例`t`调用`join()`会立刻返回。此外，`join(long)`的重载方法也可以指定一个等待时间，超过等待时间后就不再继续等待。

#### 中断进程

**如果线程需要执行一个长时间任务，就可能需要能中断线程。中断线程就是其他线程给该线程发一个信号，该线程收到信号后结束执行`run()`方法，使得自身线程能立刻结束运行。**

我们举个栗子：假设从网络下载一个100M的文件，如果网速很慢，用户等得不耐烦，就可能在下载过程中点“取消”，这时，程序就需要中断下载线程的执行。

==**中断一个线程非常简单，只需要在其他线程中对目标线程调用`interrupt()`方法，目标线程需要反复检测自身状态是否是interrupted状态，如果是，就立刻结束运行。**==

我们还是看示例代码：

```
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new MyThread();
        t.start();
        Thread.sleep(1); // 暂停1毫秒
        t.interrupt(); // 中断t线程
        t.join(); // 等待t线程结束
        System.out.println("end");
    }
}

class MyThread extends Thread {
    public void run() {
        int n = 0;
        while (! isInterrupted()) {
            n ++;
            System.out.println(n + " hello!");
        }
    }
}
```

**==`main`线程通过调用`t.interrupt()`方法中断`t`线程，但是要注意，`interrupt()`方法仅仅向`t`线程发出了“中断请求”，至于`t`线程是否能立刻响应，要看具体代码。而`t`线程的`while`循环会检测`isInterrupted()`，所以上述代码能正确响应`interrupt()`请求，使得自身立刻结束运行`run()`方法。==**

**如果线程处于等待状态，例如，`t.join()`会让`main`线程进入等待状态，此时，如果对`main`线程调用`interrupt()`，`join()`方法会立刻抛出`InterruptedException`，因此，目标线程只要捕获到`join()`方法抛出的`InterruptedException`，就说明有其他线程对其调用了`interrupt()`方法，通常情况下该线程应该立刻结束运行。**

```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new MyThread();
        t.start();
        Thread.sleep(1000);
        t.interrupt(); // 中断t线程
        t.join(); // 等待t线程结束
        System.out.println("end");
    }
}

class MyThread extends Thread {
    public void run() {
        Thread hello = new HelloThread();
        hello.start(); // 启动hello线程
        try {
            hello.join(); // 等待hello线程结束
        } catch (InterruptedException e) {
            System.out.println("interrupted!");
            //这儿接收到中断指令后，不再执行等待，继续执行run最后一步中断hello线程，而后该MyThread线程中断
        }
        hello.interrupt();		
    }
}

class HelloThread extends Thread {
    public void run() {
        int n = 0;
        while (!isInterrupted()) {
            n++;
            System.out.println(n + " hello!");
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                break;
            }
        }
    }
}
```

**`main`线程通过调用`t.interrupt()`从而通知`t`线程中断，而此时`t`线程正位于`hello.join()`的等待中，此方法会立刻结束等待并抛出`InterruptedException`。由于我们在`t`线程中捕获了`InterruptedException`，因此，就可以准备结束该线程。在`t`线程结束前，对`hello`线程也进行了`interrupt()`调用通知其中断。如果去掉这一行代码，可以发现`hello`线程仍然会继续运行，且JVM不会退出。**

**==另一个常用的中断线程的方法是设置标志位。我们通常会用一个`running`标志位来标识线程是否应该继续运行，在外部线程中，通过把`HelloThread.running`置为`false`，就可以让线程结束：==**

```java
public class Main {
    public static void main(String[] args)  throws InterruptedException {
        HelloThread t = new HelloThread();
        t.start();
        Thread.sleep(1);
        t.running = false; // 标志位置为false
    }
}

class HelloThread extends Thread {
    public volatile boolean running = true;
    public void run() {
        int n = 0;
        while (running) {
            n ++;
            System.out.println(n + " hello!");
        }
        System.out.println("end!");
    }
}
```

**==注意到`HelloThread`的标志位`boolean running`是一个线程间共享的变量。线程间共享变量需要使用`volatile`关键字标记，确保每个线程都能读取到更新后的变量值。==**

为什么要对线程间共享的变量用关键字`volatile`声明？这涉及到Java的内存模型。在Java虚拟机中，变量的值保存在主内存中，但是，当线程访问变量时，它会先获取一个副本，并保存在自己的工作内存中。如果线程修改了变量的值，虚拟机会在某个时刻把修改后的值回写到主内存，但是，这个时间是不确定的！

```ascii
┌ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┐
           Main Memory
│                               │
   ┌───────┐┌───────┐┌───────┐
│  │ var A ││ var B ││ var C │  │
   └───────┘└───────┘└───────┘
│     │ ▲               │ ▲     │
 ─ ─ ─│─│─ ─ ─ ─ ─ ─ ─ ─│─│─ ─ ─
      │ │               │ │
┌ ─ ─ ┼ ┼ ─ ─ ┐   ┌ ─ ─ ┼ ┼ ─ ─ ┐
      ▼ │               ▼ │
│  ┌───────┐  │   │  ┌───────┐  │
   │ var A │         │ var C │
│  └───────┘  │   │  └───────┘  │
   Thread 1          Thread 2
└ ─ ─ ─ ─ ─ ─ ┘   └ ─ ─ ─ ─ ─ ─ ┘
```

这会导致如果一个线程更新了某个变量，另一个线程读取的值可能还是更新前的。例如，主内存的变量`a = true`，线程1执行`a = false`时，它在此刻仅仅是把变量`a`的副本变成了`false`，主内存的变量`a`还是`true`，在JVM把修改后的`a`回写到主内存之前，其他线程读取到的`a`的值仍然是`true`，这就造成了多线程之间共享的变量不一致。

因此，`volatile`关键字的目的是告诉虚拟机：

- 每次访问变量时，总是获取主内存的最新值；
- 每次修改变量后，立刻回写到主内存。

`volatile`关键字解决的是可见性问题：当一个线程修改了某个共享变量的值，其他线程能够立刻看到修改后的值。

如果我们去掉`volatile`关键字，运行上述程序，发现效果和带`volatile`差不多，这是因为在x86的架构下，JVM回写主内存的速度非常快，但是，换成ARM的架构，就会有显著的延迟。

#### 守护线程

Java程序入口就是由JVM启动`main`线程，`main`线程又可以启动其他线程。当所有线程都运行结束时，JVM退出，进程结束。

如果有一个线程没有退出，JVM进程就不会退出。所以，必须保证所有线程都能及时结束。

但是有一种线程的目的就是无限循环，例如，一个定时触发任务的线程：

```
class TimerThread extends Thread {
    @Override
    public void run() {
        while (true) {
            System.out.println(LocalTime.now());
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                break;
            }
        }
    }
}
```

如果这个线程不结束，JVM进程就无法结束。问题是，由谁负责结束这个线程？

然而这类线程经常没有负责人来负责结束它们。但是，当其他线程结束时，JVM进程又必须要结束，怎么办？

答案是使用守护线程（Daemon Thread）。

守护线程是指为其他线程服务的线程。在JVM中，所有非守护线程都执行完毕后，无论有没有守护线程，虚拟机都会自动退出。

因此，JVM退出时，不必关心守护线程是否已结束。

如何创建守护线程呢？方法和普通线程一样，只是在调用`start()`方法前，调用`setDaemon(true)`把该线程标记为守护线程：

```
Thread t = new MyThread();
t.setDaemon(true);
t.start();
```

在守护线程中，编写代码要注意：守护线程不能持有任何需要关闭的资源，例如打开文件等，因为虚拟机退出时，守护线程没有任何机会来关闭文件，这会导致数据丢失。

#### 线程同步

==**当多个线程同时运行时，线程的调度由操作系统决定，程序本身无法决定。因此，任何一个线程都有可能在任何指令处被操作系统暂停，然后在某个时间段后继续执行。**==

==**这个时候，有个单线程模型下不存在的问题就来了：如果多个线程同时读写共享变量，会出现数据不一致的问题。**==

我们来看一个例子：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        var add = new AddThread();
        var dec = new DecThread();
        add.start();
        dec.start();
        add.join();
        dec.join();
        System.out.println(Counter.count);
    }
}

class Counter {
    public static int count = 0;
}

class AddThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) { Counter.count += 1; }
    }
}

class DecThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) { Counter.count -= 1; }
    }
}
```

上面的代码很简单，两个线程同时对一个`int`变量进行操作，一个加10000次，一个减10000次，最后结果应该是0，但是，每次运行，结果实际上都是不一样的。

这是因为**对变量进行读取和写入时，结果要正确，必须保证是原子操作。原子操作是指不能被中断的一个或一系列操作。**

例如，对于语句：

```
n = n + 1;
```

看上去是一行语句，实际上对应了3条指令：

```
ILOAD
IADD
ISTORE
```

我们假设`n`的值是`100`，如果两个线程同时执行`n = n + 1`，得到的结果很可能不是`102`，而是`101`，原因在于：

```ascii
┌───────┐    ┌───────┐
│Thread1│    │Thread2│
└───┬───┘    └───┬───┘
    │            │
    │ILOAD (100) │
    │            │ILOAD (100)
    │            │IADD
    │            │ISTORE (101)
    │IADD        │
    │ISTORE (101)│
    ▼            ▼
```

**如果线程1在执行`ILOAD`后被操作系统中断，此刻如果线程2被调度执行，它执行`ILOAD`后获取的值仍然是`100`，最终结果被两个线程的`ISTORE`写入后变成了`101`，而不是期待的`102`。**

这说明**==多线程模型下，要保证逻辑正确，对共享变量进行读写时，必须保证一组指令以原子方式执行：即某一个线程执行时，其他线程必须等待：==**

```ascii
┌───────┐     ┌───────┐
│Thread1│     │Thread2│
└───┬───┘     └───┬───┘
    │             │
    │-- lock --   │
    │ILOAD (100)  │
    │IADD         │
    │ISTORE (101) │
    │-- unlock -- │
    │             │-- lock --
    │             │ILOAD (101)
    │             │IADD
    │             │ISTORE (102)
    │             │-- unlock --
    ▼             ▼
```

**==通过加锁和解锁的操作，就能保证3条指令总是在一个线程执行期间，不会有其他线程会进入此指令区间。==**即使在执行期线程被操作系统中断执行，其他线程也会因为无法获得锁导致无法进入此指令区间。只有执行线程将锁释放后，其他线程才有机会获得锁并执行。**==这种加锁和解锁之间的代码块我们称之为临界区（Critical Section），任何时候临界区最多只有一个线程能执行。==**

可见，保证一段代码的原子性就是通过加锁和解锁实现的。

###### synchronized

Java程序使用`synchronized`关键字对一个对象进行加锁：

```
synchronized(lock) {
    n = n + 1;
}
```

`synchronized`保证了代码块在任意时刻最多只有一个线程能执行。我们把上面的代码用`synchronized`改写如下：

```java
public class Main {
    public static void main(String[] args) throws Exception {
        var add = new AddThread();
        var dec = new DecThread();
        add.start();
        dec.start();
        add.join();
        dec.join();
        System.out.println(Counter.count);
    }
}

class Counter {
    public static final Object lock = new Object();
    public static int count = 0;
}

class AddThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {
                Counter.count += 1;
            }
        }
    }
}

class DecThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {
                Counter.count -= 1;
            }
        }
    }
}
```

**==它表示用`Counter.lock`实例作为锁，两个线程在执行各自的`synchronized(Counter.lock) { ... }`代码块时，必须先获得锁，才能进入代码块进行。执行结束后，在`synchronized`语句块结束会自动释放锁。这样一来，对`Counter.count`变量进行读写就不可能同时进行。==**上述代码无论运行多少次，最终结果都是0。

使用`synchronized`解决了多线程同步访问共享变量的正确性问题。但是，它的缺点是带来了性能下降。因为`synchronized`代码块无法并发执行。此外，加锁和解锁需要消耗一定的时间，所以，`synchronized`会降低程序的执行效率。

==**我们来概括一下如何使用`synchronized`：**==

1. ==**找出修改共享变量的线程代码块；**==
2. ==**选择一个共享实例作为锁；**==
3. ==**使用`synchronized(lockObject) { ... }`。**==

在使用`synchronized`的时候，不必担心抛出异常。因为无论是否有异常，都会在`synchronized`结束处正确释放锁：

```
public void add(int m) {
    synchronized (obj) {
        if (m < 0) {
            throw new RuntimeException();
        }
        this.value += m;
    } // 无论有无异常，都会在此释放锁
}
```

我们再来看一个错误使用`synchronized`的例子：

```
public class Main {
    public static void main(String[] args) throws Exception {
        var add = new AddThread();
        var dec = new DecThread();
        add.start();
        dec.start();
        add.join();
        dec.join();
        System.out.println(Counter.count);
    }
}

class Counter {
    public static final Object lock1 = new Object();
    public static final Object lock2 = new Object();
    public static int count = 0;
}

class AddThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock1) {
                Counter.count += 1;
            }
        }
    }
}

class DecThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock2) {
                Counter.count -= 1;
            }
        }
    }
}
```

结果并不是0，这是**因为两个线程各自的`synchronized`锁住的*不是同一个对象*！这使得两个线程各自都可以同时获得锁**：因为JVM只保证同一个锁在任意时刻只能被一个线程获取，但两个不同的锁在同一时刻可以被两个线程分别获取。

因此，使用`synchronized`的时候，获取到的是哪个锁非常重要。锁对象如果不对，代码逻辑就不对。

我们再看一个例子：

```
public class Main {
    public static void main(String[] args) throws Exception {
        var ts = new Thread[] { new AddStudentThread(), new DecStudentThread(), new AddTeacherThread(), new DecTeacherThread() };
        for (var t : ts) {
            t.start();
        }
        for (var t : ts) {
            t.join();
        }
        System.out.println(Counter.studentCount);
        System.out.println(Counter.teacherCount);
    }
}

class Counter {
    public static final Object lock = new Object();
    public static int studentCount = 0;
    public static int teacherCount = 0;
}

class AddStudentThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {
                Counter.studentCount += 1;
            }
        }
    }
}

class DecStudentThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {
                Counter.studentCount -= 1;
            }
        }
    }
}

class AddTeacherThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {
                Counter.teacherCount += 1;
            }
        }
    }
}

class DecTeacherThread extends Thread {
    public void run() {
        for (int i=0; i<10000; i++) {
            synchronized(Counter.lock) {
                Counter.teacherCount -= 1;
            }
        }
    }
}
```

**上述代码的4个线程对两个共享变量分别进行读写操作，但是使用的锁都是`Counter.lock`这一个对象，这就造成了原本可以并发执行的`Counter.studentCount += 1`和`Counter.teacherCount += 1`，现在无法并发执行了，执行效率大大降低。实际上，需要同步的线程可以分成两组：`AddStudentThread`和`DecStudentThread`，`AddTeacherThread`和`DecTeacherThread`，组之间不存在竞争，因此，应该使用两个不同的锁，即：**

`AddStudentThread`和`DecStudentThread`使用`lockStudent`锁：

```
synchronized(Counter.lockStudent) {
    ...
}
```

`AddTeacherThread`和`DecTeacherThread`使用`lockTeacher`锁：

```
synchronized(Counter.lockTeacher) {
    ...
}
```

这样才能最大化地提高执行效率。

###### 不需要synchronized的操作

JVM规范定义了几种原子操作：

- 基本类型（`long`和`double`除外）赋值，例如：`int n = m`；
- 引用类型赋值，例如：`List<String> list = anotherList`。

`long`和`double`是64位数据，JVM没有明确规定64位赋值操作是不是一个原子操作，不过在x64平台的JVM是把`long`和`double`的赋值作为原子操作实现的。

单条原子操作的语句不需要同步。例如：

```
public void set(int m) {
    synchronized(lock) {
        this.value = m;
    }
}
```

就不需要同步。

对引用也是类似。例如：

```
public void set(String s) {
    this.value = s;
}
```

上述赋值语句并不需要同步。

但是，如果是多行赋值语句，就必须保证是同步操作，例如：

```
class Point {
    int x;
    int y;
    public void set(int x, int y) {
        synchronized(this) {
            this.x = x;
            this.y = y;
        }
    }
}
```

 多线程连续读写多个变量时，同步的目的是为了保证程序逻辑正确！

不但写需要同步，读也需要同步：

```
class Point {
    int x;
    int y;

    public void set(int x, int y) {
        synchronized(this) {
            this.x = x;
            this.y = y;
        }
    }

    public int[] get() {
        int[] copy = new int[2];
        copy[0] = x;
        copy[1] = y;
    }
}
```

假定当前坐标是`(100, 200)`，那么当设置新坐标为`(110, 220)`时，上述未同步的多线程读到的值可能有：

- (100, 200)：x，y更新前；
- (110, 200)：x更新后，y更新前；
- (110, 220)：x，y更新后。

如果读取到`(110, 200)`，即读到了更新后的x，更新前的y，那么可能会造成程序的逻辑错误，无法保证读取的多个变量状态保持一致。

有些时候，通过一些巧妙的转换，可以把非原子操作变为原子操作。例如，上述代码如果改造成：

```
class Point {
    int[] ps;
    public void set(int x, int y) {
        int[] ps = new int[] { x, y };
        this.ps = ps;
    }
}
```

就不再需要写同步，因为`this.ps = ps`是引用赋值的原子操作。而语句：

```
int[] ps = new int[] { x, y };
```

这里的`ps`是方法内部定义的局部变量，每个线程都会有各自的局部变量，互不影响，并且互不可见，并不需要同步。

不过要注意，读方法在复制`int[]`数组的过程中仍然需要同步。

###### 不可变对象无需同步

如果多线程读写的是一个不可变对象，那么无需同步，因为不会修改对象的状态：

```
class Data {
    List<String> names;
    void set(String[] names) {
        this.names = List.of(names);
    }
    List<String> get() {
        return this.names;
    }
}
```

注意到`set()`方法内部创建了一个不可变`List`，这个`List`包含的对象也是不可变对象`String`，因此，整个`List<String>`对象都是不可变的，因此读写均无需同步。

分析变量是否能被多线程访问时，首先要理清概念，多线程同时执行的是方法。对于下面这个例子：

```
class Status {
    List<String> names;
    int x;
    int y;
    void set(String[] names, int n) {
        List<String> ns = List.of(names);
        this.names = ns;
        int step = n * 10;
        this.x += step;
        this.y += step;
    }
    StatusRecord get() {
        return new StatusRecord(this.names, this.x, this.y);
    }
}
```

如果有A、B两个线程，同时执行是指：

- 可能同时执行set()；
- 可能同时执行get()；
- 可能A执行set()，同时B执行get()。

类的成员变量`names`、`x`、`y`显然能被多线程同时读写，但局部变量（包括方法参数）如果没有“逃逸”，那么只有当前线程可见。局部变量`step`仅在`set()`方法内部使用，因此每个线程同时执行set时都有一份独立的step存储在线程的栈上，互不影响，但是局部变量`ns`虽然每个线程也各有一份，但后续赋值后对其他线程就变成可见了。对`set()`方法同步时，如果要最小化`synchronized`代码块，可以改写如下：

```
void set(String[] names, int n) {
    // 局部变量其他线程不可见:
    List<String> ns = List.of(names);
    int step = n * 10;
    synchronized(this) {
        this.names = ns;
        this.x += step;
        this.y += step;
    }
}
```

因此，深入理解多线程还需理解变量在栈上的存储方式，基本类型和引用类型的存储方式也不同。

#### 同步方法

**我们知道Java程序依靠`synchronized`对线程进行同步，使用`synchronized`的时候，锁住的是哪个对象非常重要。**

==**让线程自己选择锁对象往往会使得代码逻辑混乱，也不利于封装。更好的方法是把`synchronized`逻辑封装起来。**==例如，我们编写一个计数器如下：

```
public class Counter {
    private int count = 0;

    public void add(int n) {
        synchronized(this) {
            count += n;
        }
    }

    public void dec(int n) {
        synchronized(this) {
            count -= n;
        }
    }

    public int get() {
        return count;
    }
}
```

这样一来，线程调用`add()`、`dec()`方法时，它不必关心同步逻辑，因为`synchronized`代码块在`add()`、`dec()`方法内部。并且，我们注意到，`synchronized`锁住的对象是`this`，即当前实例，这又使得创建多个`Counter`实例的时候，它们之间互不影响，可以并发执行：

```
var c1 = Counter();
var c2 = Counter();

// 对c1进行操作的线程:
new Thread(() -> {
    c1.add();
}).start();
new Thread(() -> {
    c1.dec();
}).start();

// 对c2进行操作的线程:
new Thread(() -> {
    c2.add();
}).start();
new Thread(() -> {
    c2.dec();
}).start();
```

现在，对于`Counter`类，多线程可以正确调用。

如果一个类被设计为允许多线程正确访问，我们就说这个类就是“线程安全”的（thread-safe），上面的`Counter`类就是线程安全的。Java标准库的`java.lang.StringBuffer`也是线程安全的。

还有一些不变类，例如`String`，`Integer`，`LocalDate`，它们的所有成员变量都是`final`，多线程同时访问时只能读不能写，这些不变类也是线程安全的。

最后，类似`Math`这些只提供静态方法，没有成员变量的类，也是线程安全的。

除了上述几种少数情况，大部分类，例如`ArrayList`，都是非线程安全的类，我们不能在多线程中修改它们。但是，如果所有线程都只读取，不写入，那么`ArrayList`是可以安全地在线程间共享的。

 没有特殊说明时，一个类默认是非线程安全的。

我们再观察`Counter`的代码：

```
public class Counter {
    public void add(int n) {
        synchronized(this) {
            count += n;
        }
    }
    ...
}
```

当我们锁住的是`this`实例时，实际上可以用`synchronized`修饰这个方法。下面两种写法是等价的：

```
public void add(int n) {
    synchronized(this) { // 锁住this
        count += n;
    } // 解锁
}
public synchronized void add(int n) { // 锁住this
    count += n;
} // 解锁
```

**==因此，用`synchronized`修饰的方法就是同步方法，它表示整个方法都必须用`this`实例加锁。==**

我们再思考一下，如果对一个静态方法添加`synchronized`修饰符，它锁住的是哪个对象？

```
public synchronized static void test(int n) {
    ...
}
```

对于`static`方法，是没有`this`实例的，因为`static`方法是针对类而不是实例。但是我们注意到任何一个类都有一个由JVM自动创建的`Class`实例，因此，对`static`方法添加`synchronized`，锁住的是该类的`Class`实例。上述`synchronized static`方法实际上相当于：

```
public class Counter {
    public static void test(int n) {
        synchronized(Counter.class) {
            ...
        }
    }
}
```

我们再考察`Counter`的`get()`方法：

```
public class Counter {
    private int count;

    public int get() {
        return count;
    }
    ...
}
```

它没有同步，因为读一个`int`变量不需要同步。

然而，如果我们把代码稍微改一下，返回一个包含两个`int`的对象：

```
public class Counter {
    private int first;
    private int last;

    public Pair get() {
        Pair p = new Pair();
        p.first = first;
        p.last = last;
        return p;
    }
    ...
}
```

就必须要同步了。

#### 死锁

**==Java的线程锁是可重入的锁。==**

什么是可重入的锁？我们还是来看例子：

```
public class Counter {
    private int count = 0;

    public synchronized void add(int n) {
        if (n < 0) {
            dec(-n);
        } else {
            count += n;
        }
    }

    public synchronized void dec(int n) {
        count += n;
    }
}
```

==**观察`synchronized`修饰的`add()`方法，一旦线程执行到`add()`方法内部，说明它已经获取了当前实例的`this`锁。如果传入的`n < 0`，将在`add()`方法内部调用`dec()`方法。由于`dec()`方法也需要获取`this`锁，现在问题来了：**==

==**对同一个线程，能否在获取到锁以后继续获取同一个锁？**==

==**答案是肯定的。JVM允许同一个线程重复获取同一个锁，这种能被同一个线程反复获取的锁，就叫做可重入锁。**==

由于Java的线程锁是可重入锁，所以，获取锁的时候，不但要判断是否是第一次获取，还要记录这是第几次获取。每获取一次锁，记录+1，每退出`synchronized`块，记录-1，减到0的时候，才会真正释放锁。

一个线程可以获取一个锁后，再继续获取另一个锁。例如：

```
public void add(int m) {
    synchronized(lockA) { // 获得lockA的锁
        this.value += m;
        synchronized(lockB) { // 获得lockB的锁
            this.another += m;
        } // 释放lockB的锁
    } // 释放lockA的锁
}

public void dec(int m) {
    synchronized(lockB) { // 获得lockB的锁
        this.another -= m;
        synchronized(lockA) { // 获得lockA的锁
            this.value -= m;
        } // 释放lockA的锁
    } // 释放lockB的锁
}
```

在获取多个锁的时候，不同线程获取多个不同对象的锁可能导致死锁。对于上述代码，线程1和线程2如果分别执行`add()`和`dec()`方法时：

- 线程1：进入`add()`，获得`lockA`；
- 线程2：进入`dec()`，获得`lockB`。

随后：

- 线程1：准备获得`lockB`，失败，等待中；
- 线程2：准备获得`lockA`，失败，等待中。

此时，两个线程各自持有不同的锁，然后各自试图获取对方手里的锁，造成了双方无限等待下去，这就是死锁。

死锁发生后，没有任何机制能解除死锁，只能强制结束JVM进程。

因此，在编写多线程应用时，要特别注意防止死锁。因为死锁一旦形成，就只能强制结束进程。

那么我们应该如何避免死锁呢？答案是：线程获取锁的顺序要一致。即严格按照先获取`lockA`，再获取`lockB`的顺序，改写`dec()`方法如下：

```
public void dec(int m) {
    synchronized(lockA) { // 获得lockA的锁
        this.value -= m;
        synchronized(lockB) { // 获得lockB的锁
            this.another -= m;
        } // 释放lockB的锁
    } // 释放lockA的锁
}
```

#### wait和notify

在Java程序中，`synchronized`解决了多线程竞争的问题。例如，对于一个任务管理器，多个线程同时往队列中添加任务，可以用`synchronized`加锁：

```
class TaskQueue {
    Queue<String> queue = new LinkedList<>();

    public synchronized void addTask(String s) {
        this.queue.add(s);
    }
}
```

但是`synchronized`并没有解决多线程协调的问题。

仍然以上面的`TaskQueue`为例，我们再编写一个`getTask()`方法取出队列的第一个任务：

```
class TaskQueue {
    Queue<String> queue = new LinkedList<>();

    public synchronized void addTask(String s) {
        this.queue.add(s);
    }

    public synchronized String getTask() {
        while (queue.isEmpty()) {
        }
        return queue.remove();
    }
}
```

==上述代码看上去没有问题：**`getTask()`内部先判断队列是否为空，如果为空，就循环等待，直到另一个线程往队列中放入了一个任务，`while()`循环退出，就可以返回队列的元素了。**==

==**但实际上`while()`循环永远不会退出。因为线程在执行`while()`循环时，已经在`getTask()`入口获取了`this`锁，其他线程根本无法调用`addTask()`，因为`addTask()`执行条件也是获取`this`锁。**==

==**因此，执行上述代码，线程会在`getTask()`中因为死循环而100%占用CPU资源。**==

如果深入思考一下，我们想要的执行效果是：

- 线程1可以调用`addTask()`不断往队列中添加任务；
- 线程2可以调用`getTask()`从队列中获取任务。如果队列为空，则`getTask()`应该等待，直到队列中至少有一个任务时再返回。

**==因此，多线程协调运行的原则就是：当条件不满足时，线程进入等待状态；当条件满足时，线程被唤醒，继续执行任务。==**

对于上述`TaskQueue`，我们先改造`getTask()`方法，在条件不满足时，线程进入等待状态：

```
public synchronized String getTask() {
    while (queue.isEmpty()) {
        this.wait();
    }
    return queue.remove();
}
```

**当一个线程执行到`getTask()`方法内部的`while`循环时，它必定已经获取到了`this`锁，此时，线程执行`while`条件判断，如果条件成立（队列为空），线程将执行`this.wait()`，进入等待状态。**

这里的关键是：`wait()`方法必须在当前获取的锁对象上调用，这里获取的是`this`锁，因此调用`this.wait()`。

调用`wait()`方法后，线程进入等待状态，`wait()`方法不会返回，直到将来某个时刻，线程从等待状态被其他线程唤醒后，`wait()`方法才会返回，然后，继续执行下一条语句。

有些仔细的童鞋会指出：即使线程在`getTask()`内部等待，其他线程如果拿不到`this`锁，照样无法执行`addTask()`，肿么办？

这个问题的关键就在于**==`wait()`方法的执行机制非常复杂。首先，它不是一个普通的Java方法，而是定义在`Object`类的一个`native`方法，也就是由JVM的C代码实现的。其次，必须在`synchronized`块中才能调用`wait()`方法，因为`wait()`方法调用时，会*释放*线程获得的锁，`wait()`方法返回后，线程又会重新试图获得锁。==**

因此，**==只能在锁对象上调用`wait()`方法==**。因为在`getTask()`中，我们获得了`this`锁，因此，只能在`this`对象上调用`wait()`方法：

```
public synchronized String getTask() {
    while (queue.isEmpty()) {
        // 释放this锁:
        this.wait();
        // 重新获取this锁
    }
    return queue.remove();
}
```

**==当一个线程在`this.wait()`等待时，它就会释放`this`锁，从而使得其他线程能够在`addTask()`方法获得`this`锁。==**

现在我们面临第二个问题：**==如何让等待的线程被重新唤醒，然后从`wait()`方法返回？答案是在相同的锁对象上调用`notify()`方法。我们修改`addTask()`如下：==**

```
public synchronized void addTask(String s) {
    this.queue.add(s);
    this.notify(); // 唤醒在this锁等待的线程
}
```

注意到在往队列中添加了任务后，线程立刻对`this`锁对象调用`notify()`方法，这个方法会唤醒一个正在`this`锁等待的线程（就是在`getTask()`中位于`this.wait()`的线程），从而使得等待线程从`this.wait()`方法返回。

我们来看一个完整的例子：

```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        var q = new TaskQueue();
        var ts = new ArrayList<Thread>();
        for (int i=0; i<5; i++) {
            var t = new Thread() {
                public void run() {
                    // 执行task:
                    while (true) {
                        try {
                            String s = q.getTask();
                            System.out.println("execute task: " + s);
                        } catch (InterruptedException e) {
                            return;
                        }
                    }
                }
            };
            t.start();
            ts.add(t);
        }
        var add = new Thread(() -> {
            for (int i=0; i<10; i++) {
                // 放入task:
                String s = "t-" + Math.random();
                System.out.println("add task: " + s);
                q.addTask(s);
                try { Thread.sleep(100); } catch(InterruptedException e) {}
            }
        });
        add.start();
        add.join();
        Thread.sleep(100);
        for (var t : ts) {
            t.interrupt();
        }
    }
}

class TaskQueue {
    Queue<String> queue = new LinkedList<>();

    public synchronized void addTask(String s) {
        this.queue.add(s);
        this.notifyAll();
    }

    public synchronized String getTask() throws InterruptedException {
        while (queue.isEmpty()) {
            this.wait();
        }
        return queue.remove();
    }
}
```

这个例子中，我们重点关注`addTask()`方法，内部调用了`this.notifyAll()`而不是`this.notify()`，使用`notifyAll()`将唤醒所有当前正在`this`锁等待的线程，而`notify()`只会唤醒其中一个（具体哪个依赖操作系统，有一定的随机性）。这是因为可能有多个线程正在`getTask()`方法内部的`wait()`中等待，使用`notifyAll()`将一次性全部唤醒。通常来说，`notifyAll()`更安全。有些时候，如果我们的代码逻辑考虑不周，用`notify()`会导致只唤醒了一个线程，而其他线程可能永远等待下去醒不过来了。

但是，注意到`wait()`方法返回时需要*重新*获得`this`锁。假设当前有3个线程被唤醒，唤醒后，首先要等待执行`addTask()`的线程结束此方法后，才能释放`this`锁，随后，这3个线程中只能有一个获取到`this`锁，剩下两个将继续等待。

再注意到我们在`while()`循环中调用`wait()`，而不是`if`语句：

```
public synchronized String getTask() throws InterruptedException {
    if (queue.isEmpty()) {
        this.wait();
    }
    return queue.remove();
}
```

这种写法实际上是错误的，因为线程被唤醒时，需要再次获取`this`锁。多个线程被唤醒后，只有一个线程能获取`this`锁，此刻，该线程执行`queue.remove()`可以获取到队列的元素，然而，剩下的线程如果获取`this`锁后执行`queue.remove()`，此刻队列可能已经没有任何元素了，所以，要始终在`while`循环中`wait()`，并且每次被唤醒后拿到`this`锁就必须再次判断：

```
while (queue.isEmpty()) {
    this.wait();
}
```

所以，正确编写多线程代码是非常困难的，需要仔细考虑的条件非常多，任何一个地方考虑不周，都会导致多线程运行时不正常。

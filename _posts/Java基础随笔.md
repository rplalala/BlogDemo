---
title: Java基础随笔
tags:
  - Java基础
  - 随笔
categories: 随笔
sticky: 1
abbrlink: f96a58f3
date: 2022-01-02 19:00:36
updated: 2022-01-02 20:29:55
---

{% note info modern %}
本文只是记录在我学习完 Java基础阶段后，在今后的学习中遇到遗忘的知识，再次记录加深记忆的过程，并不是学习 Java基础 的教程 。
{% endnote %}

# 成员变量、局部变量、类变量
1. **成员变量：** 定义在类里方法外，没被static修饰的变量。一定要有默认值
   
2. **局部变量：** 定义在方法、构造方法、代码块里的变量，随着局部的结束而被销毁。可以没有默认值
   
3. **类变量：** 定义在类里方法外，被static修饰的变量。一定要有默认值

# 数组与字符串的长度
1. array.length 读取数组的长度;

2. str.length() 读取字符串的长度；

# 程序的健壮性
**最好不要用可能为NULL值的变量去引用方法**

    1."admin".equals(userName)
    2. userName.equals("admin") 
    效果一样，但前者更安全

# break可以直接结束标识处的循环
**例：达到if条件后,break直接结束外层for循环，而不是只结束里层for循环**

```java
    end:for(int i = 0;i<5;i++){
        for (int j = 0;j<6;j++){
            if((i+j) == 8){
                break end;
            }
        }
    }
```
# 同一个模块中导入工具类的方法
```java
import java.包名.类名;
例如：
    import java.util.*;
    import java.io.*;
```

# 方法的返回值
1. **若一个方法有返回值，则方法的最外层必须要有一个return（只有在最外层return了，才相当于告诉编译器这个方法我return了）。**
   
2. **不能只在代码块（for、while、if……）中直接return，而不在方法最外层return（会出现编译错误）**

```java
(1).引入中间变量
    public Integer test(){
        Integer x = 0;
        if(true){
            x = 1;
        }
        return x;
    }
```
```java
(2).在代码块中直接return，再在方法最外层return
    public Integer test(){
        if(true){
            return 1;
        }
        return null;
    }
```
* 由于在代码块中已经return过返回值了，则方法最外层的return不会执行，只是向编译器声明我return过了。
  
* 此时方法返回值仍为 1 ，最外层的return只是起到防止编译出错的作用

# java新建数组
1. 与C语言不同,java新建数组时，数组的长度可以直接传变量
   ```java
        Scanner scan = new Scanner(System.in);
        int index = scan.nextInt();
        String[] s = new String[index];

   ```
2. 可以直接 return 给定数值的数组
   ```java
        // int[] nums = new int[]{1,2,3}; 
        // 这样可以做到直接return自己给定值的数组
        public int[] test(){
            return new int[]{1,2,3};
            // return {1,2,3} 这样是错的，编译错误
        }

   ```

# 条件运算符… ? … : …
**常用来取代if…else…**

    例：
        score >= 60 ? "通过" : "不通过"
        若 score >= 60 为 true，则返回"通过"，否则返回"不通过"

# JavaBean
**Java中封装类的一种规范**
1. 所有属性为private
2. 提供默认构造方法
3. 提供getter和setter
4. 实现serializable接口

# 集合
集合不能存放基本数据类型，只能存放对象的引用，例如包装类

# 常量池
* **包装类** 和 **String** 的比较用 equals 不用 == ，但是调用常量池的话，就算用 == 二者也会相等。
  
* 在 -128 - 127 范围内，Integer存在于常量池中，此时是将Integer变量指向常量池中的值。若在常量池的范围外，则是在内存中新建一个Integer变量。

```java
  Integer a = 3;   
  Integer b = 3;   
  Integer c = 500;   
  Integer d = 500;   
  a == b;(true) 
  c == d;(false)

  String s1 = "123";
  String s2 = "123";
  s1 == s2;(true)

```
* 包装类(Double、Float除外)和 String 具有常量池。例如：

```java
    String s = "123";
    Integer i = 123;

```
其中 s 和 i 都是在常量池中储存和拿取的

* 但如果用new对象的办法构建时，无法调用常量池，直接会在内存中开辟新空间。例如：

```java
    String s = new String("123");
    Integer i = new Integer(123);

```
* 只要使用new方法，便需要创建新的对象

# switch
在switch的case中声明过一次变量后，之后的case不用再次声明
```java
  case 1:Integer a = 2;break;   
  case 2:Integer a = 3;break;   
  (false)      
  
  case 1:Integer a = 2;break;   
  case 2: a = 3;break; 
  (true)

```

# ArrayList 和 Vector
* Vector是线程同步的，在同一时间内只允许一个线程访问，安全但效率低；
  
* ArrayList是线程不同步的，在同一时间内允许多个线程访问，不安全但效率高

# 链表、数组内存的区别
* 链表的内存空间不连续，所以不能根据索引直接得到结点位置
  
* 数组的内存空间是连续的，可以直接根据索引得到元素，时间复杂度为O(1)

# String 和 StringBuilder
## **String是一种特殊的引用数据类型，它具有值不可变性**
* **String本身是不可改变的，它只可被赋值一次**。每一次内容被更改，实际上就是产生了一个新的字符串对象，因此对String的多次修改会浪费大量空间。

* StringBuilder则不同，每次都是修改自身，不会产生新的对象。
  
* 例如 s1 = s2，并不是将s1的值改为s2，而是在内存中新开辟一个空间给s1，将s2的值赋予这个新空间。而之前s1的内容仍然存在于内存中，占据着空间。
  
* 对String的所有操作都是新开辟一个内存来接受的，若想直接在原字符串上修改而不开辟新空间，可以用StringBuilder。

* **因此对字符串进行大量操作时，应使用StringBuilder提高效率，减少空间消耗。
例如：频繁的字符串拼接用+连接效率低，建议用StringBuilder**


## **String的常量池**
* 若不通过new对象的方式，而是直接赋予字符串一个值，则会引用了常量池里的字符串。若常量池不存在该字符串，则在常量池中创建该字符串。
  
* 若是用new 对象的方法构建字符串，则会在内存中开辟一个新空间存放这个对象。
```java
    String s1 = "123"； //常量池中不存在"123",则在常量池中构建"123"
    String s2 = "123"； //常量池中存在"123"，直接从常量池中拿字符串
    String s3 = new String("123")； //给s3对象新开辟一个内存
    (s1 == s2)
    (s1 != s3)
    
```

## **String的拼接与常量池**
* 直接看如下例子
```java
    String str1 = "str";        //放进常量池
    String str2 = "ing";        //放进常量池
    String str3 = "str" + "ing";//从常量池中取出并拼接，再把"string"放进常量池
    String str4 = str1 + str2;  //在堆上创建的新的对象     
    String str5 = "string";     //从常量池中取出"string"

    System.out.println(str3 == str5);//true
    System.out.println(str4 == str5);//false

```
* **问:** String s = new String("abc");这句话创建了几个对象？
* **答：** 1-2个对象
    * 若常量池中不存在"abc" ，则先在常量池中创建"abc"，再在堆内存中创建值为"abc"的字符串对象s。共创建了2个对象
    * 若常量池中存在"abc"，则直接从常量池中拿"abc"，再在堆内存中创建值为"abc"的字符串对象s。共创建了1个对象

# 基本数据类型
1. **所占字节**（1个字节是8位）

| 类型 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; | 字节 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; | 位数 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; |
| ----------------------------------------- | ----------------------------------------- | ----------------------------------------- |
| byte                                      | 1字节                                     | 8位                                       |
| short                                     | 2字节                                     | 16位                                      |
| int                                       | 4字节                                     | 32位                                      |
| long                                      | 8字节                                     | 64位                                      |
| float                                     | 4字节                                     | 32位                                      |
| double                                    | 8字节                                     | 64位                                      |
| char                                      | 2字节                                     | 16位                                      |
| boolean                                   | 1字节                                     | 8位                                       |

2. **取值范围**
* int
   * 基本类型：int
   * 包装类：java.lang.Integer
   * 最小值：Integer.MIN_VALUE= -2147483648 （-2的31次方）
   * 最大值：Integer.MAX_VALUE= 2147483647  （2的31次方-1）
   * 最大位数：10位数
* short
   * 基本类型：short
   * 包装类：java.lang.Short
   * 最小值：Short.MIN_VALUE=-32768 （-2的15此方）
   * 最大值：Short.MAX_VALUE=32767 （2的15次方-1）
   * 最大位数：5位数
* long
   * 基本类型：long
   * 包装类：java.lang.Long
   * 最小值：Long.MIN_VALUE=-9223372036854775808 （-2的63次方）
   * 最大值：Long.MAX_VALUE=9223372036854775807 （2的63次方-1）
   * 最大位数：19位数

```java
注意：long最特殊，要在数字后面以L后缀结尾才能作为长整数long
例如：
   long l = 10000000000000  //(错误)，未加L，10000000000000在赋值前就默认为整型int报错了
   long l = 10000000000000L //(正确)，加了L，10000000000000被认为是长整型，不报错
```
* float
   * 基本类型：float
   * 包装类：java.lang.Float
   * 最小值：Float.MIN_VALUE=1.4E-45 （2的-149次方）
   * 最大值：Float.MAX_VALUE=3.4028235E38  （2的128次方-1）
   * 最大位数：小数点后6位
* double
   * 基本类型：double
   * 包装类：java.lang.Double
   * 最小值：Double.MIN_VALUE=4.9E-324 （2的-1074次方）
   * 最大值：Double.MAX_VALUE=1.7976931348623157E308  （2的1024次方-1）
   * 最大位数：小数点后16位

# HashMap底层、红黑二叉树
1. **HashMap实现原理**
   * 底层：数组+单向链表+红黑二叉树
   * 先通过对HashCode进行运算，将键值存到数组中的对应下标。
   * 若该下标已存在元素（即产生哈希冲突），则插入到以该下标元素为头结点的链表尾部
   * 当链表的长度超过8时，才会将链表转换成红黑树，加快索引的速度，提高性能。
2. **红黑二叉树**
   * 一种特殊的平衡二叉搜索树，防止二叉搜索树过长，使时间复杂度不能保持对数级

# 冒泡排序和选择排序（以下代码都以升序为例）
1. **冒泡排序**

```java
/* 冒泡排序：满足条件则将两个值交换，每次循环完末尾是当前最大值 */
public void Bubblesort(){
    int[] a = new int[]{5, 8, 3, 1, 4, 2, 7, 9, 6};
    int size = a.length;
    // 最多循环size-1次即可，最后size-1个确定了第一个也就确定了
    for(int i = 0; i < size - 1; i++) {
        boolean flag = true;//若遍历过程中一次交换都没有出现过，则代表数组此时已经有序，可以提前退出循环
        //循环size-1-i次即可，因为每次经过大循环必定能找出一个最大值放到末尾，不用再进行比较
        for(int j = 0; j < size - 1 - i; j++) {
            if(a[j + 1] < a[j]) {
                flag = false;
                int t = a[j];
                a[j] = a[j + 1];
                a[j + 1] = t;
            }
        } 
        if(flag) {
            break;
        }
    } 
    // 循环遍历输出排序好的数组
    for(int x : a){
        System.out.println(x)
    }
}
```
2. **选择排序**

```java
/* 选择排序：找出最小的元素放到首位*/
public void selectSort(){
    int[] a = new int[]{5, 8, 3, 1, 4, 2, 7, 9, 6};
    int size = a.length;
    for(int i = 0; i < size - 1; i++) {
        int minIndex = i; //先默认最小值为当前元素
        for(int j = i + 1; j < size; j++) {
            if(a[j] < a[minIndex]) {
                minIndex = j; //若后续有元素更小，则更新最小值
            }
        }
        //若最小值就为当前元素，则不变；若不为，则交换位置
        if(minIndex != i){
            int t = a[i];
            a[i] = a[minIndex];
            a[minIndex] = t;
        }
    } 
    // 循环遍历输出排序好的数组
    for(int x : a){
        System.out.println(x)
    } 
}
```

# && 和 || 的判断规则和应用方法
1. **&&的运算规则：**
    * 当符号左边表达式为false时，&&将直接返回false不再判断符号右边表达式的结果。
    * 当符号左边表达式为true时，将继续判断符号右边表达式。
2. **||的运算规则：**
    * 当符号左边表达式为true时，||将直接返回true不在判断符号右边的表达式结果。
    * 当符号左边表达式为false时，将继续判断符号右边表达式。
3. **应用方法**
    * 因此为了提高效率，我们应该注意判断的先后顺序
    * 例如：判断回文素数时，由于符合回文数的数更少，我们应该先判断回文数，省去不必要的判断
    * 即：判断回文数 && 判断素数

# Java7新特性，数字下划线
    例如：100_000_000 相当于 现实生活中的100,000,000
    便于我们计数，在编译中会自动省去下划线

# hashcode 和 equals
* 如果equals一样的话，hashcode也必须一样(反之不成立);若equals重写了，hashcode最好也要重写。
   
* 没有重写过的equals比较的是地址（相当于==），重写过的比较的是值
   
* **特殊hashCode**
  * **String：** 只要字符串内容相同，其hashCode也相同。这是因为字符串重写的equals就是比较的字符串内容，内容一样equals就一样，hashcode也一样
  
  * **Integer：** 返回的hashCode是其对象内包含的整数的值。
  
* **hashcode和地址**
  * HashCode不等于内存地址，它只是跟内存地址相关。
  * HashCode相同，内存地址也可能不同。但地址相同，hashcode相同
  * hashCode返回的是经过处理后的对象内存地址结构，每个对象的内存地址都不同，所以hashCode也不同。

# boolean containsKey(Object key)的判断原则
1. **见containsKey的源码可得：**

```java
if (e.hash == hash && ((k = e.key) == key || (key != null && key.equals(k))))
```
判断一个key是否存在于map中时，先判断二者的 **hash** ，再判断二者的 **地址** 或 **equals方法** 是否相等

2. **所以可得如下结论：**

* 传入的类需重写equals和hashcode方法才能对key进行值判断。例如String和Integer重写了equals和hashcode方法，所以只要二者值相同即可认为是同一个key。
  
* 若自定义类没有重写equals和hashcode方法，比较的是地址。则必须是同一个对象（地址相同）才是同一个key。例如链表节点，树节点等

# 浮点数取整
* Math.ceil(double n); //对浮点数向上取整
* Math.floor(double n); //对浮点数向下取整
* Math.round(double n); //对浮点数四舍五入取整，不够准确。例如：-11.5 -> -11

**利用大数类进行准确的四舍五入**
```
//对10.7进行四舍五入取整
new BigDecimal("10.7").setScale(0,BigDecimal.ROUND_HALF_UP).intValue();
```

# 二十四、static
## 静态变量
```
1. 静态变量被所有的对象所共享，当且仅当在类初次加载时会被初始化。

2. 非静态变量是各个对象所独立的，在创建对象的时候被初始化，各个对象互不影响。

3. 局部变量不能设为静态，只有成员变量可以

```
## 静态方法
```
1. 静态方法内不能使用非静态方法、非静态变量

2. 静态方法内不能出现this和super

3. 非静态方法内可以使用静态资源、非静态资源、super和this

4. 静态方法和变量可以通过类名直接调用，也可以通过实例化对象调用。

5. 非静态方法和变量必须通过实例化对象调用

```
## 静态内部类
```
  1. 如果一个类要被声明为static，只有一种情况 —— 静态内部类。如果将外部类声明为static，编译都不会过。

  2. 静态内部类可以 声明 非静态/静态的方法和变量，而非静态内部类只能声明非静态的方法和变量。

  3. 静态内部类可以 访问 外部类的静态变量和静态方法,但是对于外部类的非静态变量和方法不能访问。

  4. 只有静态变量才需要考虑对象共享问题，静态内部类并不会导致其实例化的每个对象共享（除非该内部类存在静态变量，那么该变量会被共享）。

```

### 静态内部类在外部类的main方法中实例化
```java
1.同一个外部类实例化
public class Main{
    static class People {
    }
    public static void main(String[] args){
        直接（在同一个外部类内）
        People p1 = new People();
        或（在其他外部类内实例化时可以用这个）
        Main.People p2 = new Main.People(); 
    }
}
2.其他外部类实例化
class Hello{
    public static void main(String[] args){
        Main.People p = new Main.People(); 其他外部类调用时可以用这个
    }
}
```

### 非静态内部类在外部类的main方法中实例化
* 基于外部类对象来实例化

```java
public class Main{
    class People {
    }
    public static void main(String[] args){
        People p = new Main().new People();
    }
}
```

# 不能实例化对象的原因
1. 接口 —— interface
2. 抽象类 —— abstract（有抽象方法一定是抽象类，抽象类不一定有抽象方法）
3. 构造方法访问权限为private

# Scanner类 in.next() 和 in.nextLine()
## in.next() 
  * 检测到 有效字符 前的空格/回车会被舍去
  * 检测到 有效字符 后再遇到空格/回车则结束读取，并且该空格/回车作为结束符舍去

```java
输入"   \n  \n  12 34\n"，得到"12"
```

## in.nextLine()
* 直接开始读取，可以读取空格，遇到回车则结束读取，并且该回车会作为结束符舍去

```java
输入"    12 34 \n"，得到"    12 34 "

输入"\n"，得到""
```
    






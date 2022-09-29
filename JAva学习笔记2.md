

# Regular Expressions

## 正则表达式底层实现	

给定一段文本（字符串），请找出所有四个数字连在一起的子串，如：1998 2020 2021

```java
 public static void main(String[] args) {
	String context = "1995年，互联网的蓬勃发展给了Oak机会。业界为了使死板、单调的静态网页能够“灵活”起来，……";
	// \\d 表示一个任意的数字
String regStr = "\\d\\d\\d\\d";

//创建一个模式对象
Pattern pattern = Pattern.compile(regStr);
//创建一个匹配器
Matcher matcher = pattern.matcher(context);
//开始匹配
while(matcher.find()){
	System.out.println("找到：" + matcher.group(0));
}	
 }
```
### 不考虑分组

#### matcher.find()

1. 根据指定的规定，定位满足规则的子字符串（比如1995）
2. 找到后将子字符串开始的索引记录到matcher对象的属性 int[] groups;groups[0] = 0,把该子字符串结束的索引(3)+1的值 记录到 groups[1] = 4
3. 同时记录oldLast 的值为 子字符串的结束的 索引+1的值即4，即下次执行find时，就从4开始匹配

#### matcher.group(0)

1. 根据group[0] = 0 和 group[1] = 4 的记录的位置，从context 开始截取子字符串[0,4) 返回

2. 如果再次执行find方法，仍然按照上面分析来执行

   

### 考虑分组

分组：正则表达式中有小括号代表分组，第一个()代表第一组……：(\d\d)(\d\d)

#### matcher.find()

1. 根据指定的规定，定位满足规则的子字符串（比如(19)(95)）
2. 找到后将子字符串开始的索引记录到matcher对象的属性 int[] groups;
   2.1 groups[0] = 0,把该子字符串结束的索引(3)+1的值 记录到 groups[1] = 4
   2.2 记录第一组()匹配到的字符串: groups[2] = 0, groups[3] = 2
   2.3 记录第二组()匹配到的字符串: groups[4] = 2, groups[5] = 4

#### matcher.group()

group(0) 表示匹配到的整体的子字符串
group(1) 表示匹配到的子字符串的第一个分组
group(2) 表示匹配到的子字符串的第二个分组
注：分组数不能越界
————————————————
原文链接：https://blog.csdn.net/wu6426/article/details/121033430

## 正则表达式语法

### 元字符（Metacharacter)

#### 转义号 \\

在使用正则表达式去检索某些特殊字符的时候，需要用到转义符号\\，否则检索不到结果，甚至会报错。
注：在java的正则表达式中，两个\\代表其它语言中的一个\。
需要用到转义符号的字符：.*+()$/?[]^{}
如找到左括号（：\\(

#### 字符匹配符

![](C:\Users\ThinkPad\iCloudDrive\Java\RegExp元字符1 2021-11-21 at 23.00.03.png)

![image-20211123092707639](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20211123092707639.png)

![image-20211123093123480](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20211123093123480.png)

![image-20211123092920336](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20211123092920336.png)

#### 选择匹配符 |

![img](https://img-blog.csdnimg.cn/adf2c28dacba4347ba318a53b234eecf.png)

#### 限定符 *+？{n} {n,} {n,m}

![在这里插入图片描述](https://img-blog.csdnimg.cn/65f8513c701842e1b4569ab939d193e9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5LiA5Y-q5oeS6bG85YS_,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/0fd33a467a4441dc81312825a7c333cf.png)

#### 定位符^ $ \\\b \\B

![image-20211123093523347](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20211123093523347.png)

#### 分组，捕获，反向引用

![image-20211127005019724](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20211127005019724.png)

##### 捕获分组

![在这里插入图片描述](https://img-blog.csdnimg.cn/0dfc9e80f0cc45d9aa29154ee0e6e96c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5LiA5Y-q5oeS6bG85YS_,size_18,color_FFFFFF,t_70,g_se,x_16)

##### 非捕获分组（特别分组）

![在这里插入图片描述](https://img-blog.csdnimg.cn/c029bb468f584cf88123aae0445cc11f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5LiA5Y-q5oeS6bG85YS_,size_18,color_FFFFFF,t_70,g_se,x_16)

#### Reluctant quantifiers

![在这里插入图片描述](https://img-blog.csdnimg.cn/63465389238b4eda82cf52aa3b57741e.png)

```java
/**
 * @author Wei Liu
 * @version V1.0
 */
public class RegExp12 {
    public static void main(String[] args) {
        //分组，捕获，反向引用 Some examples
        /*examples:
        1.要匹配两个连续的相同的数字： (\\d)\\1
        2.要匹配5个连续的相同的数字： (\\d)\\1{4}
        3.要匹配千位与个位相同，十位与百位相同的数字 5555 5225 1551： (\\d)(\\d)\\2\\1

         */
        String content = "22222unb10031,stu5225,1234,ded11111,ID:12334-111222333";
        //String regExp = "(\\d)\\1";
        //String regExp = "(\\d)\\1{4}";
        //String regExp = "(\\d)(\\d)\\2\\1";

        /*
        * 请在String 中检索商品编号，形式如：12334-111222333这样的号码，
        * 要求，前五个是五位数+ “-”号，然后是一个每三位相同的九位数
        * \\d{5}-(\\d)\\1{2}(\\d)\\2{2}(\\d)\\3{2}
        * */
        String regExp = "\\d{5}-(\\d)\\1{2}(\\d)\\2{2}(\\d)\\3{2}";
        Pattern pattern = Pattern.compile(regExp);
        Matcher matcher = pattern.matcher(content);
        while (matcher.find()){
            System.out.println(matcher.group());
        }
    }
}
```



```java
package RegExp;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * @author Wei Liu
 * @version V1.0
 */
public class RegExp13 {
    public static void main(String[] args) {
        /*
        * Regular Expression instance
        * A classic jieba program
        * "I.....I want...ssssssstu...ddddy....Java!" to "I want to study java"
        * */
        String content = "I.....I want ...ssssssstu...ddddy ....Java!";
        String regExp = "\\.";
        Pattern pattern = Pattern.compile(regExp);
        Matcher matcher = pattern.matcher(content);
        content = matcher.replaceAll("");
        System.out.println(content);

        //分步
/*         regExp = "(.)\\1+";
        pattern = Pattern.compile(regExp);
        matcher = pattern.matcher(content);
        content = matcher.replaceAll("$1");*/

        //链式
        //content = Pattern.compile("(.)\\1+").matcher(content).replaceAll("$1");

        content = content.replaceAll("(.)\\1+", "$1");
        System.out.println(content);
    }
}
```

## String class  Rex

```java
package RegExp;
/*
        * String 类中使用正则表达式
        * 替换功能
        * String class public String replaceAll(String regex, String replacement)
        *
        * example:
        * String content ="2000年5月，JDK1.3、JDK1.4和J2SE1.3相继发布，几周后其获得了Apple公司Mac OS X的工业标准的支持。" +
                "2001年9月24日，J2EE1.3发布。2002年2月26日，J2SE1.4发布。" +
                "自此Java的计算能力有了大幅提升，与J2SE1.3相比，其多了近62%的类和接口。" +
                "在这些新特性当中，还提供了广泛的XML支持、安全套接字（Socket）支持（通过SSL与TLS协议）、全新的I/OAPI、正则表达式、日志与断言。" +
                "2004年9月30日，J2SE1.5发布，成为Java语言发展史上的又一里程碑。" +
                "为了表示该版本的重要性，J2SE 1.5更名为Java SE 5.0（内部版本号1.5.0），代号为“Tiger”，Tiger包含了从1996年发布1.0版本以来的最重大的更新，" +
                "其中包括泛型支持、基本类型的自动装箱、改进的循环、枚举类型、格式化I/O及可变参数。”；
                *
                * 将 content 中的JDK1.3 JDK1.4 统一替换成JDK
                *
        * */
/**
 * @author Wei Liu
 * @version V1.0
 */
public class RegExp14_StringReg {
    public static void main(String[] args) {
        String content ="2000年5月，JDK1.3、JDK1.4和J2SE1.3相继发布，几周后其获得了Apple公司Mac OS X的工业标准的支持。" +
                "2001年9月24日，J2EE1.3发布。2002年2月26日，J2SE1.4发布。" +
                "自此Java的计算能力有了大幅提升，与J2SE1.3相比，其多了近62%的类和接口。" +
                "在这些新特性当中，还提供了广泛的XML支持、安全套接字（Socket）支持（通过SSL与TLS协议）、全新的I/OAPI、正则表达式、日志与断言。" +
                "2004年9月30日，J2SE1.5发布，成为Java语言发展史上的又一里程碑。" +
                "为了表示该版本的重要性，J2SE 1.5更名为Java SE 5.0（内部版本号1.5.0），代号为“Tiger”，Tiger包含了从1996年发布1.0版本以来的最重大的更新，" +
                "其中包括泛型支持、基本类型的自动装箱、改进的循环、枚举类型、格式化I/O及可变参数.";
        content = content.replaceAll("JDK1\\.3|JDK1\\.4"/*"JDK(?:1.3|1.4)"*/, "JDK");
        System.out.println(content);

        /*
        * 判断功能
        * String Class public boolean matches(String regex）{}
        * 要求验证一个手机号，要求必须是以138 139开头
         * */
        String content1 = "13912345678";
         if(content1.matches("13(8|9)\\d{8}")){
             System.out.println("验证成功");
         }else{
             System.out.println("验证失败");
         }

         /*
         * 判断功能
         * String class : public String[] split(String regEx)
         * String content = "hello#fabc-jack12smith~Beijing"
         * Split by # or - or ~ or numbers
          *
         * */
        String content2 = "hello#fabc-jack12smith~Beijing";
        String[] split = content2.split("#|-|~|\\d+");
        for (String s:split
             ) {
            System.out.println(s);
        }

        /*
        * 判断是否为合法电子邮件
        * only one @
        * a-z A-Z 0-9 . _ - before @
        * After @ 域名只能是英文字母，比如sohu.com or tsinhua.org.cn
        *

        * */

        String content3 = "wei.liu@unb.ca";
        //String regExp = "[\\w-.]{3,16}@([a-zA-Z]+\\.)+[a-zA-Z]+";
        String regExp = "^[\\w-.]{3,16}@([a-zA-Z]+\\.)+[a-zA-Z]+$";
        if (content3.matches(regExp)){
            System.out.println("valid Email address");
        }else {
            System.out.println("invalid Email address");
        }

        /*
        *备注
         * String  matches method matches the entire region against the pattern.
         * 看源码
         * String:
         *     public boolean matches(String regex) {
              return Pattern.matches(regex, this);
                }

          Pattern:
          * public static boolean matches(String regex, CharSequence input) {
        Pattern p = Pattern.compile(regex);
        Matcher m = p.matcher(input);
        return m.matches();
    }
    *
     * Attempts to match the entire region against the pattern.
        Matcher：
        public boolean matches() {
            return match(from, ENDANCHOR);
        }
 */
    }
}
```

## Homework

```java
package RegExp;

/**
 * @author Wei Liu
 * @version V1.0
 */
public class RegExp15_Homework1 {
    public static void main(String[] args) {
        /*
         * 判断是否为合法电子邮件
         * only one @
         * a-z A-Z 0-9 . _ - before @
         * After @ 域名只能是英文字母，比如sohu.com or tsinhua.org.cn
         *

         * */

        String content3 = "wei.liu@unb.ca";
        //String regExp = "[\\w-.]{3,16}@([a-zA-Z]+\\.)+[a-zA-Z]+";
        String regExp = "^[\\w-.]{3,16}@([a-zA-Z]+\\.)+[a-zA-Z]+$";
        if (content3.matches(regExp)){
            System.out.println("valid Email address");
        }else {
            System.out.println("invalid Email address");
        }
    }
}

```

```java
package RegExp;
        /*
        * Verify a num whether is possitive num or negative
        * like 123 -345 34 89 -87.9 -0.01 0.45
        * -?\\d+\\.?\\d*
        * */
import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * @author Wei Liu
 * @version V1.0
 */
public class RegExp16_Homework2 {
    public static void main(String[] args) {

        String content = "0.880";
        String regExp = "^[-+]?(([1-9]\\d*)|0)\\.?\\d*$";
        Pattern pattern = Pattern.compile(regExp);
        Matcher matcher = pattern.matcher(content);
        if(matcher.find()){
            System.out.println(content + " is valid");
        }else {
            System.out.println(content + "is invalid");
        }
    }
}
```

```java
package RegExp;
/*
* 对一个url 进行解析 RegExp17_Homework3
* http://www.sohu.com:8080/abc/index.htm
* a) 要求得到协议 http
* b）域名： www.sohu.com
* c) 端口：8080
* d）文件名 index.htm
* 思路： 分组（3组）分别获取对应值
*
* */

import javax.xml.stream.events.EntityReference;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * @author Wei Liu
 * @version V1.0
 * @date 2021/11/23
 */
public class RegExp17_Homework3 {
    public static void main(String[] args) {
        String content = "http://www.sohu.com:8080/abc/index.htm";
        String regExp = "^([A-Za-z]+)://([A-Za-z.]+):(\\d+)[\\w-/]*/([\\w.@#$%]+)$";
        Pattern pattern = Pattern.compile(regExp);
        Matcher matcher = pattern.matcher(content);
        //entire region matches,if match ，we get the specific content by group（x）；
        if (matcher.matches()){
            System.out.println("Entire match :" + matcher.group(0));
            System.out.println("协议: " + matcher.group(1));
            System.out.println("域名: " + matcher.group(2));
            System.out.println("端口号： " + matcher.group(3));
            System.out.println("文件名： " + matcher.group(4));

        }
    }
        }
```

# 类作为成员变量类型

[举例]: D:\Java\IdeaProjects\basic-code\mianXiangDuiXiang-code\src\classAsBianLiangTybe

```java
package classAsBianLiangTybe;

public class HeroMain {
    public static void main(String[] args) {
        Hero hero = new Hero();
        hero.setName("张三丰");
        hero.setAge(98);
        //创建一个武器对象
        Weapon weapon = new Weapon("鸡毛掸子");
        //为英雄配备武器
        hero.setWeapon(weapon);
        hero.attack();
    }
}
```

接口作为成员变量类型

```java
package interfaceAsVariableTybe;

public class Test {
    public static void main(String[] args) {
        Hero hero = new Hero();
        hero.setName("花木兰");
        /*SkillImpl skill = new SkillImpl();*/
        /*hero.setSkill(new SkillImpl());*///匿名对象

        //匿名内部类
        /*Skill skill = new Skill() {
            @Override
            public void skillMethod() {
                System.out.println("piu~piu~");
            }
        }*/
        
        //同时使用匿名内部类和内部对象
        hero.setSkill(new Skill() {
            @Override
            public void skillMethod() {
                System.out.println("piu~pia~piu~");
            }
        });
        hero.attack();
    }
}
```

## 接口作为方法的参数和返回值

```java
package InterfaceAsReturnOfMethod;

import java.util.ArrayList;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        //ArrayList 是List接口的 的实现类
        List<String> list = new ArrayList<>();
        List<String> result = addNames(list);
        System.out.println(result);
        }
    public static List<String> addNames(List<String> list){
        list.add("刘思坦");
        list.add("刘曦冉");
        list.add("刘加一");
        return list;
    }
}
```

## 发红包案例

D:\Java\IdeaProjects\basic-code\mianXiangDuiXiang-code\src\redListPractice1

# 常用类

### System类

| 方法名                                 | 说明                                      |
| -------------------------------------- | ----------------------------------------- |
| public static void exit (int  status)  | 终止当前运行的Java虚拟机,非零表示一场终止 |
| public static long currentTimeMillis() | 返回当前时间(以毫秒为单位)                |

#### 计算程序运行时间

```java
package classSystem;

public class Demo01 {
    public static void main(String[] args) {
        System.out.println(System.currentTimeMillis());
        long start = System.currentTimeMillis();
        for (int i = 0; i <10000 ; i++) {
            System.out.println(i);
        }
        long end = System.currentTimeMillis();
        System.out.println("共耗时" + (end - start) + "ms");
    }
}

```

### Object类

![image-20210106234615703](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210106234615703.png)

#### Object 类的常用方法

| 方法名                                                       | 说明         |
| ------------------------------------------------------------ | ------------ |
| public String toString() {      return getClass().getName() + "@" + Integer.toHexString(hashCode());  }*/ | 转换成字符串 |
| public boolean equals(Object obj) {     return (this == obj); | 哈希值对比   |



### Arrays 类

#### Arrays类的常用方法

![image-20210107123602425](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210107123602425.png)

### 包装类

基本类型包装类(Integer为例)

常用方法:

| 方法名                                   | 说明                                            |
| ---------------------------------------- | ----------------------------------------------- |
| public static Integer valueOf（String s) | 返回一个表示指定的 `int` 值的 `Integer` 实例。  |
| public static Integer valueOf（int i)    | 返回保存指定的 `String` 的值的 `Integer` 对象。 |

常用于基本类型和字符串之间的转换

### String 类

#### 构造方法

![image-20210107134026043](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210107134026043.png)

字符串特点

![image-20210107135224744](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210107135224744.png)

字符串比较

![image-20210107135139980](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210107135139980.png)

```Java
package classString;
/*String 方法的特点*/
public class Text {
    public static void main(String[] args) {
        char[] cha = {'a', 'b', 'c'};
        String s1 = new String(cha);
        String s2 = new String(cha);
        String s3 = "abc";
        String s4 = "abc";
        //比较地址是否相同
        System.out.println(s1 == s2);//false
        System.out.println(s1 == s3);//false
        // String 类final(public final class String),所以在堆内存有一个常量池中.
        System.out.println(s3 == s4);//true
        //比较字符串内容是否相同
        System.out.println(s1.equals(s2));//true
        System.out.println(s1.equals(s3));//true
        System.out.println(s4.equals(s3));//true
    }

```

### StringBuilder类

String 的内存图

![image-20210107164757577](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210107164757577.png)

概述

![image-20210107165210577](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210107165210577.png)

StringBuilder构造方法

![image-20210107165348442](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210107165348442.png)

StringBuilder方法

![image-20210107173730007](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210107173730007.png)

```java
package classStringBuilder;
/*
StringBuilder 构造方法:
* public StringBuilder()构造一个不带任何字符的字符串生成器，其初始容量为 16 个字符。
* public StringBuilder(String str)构造一个字符串生成器，并初始化为指定的字符串内容。该字符串生成器的初始容量为 16 加上字符串参数的长度。
 * */
public class StudyNote {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();
        StringBuilder sb1 = new StringBuilder("World");
        System.out.println(sb);//null
        sb.append("Hello");
        System.out.println(sb);//Hello;
        sb.append(sb1);
        System.out.println(sb);//Hello World
        //链式编程
        sb.append(1).append(2).append(3).append("!");
        System.out.println(sb);
```

String 和StringBuilder 类型相互转换

```java
//String 和StringBuilder 类型相互转换
//StringBuilder --> String
String sb2 =sb.toString();//使用 public String toString() 方法.
String sb3 = "Hello Java";

//S  String --> StringBuilder
StringBuilder sb4 = new StringBuilder(sb3);//通过 public StringBuilder(String s) 构造方法
System.out.println(sb2 + sb4);
```

### DATE

### *SimpleDateFormat*

### *Calendar*

### 实例练习,学生管理系统

# Exception(异常)

![image-20210110220240455](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210110220240455.png)

### 异常处理

#### try...catch...

![image-20210110220922341](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210110220922341.png)





Throwable 类

成员方法

![image-20210110221938057](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210110221938057.png)



throws

### 自定义异常

格式

![image-20210111160010495](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210111160010495.png)

自定义异常类ScoreException

```java
package exceptionStudy;

public class ScoreException extends Exception {
    public ScoreException() {
    }

    public ScoreException(String message) {
        super(message);
    }
}
```

```java
package exceptionStudy;

public class Teacher {
    public void checkScore(int score) throws Exception{
        if(score < 0 || score > 100){
            throw new ScoreException("请输入0~100之间的整数");
        }else{
            System.out.println("分数正常");

        }
    }
}
```

测试

```java
import java.util.Scanner;

public class TeacherTest {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入分数");
        int score = sc.nextInt();
        Teacher t = new Teacher();
        try {
            t.checkScore(score);
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```

并发修改异常

![image-20210112150655624](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210112150655624.png)

```
package exceptionStudy.bingFaXiuGaiException;
```

# 集合进阶

1.1集合类体系结构

![](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210111172019633.png)

###  Collection Class

常用方法

![image-20210111173224876](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210111173224876.png)

note: alt +7 打开能够看到类的所有信息的窗口.

Collection 集合的遍历

Iterator: 迭代器,集合的专用遍历方式

![image-20210111174640958](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210111174640958.png)

```java
package classCollection;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

public class IteratorDemo {
    public static void main(String[] args) {
        //创建集合对象
        Collection<String> c = new ArrayList<>();
        //添加元素
        c.add("Hello");
        c.add("great");
        c.add("Java");
        //Iterator<E> iterator()/返回在此 collection 的元素上进行迭代的迭代器。
        Iterator<String> it = c.iterator();
       // 获取下一个元素 E next()
        String str =it.next();
        //遍历集合
        //boolean hasNext()方法判断是否还有元素
        while (it.hasNext()){
            System.out.println(str);
        }
    }
}
/*
* public Iterator<E> iterator() {
        return new Itr();
    }
    private class Itr implements Iterator<E> {
    ....
    }
```

#### 1.3 List Class

![image-20210111203156623](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210111203156623.png)

#### List Class 的特有方法

![image-20210111203809270](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210111203809270.png)

ListIterator

![image-20210112204216460](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210112204216460.png)

##### 增强for循环

![image-20210113100923472](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210113100923472.png)

```java
package classCollection.classList;

import java.util.ArrayList;
import java.util.Collection;

public class Forcycle {
    public static void main(String[] args) {
        //创建集合对象
        Collection<String> c = new ArrayList<>();
        c.add("Hello");
        c.add("great");
        c.add("Java");
        for (String str : c) {
            System.out.println(str);
        }
    }
}
```

案例:List集合存储学生对象用三种方式遍历

![image-20210113103221505](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210113103221505.png)

```java
package classCollection;

import java.util.ArrayList;
import java.util.List;
import java.util.ListIterator;

public class Practice {
    public static void main(String[] args) {
        //创建学生对象
        Student s = new Student("刘思坦",5);
        Student s1 = new Student("刘曦冉",2);
        Student s2 = new Student("刘加一",1);

        //创建List集合并添加学生对象到集合
        List<Student> sL = new ArrayList<>();
        sL.add(s);
        sL.add(s1);
        sL.add(s2);


        //迭代器方法遍历结合
        ListIterator<Student> listIterator = sL.listIterator();
        while (listIterator.hasNext()){
            Student student = listIterator.next();
            System.out.println("姓名:" + student.getName() + "\t" + "年龄:" + student.getAge() + "岁");
        }

        //普通for循环:带有索引的遍历方式
        System.out.println("==============");
        for (int i = 0; i < sL.size(); i++) {
            Student student = sL.get(i);
            System.out.println("姓名:" + student.getName() + "\t" + "年龄:" + student.getAge() + "岁");
        }

        System.out.println("=========");
        //增强for:最方便的遍历方式
        for (Student st : sL) {
            System.out.println("姓名:" + st.getName() + "\t" + "年龄:" + st.getAge() + "岁");
        }
```

##### 数据结构

栈(先进后出)和队列(先进先出).

常见数据结构

数组(查询快增删慢)和链表(增删快查询慢)

##### LinkedList Class 集合的特有功能

![image-20210113124825843](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210113124825843.png)

#### Set Class

![image-20210113125238572](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210113125238572.png)

##### HashSet Class 的特点

![image-20210113135033878](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210113135033878.png)

```java
package classCollection.setClass;

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

/*
* Set一个不包含重复元素的 collection
* HashSet 此类实现 Set 接口，由哈希表（实际上是一个 HashMap 实例）支持。它不保证 set 的迭代顺序
* */
public class DemoSetClass {
    public static void main(String[] args) {
        Set<String>  set = new HashSet<>();
        set.add("Hello");
        set.add("World");
        set.add("Java");
        set.add("!!!!");

        Iterator<String> it = set.iterator();
        while (it.hasNext()){
            System.out.println(it.next());
        }

        for(String str : set){
            System.out.println(str);
        }
    }
}
```

##### Note: hashCode

![image-20210113132118794](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210113132118794.png)

常见数据结构之哈希表

取余---(==)--->对比哈希值---(==)--->对比内容---(==)--->不存储.

![image-20210113182004447](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210113182004447.png)

##### 案例:HashSet集合存储学生对象并遍历

![image-20210113191734931](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210113191734931.png)

```java
package classCollection.setClass;

import classCollection.Student;

import java.util.HashSet;

public class HashSetDemo {
    public static void main(String[] args) {
        //Create HashSet 集合对象
        HashSet<Student> hashSet = new HashSet();

        // create Student 对象
        Student s1 = new Student("Liu,Sitan",5);
        Student s2 = new Student("Liu,Xiran",2);
        Student s3 = new Student("Liu,Jiayi",1);
        Student s4 = new Student("Liu,Jiayi",1);

        //添加对象到集合
        hashSet.add(s1);
        hashSet.add(s2);
        hashSet.add(s3);
        hashSet.add(s4);

        //遍历集合
        //OverRide equals 和 hashcode 方法
        for(Student s : hashSet){
            System.out.println(s.getName() + "\t" + s.getAge());
        }
    }
}
//输出结果
Liu,Sitan	5
Liu,Jiayi	1
Liu,Xiran	2
```

##### LinkedHashSet Class

LinkedHashSet 的特点

![image-20210113192139700](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210113192139700.png)

##### TreeSet Class

![image-20210113200202731](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210113200202731.png)

```java
package classCollection.setClass;

import classCollection.Student;

import java.util.TreeSet;

public class TreeSetDemo {
    public static void main(String[] args) {
        //创建LinkedHashSet 集合对象
        TreeSet<Student> treeSet = new TreeSet<>();

        // create Student 对象
        Student s3 = new Student("Liu,Jiayi",2);
        Student s2 = new Student("Liu,Xiran",2);
        Student s4 = new Student("Liu,Jiayi",1);
        Student s1 = new Student("Liu,Sitan",5);

        //添加对象到集合
        treeSet.add(s1);
        treeSet.add(s2);
        treeSet.add(s3);
        treeSet.add(s4);

        //遍历结合
        for (Student s : treeSet){
            System.out.println(s.getName() + "\t" + s.getAge());
        }
    }
}
```

```java

```

Note : 自然排序Comparable 接口使用

![image-20210113232031596](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210113232031596.png)

类实现Compareble 接口并Override 方法

```java
public class Student implements Comparable<Student>{
    ....
        @Override
    public int compareTo(Student s) {
//        return 0;//重复
//          return 1;//升序
//        return -1;//降序
//        return this.age - s.age();//升序
//        return s.age - this.age();//降序
        //比较年龄
        int i = this.age - s.getAge();
        i = i==0? this.name.compareTo(s.name) : i;
        return i; 
	}
}
```

##### 比较器排序 Comparator的使用

![image-20210114133853147](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210114133853147.png)

```java
package classCollection.comparatorDemo;
/*
* 存储学生对象并遍历,创建TreeSet集合并使用带参构造方法
* 要求:按照年龄从小到大排序,年龄相同时,按照姓名的字母顺序排列.
* */

import java.util.Comparator;
import java.util.TreeSet;

public class ComaratorInTreeSet {
    public static void main(String[] args) {
        //创建TreeSet 有参对象
        TreeSet<Student> treeSet = new TreeSet<>(new Comparator<Student>() {
            @Override
            public int compare(Student s1, Student s2) {
               // return 0; //Liu,sitan,5
                //return 1;//Liu,sitan,5 Liu,Sitan,5 Liu,xiran,2 Liu,xiran,3 Liu,jiayi,2 Liu,Jiayi,1
             //return -1;//Liu,Jiayi,1  Liu,jiayi,2  Liu,xiran,3  Liu,xiran,2  Liu,Sitan,5  Liu,sitan,5

                //只比较年龄
               // int i = s1.getAge() - s2.getAge();
            //return i;//Liu,Jiayi,1  Liu,xiran,2  Liu,xiran,3  Liu,sitan,5

                //只比较姓名
                /*String str = s1.getName();
                int i = str.compareTo(s2.getName());
                return i*/;//Liu,Jiayi,1    Liu,Sitan,5    Liu,jiayi,2  Liu,sitan,5   Liu,xiran,2

                //先比较age,后比较name
                String str1 = s1.getName();
                String str2 = s2.getName();
                  int i = s1.getAge() - s2.getAge();
                  i = i==0? str1.compareTo(str2) : i;
                  return i;//Liu,Jiayi,1   Liu,jiayi,2    Liu,xiran,2Liu,xiran,3 Liu,Sitan,5    Liu,sitan,5
            }
        });

        //创建学生对象

        Student s1 = new Student("Liu,sitan",5);
        Student s2 = new Student("Liu,Sitan",5);

        Student s3 = new Student("Liu,xiran",2);
        Student s4 = new Student("Liu,xiran",3);

        Student s5 = new Student("Liu,jiayi",2);
        Student s6 = new Student("Liu,Jiayi",1);

        //添加学生对象到TreeSet集合对象
        treeSet.add(s1);
        treeSet.add(s2);
        treeSet.add(s3);
        treeSet.add(s4);
        treeSet.add(s5);
        treeSet.add(s6);

        //增强for循环遍历
        for (Student s : treeSet){
            System.out.println(s.getName() + "," + s.getAge());
        }

    }
}
```

##### 案例练习:

###### 案例一

成绩排序

需求:用TreeSet集合存储多个学生信息(姓名,语文成绩,数学成绩),并遍历集合.

要求:按照总分从高到底出现.

位置: classCollection.PracticeDemo01

###### 案例二

编写一个程序,获取10个1-20之间的随机数,要求随机数不能重复,并在控制台输出.

```java
package classCollection.PracticeDemo02;
/*
* 案例:编写一个程序,获取10个1-20之间的随机数,要求随机数不能重复,并在控制台输出.
* */

import java.util.*;

public class RandomNumbers {
    public static void main(String[] args) {
        //创建对象
        Random r = new Random();
        Set<Integer> setArray = new HashSet<>();

        //添加对象
        while (setArray.size() < 10) {
            Integer num = r.nextInt(20) + 1;
            setArray.add(num);
        }
        //遍历集合
        for(Integer integer : setArray){
            System.out.println(integer);
        }
    }
}
```

### Map(掌握)

​	(1)将键映射到值的对象。一个映射不能包含重复的键；每个键最多只能映射到一个值。 

#### 	(2)Map和Collection的区别?

​		A:Map 存储的是键值对形式的元素，键唯一，值可以重复。夫妻对
​		B:Collection 存储的是单独出现的元素，子接口Set元素唯一，子接口List元素可重复。光棍

#### 	(3)Map接口功能概述

##### 		A:添加功能

| [V](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Map.html) put([K](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Map.html) key, [V](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Map.html) value) |      |
| ------------------------------------------------------------ | ---- |
|                                                              |      |

##### 		B:删除功能

| void clear()                                                 |      |
| ------------------------------------------------------------ | ---- |
| [V](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Map.html) remove([Object](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Object.html) key) |      |

##### 		C:判断功能

| boolean containsKey([Object](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Object.html) key) |      |
| ------------------------------------------------------------ | ---- |
| boolean containsValue([Object](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Object.html) value) |      |
| boolean isEmpty()                                            |      |

##### 		D:获取功能

| [V](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Map.html) get([Object](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Object.html) key) | Returns the value to which the specified key is mapped, or `null` if this map contains no mapping for the key. |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Set](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Set.html)<K> keySet() | returns a [`Set`](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Set.html) view of the keys contained in this map |
| [Collection](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Collection.html)<V>values() |                                                              |
| Set<Map.Entry<K,V>> entrySet()                               | Returns a [`Set`](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Set.html) view of the mappings contained in this map. |

##### 		E:长度功能

| int size() |      |
| ---------- | ---- |
|            |      |



#### 	(4)Map集合的遍历

##### 		A:键找值

​	a:获取所有键的集合
​			b:遍历键的集合,得到每一个键
​			c:根据键到集合中去找值

##### B:键值对对象找键和值

​	a:获取所有的键值对对象的集合
​			b:遍历键值对对象的集合，获取每一个键值对对象
​			c:根据键值对对象去获取键和值

```java
package classCollection.mapStudy;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class MapDemo01 {
    public static void main(String[] args) {
        //create the Map Object
        Map<String, String>  map= new HashMap<String, String>();

        //put elements to map
        map.put("Obama","mightier");
        map.put("Cliton","Xilali");
        map.put("习近平","彭丽媛");

        int mapSize = map.size();
        boolean hasKey = map.containsKey("Obama");
        boolean hasValue = map.containsValue("彭丽媛");
        boolean isEmpty = map.isEmpty();
        //String remove1 = map.remove("Obama");
        //boolean remove2 = map.remove("Cliton", "Xilali");
        //boolean remove21 = map.remove("Cliton", "Xilal");
        //String replace3 = map.replace("习近平", "彭ly");
        //boolean replace4 = map.replace("习近平", "彭丽媛", "ply");
        //Collection<String> values = map.values();
       

        //Iterate over the collection
        //键集合Set<K> keySet() ---> Iterate by for ---> the value of the key is mapped V get(Object key)
        Set<String> keySet = map.keySet();
        for (String str : keySet){
            String value = map.get(str);
            System.out.println(str + "\t" + value);
        }

        //  Set<Map.Entry<K,V>> entrySet()--->Iterate over the collection
        Set<Map.Entry<String, String>> entrySet = map.entrySet();
        for (Map.Entry<String, String> me : entrySet){
            System.out.println(me.getKey() + "\n" + me.getValue());
        }
    }
}
```

```Java 

		Map<String,String> hm = new HashMap<String,String>();
		
		hm.put("it002","hello");
		hm.put("it003","world");
		hm.put("it001","java");
		
		//方式1 键找值
		Set<String> set = hm.keySet();
		for(String key : set) {
			String value = hm.get(key);
			System.out.println(key+"---"+value);
		}
		
		//方式2 键值对对象找键和值
		Set<Map.Entry<String,String>> set2 = hm.entrySet();
		for(Map.Entry<String,String> me : set2) {
			String key = me.getKey();
			String value = me.getValue();
			System.out.println(key+"---"+value);
		}

```

#### (5)HashMap集合的练习

​	A:HashMap<String,String>
​	B:HashMap<Integer,String>
​	C:HashMap<String,Student>
​	D:HashMap<Student,String>

```java
package classCollection.mapStudy.hashMapPractice;

import java.util.HashMap;
import java.util.Set;

/*
* 需求:
*   创建一个Has和Map集合,Key是学生对象Student ,value 工作地点.存储多个建制对元素,并遍历.
* 思路:
*   1.定义学生类
*   2.创建HashMap 集合对象
*   3.把学生添加到集合
*   4.遍历结合
*   5.在学生类中重写两个方法
*       hashCode();
*       equals();
*
* */
public class HashMapTest {
    public static void main(String[] args) {
        HashMap<Student, String> studentStringHashMap = new HashMap<>();

        Student s1 = new Student("刘思坦", 5);
        Student s2 = new Student("刘曦冉", 2);
        Student s3 = new Student("刘加一",1);
        Student s4 = new Student("刘加一",1);

        studentStringHashMap.put(s1, "Fredericton");
        studentStringHashMap.put(s2, "Fredericton");
        studentStringHashMap.put(s3, "Fredericton");
        studentStringHashMap.put(s4, "Moncton");

        Set<Student> studentSet = studentStringHashMap.keySet();
        for(Student student : studentSet){
            String value = studentStringHashMap.get(student);
            System.out.println(student.getName() +"\t" + student.getAge() + "\t" + value);
        }
    }
}
```

(6)TreeMap集合的练习		
	A:TreeMap<String,String>
	B:TreeMap<Student,String>

```java
package classCollection.mapStudy;

import java.util.Comparator;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;

public class TreeMapDemo01 {
    public static void main(String[] args) {
        Student s1 = new Student("刘思坦",5);
        Student s2 = new Student("刘曦冉",2);
        Student s3 = new Student("刘加一",5);
        Student s4 = new Student("刘加一",1);


        Map<Student, Integer> treeMap = new TreeMap(new Comparator<Student>() {
            @Override
            public int compare(Student s1, Student s2) {
                int i = s1.getiD() - s2.getiD();
                String name1 = s1.getName();
                String name2 = s2.getName();
                i = i == 0 ? name1.compareTo(name2) : i;
                return i;
            }
        });
        treeMap.put(s1, 232);
        treeMap.put(s2, 435);
        treeMap.put(s3, 112);
        treeMap.put(s4, 444);

        //Student student = treeMap.get();
        Set<Student> studentsSet = treeMap.keySet();
        for (Student student : studentsSet){
            int iD = student.getiD();
            String name = student.getName();
            int value = treeMap.get(student);
            System.out.println( name + iD + value);
        }
    }
}
```

(7)案例
	A:统计一个字符串中每个字符出现的次数

```java
package classCollection.mapStudy;

import java.util.HashMap;
import java.util.Scanner;
import java.util.Set;
import java.util.TreeMap;

/*
*
* 键盘输入一个有字母和数字组成的字符串,统计字符串中每个字母数字出现的次数,并依次输出.
* 分析:使用TreeMap or HashMap? <Character, Integer>(null null) 字符作为key, 计数作为value
* 键盘输入 Scanner 方法, String --->将字符串转为字符数组 char[] charArray = toCharArray() --->遍历字符数组
* 取出每一个字符 增强for --->将取出的字符char 在map集合中找key, 如果返回null,则放入 (key:char, value:1 );若果返回不是null
* 则++ ---->遍历map集合.
*
* */
public class Practice1 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        HashMap<Character, Integer> characterIntegerTreeMap = new HashMap<Character, Integer>();
        System.out.println("请输入由数字和字母组成的字符串:");
        String str = sc.nextLine();

        char[] charArray = str.toCharArray();
        for (Character ch : charArray){
            Integer i = characterIntegerTreeMap.get(ch);
            if(i == null){
                characterIntegerTreeMap.put(ch, 1);
            }else{
                i++;
                characterIntegerTreeMap.put(ch, i);
            }
        }
        Set<Character> charactersSet = characterIntegerTreeMap.keySet();
        for(Character ch : charactersSet){
            Integer i = characterIntegerTreeMap.get(ch);
            System.out.println(ch +"\t" + i);
        }
    }
}
```

​	B:集合的嵌套遍历
​		a:HashMap嵌套HashMap
​		b:HashMap嵌套ArrayList

```java
package classCollection.mapStudy.hashMapPractice;
//创建一个HashmMap集合,存储三个键值对元素,每一个键值对的键是String, value is ArrayList,每一个ArrayList元素是String,并遍历.

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Set;

public class HashMapAndArrayList {
    public static void main(String[] args) {
        ArrayList<String> stringArrayList1 = new ArrayList<>();
        ArrayList<String> stringArrayList2 = new ArrayList<>();
        ArrayList<String> stringArrayList3 = new ArrayList<>();
        stringArrayList1.add("北京");
        stringArrayList2.add("上海");
        stringArrayList3.add("南京");

        HashMap<String, ArrayList<String>> stringArrayListHashMap = new HashMap<>();
        stringArrayListHashMap.put("刘思坦",stringArrayList1);
        stringArrayListHashMap.put("刘曦冉",stringArrayList2);
        stringArrayListHashMap.put("刘加一",stringArrayList3);

        Set<String> stringSet = stringArrayListHashMap.keySet();
        for(String str : stringSet){
            ArrayList<String> arrayList = stringArrayListHashMap.get(str);
            for (int i = 0; i < arrayList.size(); i++) {
                String str1 = arrayList.get(i);
                System.out.println(str + "\t" + str1);
            }
        }
    }
}
```

​		c:ArrayList嵌套HashMap

```java
package classCollection.mapStudy.hashMapPractice;

import java.util.*;

/*
* 需求: 创建一个ArrayList Collection ,存储3个元素,每一个元素都是HashMap,每一个HashMap的Key 和value 都是String,并遍历.
* */
public class ArrayListAndHashMap {
    public static void main(String[] args) {
        //ArrayList
        ArrayList<HashMap<String, String>> hashMapArrayList = new ArrayList<>();

        //HashMap 并添加元素
        HashMap<String,String> hashMap1 = new HashMap<>();
        HashMap<String,String> hashMap2 = new HashMap<>();
        HashMap<String,String> hashMap3 = new HashMap<>();
        hashMap1.put("Bill", "Hillary");
        hashMap2.put("Barack", "Michelle");
        hashMap3.put("Biden", "Jill");

        //将hashMap 作为元素存入ArrayList
        hashMapArrayList.add(hashMap1);
        hashMapArrayList.add(hashMap2);
        hashMapArrayList.add(hashMap3);

        //遍历ArrayList,取出每一HashMap
        for(HashMap<String, String> hashMap : hashMapArrayList){
            //HashMap<String, String> hash= hashMap;
            Set<String> set = hashMap.keySet();
            for(String str : set){
                String value = hashMap.get(str);
                System.out.println(str + "\t" + value);
            }
        }
    }
}
```

​		d:多层嵌套

### 2:Collections(理解)	

​	(1)是针对集合进行操作的工具类

​	(2)面试题：Collection和Collections的区别
​		A:Collection 是单列集合的顶层接口，有两个子接口List和Set
​		B:Collections 是针对集合进行操作的工具类，可以对集合进行排序和查找等

​	(3)常见的几个小方法：
​		A:public static <T> void sort(List<T> list)
​		B:public static <T> int binarySearch(List<?> list,T key)
​		C:public static <T> T max(Collection<?> coll)
​		D:public static void reverse(List<?> list)
​		E:public static void shuffle(List<?> list)

​	(4)案例
​		A:ArrayList集合存储自定义对象的排序

```
package classCollection.classCollections;
/*
* 需求:ArrayList存储学生对象,使用Collections 对ArrayList 进行排序
* 要求: 按照年龄从小到大,年龄相同时候,按照姓名字母顺序排列
* 思路:ArrayList 集合
* Collection sort
* */

import arrayStudy.Array;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class CollectionTest {
    public static void main(String[] args) {
        Student s1 = new Student("liusitan",5);
        Student s2 = new Student("liuxiran",2);
        Student s3 = new Student("liujiayi",1);
        Student s4 = new Student("liuyi",2);

        ArrayList<Student> studentArrayList = new ArrayList<Student>();
        studentArrayList.add(s1);
        studentArrayList.add(s2);
        studentArrayList.add(s3);
        studentArrayList.add(s4);

        Collections.sort(studentArrayList, new Comparator<Student>() {
            @Override
            public int compare(Student s1, Student s2) {
                int i = s1.getAge() - s2.getAge();
                i = i == 0 ? s1.getName().compareTo(s2.getName()) : i;
                return i;
            }
        });

        for(Student student : studentArrayList){
            System.out.println(student.getName() + "\t" + student.getAge());
        }
    }
}
```

​		B:模拟斗地主洗牌,发牌和看牌

```java
package classCollection;

import java.util.ArrayList;
import java.util.Collections;

/*
* 需求: 通过程序实现斗地主过程中的洗牌,发牌和看牌
*
* 思路:
*   1.创建牌盒用ArrayList实现
*       花色 colors
*       2~~K,A;
*        大王,小王
*   2.牌盒装牌
*       pokerBox
*   2.2 洗牌
*   3.发牌,遍历集合,给三个玩家发牌
*   4.看牌,创建一个看牌方法 三个玩家分别遍历自己的牌
*
* */
public class PokerDemo01 {
    public static void main(String[] args) {
        //1.创建牌盒用ArrayList实现
        String[] colors = new String[]{"♥", "♠", "♦","♣"};
        String[] numbers = new String[]{"2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A"};

        //创建牌盒
        ArrayList<String> pokerBox = new ArrayList<>();
        //装牌
        for(String color : colors){
            for(String number : numbers){
                pokerBox.add(color + number);
            }
        }
        pokerBox.add("大王");
        pokerBox.add("小王");
        //洗牌
        Collections.shuffle(pokerBox);

        //发牌
        ArrayList<String> player1 = new ArrayList<>();
        ArrayList<String> player2 = new ArrayList<>();
        ArrayList<String> player3 = new ArrayList<>();
        ArrayList<String> last3Pokers = new ArrayList<>();

        for (int i = 0; i < pokerBox.size(); i++) {
            String card = pokerBox.get(i);
            if(i < 3){
                last3Pokers.add(card);
            }else if(i % 3 == 0){
                player1.add(card);
            }else if(i % 3 == 1){
                player2.add(card);
            }else if(i % 3 == 2){
                player3.add(card);
            }
        }

        //调用看牌方法遍历每个人的牌
        lookPokers("play1", player1);
        lookPokers("play2", player2);
        lookPokers("play3", player3);
        lookPokers("底牌", last3Pokers);

    }
    //看牌方法
    public static void lookPokers(String name, ArrayList<String> pai){
        System.out.println(name + "的牌:");
        for(String str : pai){
            System.out.println(str);
        }
    }
}
```

​		C:模拟斗地主洗牌和发牌并对牌进行排序

```java
package classCollection;

import java.lang.module.FindException;
import java.util.*;

/*
* 需求: 模拟斗地主洗牌和发牌并对牌进行排序
* 思路:
* 使用HashMap集合创建pokerBox牌盒 key 存储index (0-53),value 存储存牌,
* 制作54张牌
* 创建ArrayList集合索引indexArray,使用Collections.shuffle 方法对index随机排序
* 创建TreeMap 玩家
* 发牌 key 为indexArray 内的索引,value 为key在pokerBox找对应的value
* 定义看牌方法遍历TreeMap集合玩家
* 调取看牌方法
* 
* */
public class PokerDemo02 {
    public static void main(String[] args) {
        //创建牌盒
        HashMap<Integer, String> pokersBox = new HashMap<Integer, String>();

        //制牌
        String[] colors = new String[]{"♥", "♠", "♦","♣"};
        String[] numbers = new String[]{"2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A"};

        //存入牌盒
        int key = 0;
        for (String color : colors) {
            for(String number : numbers){
                pokersBox.put(key,color + number);
                key++;
            }
        }
        pokersBox.put(key, "大王");
        key++;
        pokersBox.put(key, "小王");
        key++;
       // System.out.println(key);

        //洗牌
        ArrayList<Integer> indexArray = new ArrayList<>();
        for (int i = 0; i < key; i++) {
            indexArray.add(i);
        }
        //System.out.println(indexArray.size());
        Collections.shuffle(indexArray);

        //Treemap 玩家
        TreeMap<Integer, String> player1 = new TreeMap<>();
        TreeMap<Integer, String> player2 = new TreeMap<>();
        TreeMap<Integer, String> player3 = new TreeMap<>();
        TreeMap<Integer, String> last3Cards = new TreeMap<>();

        //发牌
        for (int i = 0; i < indexArray.size() ; i++) {
            int index = indexArray.get(i);
            String value = pokersBox.get(index);
            if(i < 3){
                last3Cards.put(index,value );
            }else if(i % 3 == 0){
                player1.put(index, value);
            }else if(i % 3 == 1) {
                player2.put(index, value);
            }else if(i % 3 == 2) {
                player3.put(index, value);
            }
        }

        //调取看牌方法看牌
        lookPokers("player1", player1);
        lookPokers("player2", player2);
        lookPokers("player3", player3);
        lookPokers("底牌", last3Cards);
    }
    //定义看牌方法,遍历treeMap集合
    public static void lookPokers(String name, TreeMap<Integer, String> player){
        System.out.println(name + "的牌是:");
        int count = 0;
        Set<Integer> integerSet = player.keySet();
        for(Integer integer : integerSet){
            count++;
            System.out.println(count + player.get(integer));
        }
    }
}
```

## 集合特点总结

集合

1. Collection(单列集合)
   1. List(有序,可重复)
      1. 底层数据接口是数组,查询快,增删慢
      2. 线程不安全,效率高
   2. Vector
      1. 底层数据结构是数组,查询快,增删慢
      2. ​		线程安全,效率低
   3. LinkedList
      1.   		底层数据结构是数组,查询慢,增删快
      2. ​		   线程不安全,效率高

Set（无序,唯一)

1.  HashSet
   1. 底层数据结构是哈希表.

   2. 哈希表依赖两个方法:hashCode()和equals()

      1. 执行顺序:

         首先判断hashCode()值是否相同

         是:继续执行equals(),看骑返回值

         true:说明元素重复,不添加

         false:添加

         最终:自动生成hashCode()和equals即可.

1. LinkedHashSet
   1. 底层数据结构有链表和哈希表组成.
      1. 由链表保证有序
      2. 由哈希表保证唯一
2. TreeSet
   1. 底层数据结构是红黑数.(一种自平衡的二叉树)
   2. 排序方式
      1. 自然排序(元素具备比较性):让元素所属类实现Comparable接口
      2. 比较器排序(集合具备比较性):让集合接受一个Comparator的实现类对象

Map(双列集合)

1. HashMap（同HashSet）
   1. LinkedHashMap(同LinkedHashSet)
2. Hashtable
3. TreeMap





# 泛型

![](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210114183332200.png)

### 泛型类

![image-20210114183954931](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210114183954931.png)

```java
package genericStudy;

public class Generic<T> {
    private T t;

    public Generic() {
    }

    public Generic(T t) {
        this.t = t;
    }

    public T getT() {
        return t;
    }
    
    public void setT(T t) {
        this.t = t;
    }
}
```

```java
package genericStudy;
/*
*泛型类
* */
import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

public class GenericClassDemo01 {
    public static void main(String[] args) {
        //Collection c = new ArrayList();
        Collection<String> c = new ArrayList<>();
        c.add("刘斯坦");
        //c.add(100);
        c.add("100");
        //Iterator iterator = c.iterator();
        Iterator<String> iterator = c.iterator();
        while (iterator.hasNext()){
           // System.out.println( (String) iterator.next());
            /*刘斯坦
            /Exception in thread "main" java.lang.ClassCastException:
            java.lang.Integer cannot be cast to java.lang.String*/
            String str = iterator.next();
            System.out.println(str);
        }
    }
}
```

### 泛型方法

![image-20210118193521837](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210118193521837.png)

```java
package genericStudy;
/*
* 泛型方法
* public <T> 返回值类型 方法名(T t)
* */

public class GenericMethod {
    public <T> void show(T t){
        System.out.println(t);
    }
}
```

```java
//泛型方法
System.out.println("------泛型方法示例------");
GenericMethod genericMethod = new GenericMethod();
genericMethod.show("刘加一");
genericMethod.show(true);
genericMethod.show(100);
genericMethod.show(100.94);
```

### 泛型接口

![image-20210118205857732](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210118205857732.png)

```java
package genericStudy;

public interface GenericInterface<T> {
    public void show(T t);

}
```

```
//泛型接口
System.out.println("------泛型接口示例------")

GenericInterface<String> genericInterface = (String s) -> {
    System.out.println(s);
};

genericInterface.show("刘思坦");
```

### 类型通配符

![image-20210118220310151](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210118220310151.png)

```java
package genericStudy.tongPeiFu;

import java.util.ArrayList;
import java.util.List;

/*
* 类型通配符
* */
public class Demo01 {
    public static void main(String[] args) {
        //类型通配符<?>
        List<?> list1 = new ArrayList<Object>();
        List<?> list2 = new ArrayList<Number>();
        List<?> list3 = new ArrayList<Integer>();

        //类型通配符上限<? extends 类型>
        //List<? extends Number> list4 = new ArrayList<Object>();
        List<? extends Number> list5 = new ArrayList<Number>();
        List<? extends Number> list6 = new ArrayList<>(Integer);

        //类型通配符下限<? super 类型>
        List<? super Number>  list7 = new ArrayList<Object>();
        List<? super Number>  list8 = new ArrayList<Number>();
        //List<? super Number>  list9 = new ArrayList<Integer>();

    }
```

#### extends通配符

```java
public void someMethod(List<? extends Number> list) {
    Number n = list.get(0);
    list.add(n); // ERROR
}
```

允许传入List<Number>，List<Integer>，List<Double>...
允许调用方法获取Number类型
不允许调用方法传入Number类型（null除外）
<T extends Number>
定义泛型时可以通过extends限定T必须是Number或Number的子类

#### super通配符

```java
void someMethod(List<? super Integer> list) {
list.add(123);
Integer n = list.get(0); // ERROR
}
```

允许传入List<Integer>，List<Number>，List<Object>
允许调用方法传入Integer类型
不允许调用方法获取Integer类型（Object除外）

<T super Integer>
定义泛型时可以通过extends限定T必须是Integer或Integer的超类

#### extends和super通配符的区别

<? extends T>允许调用方法获取T的引用
<? super T>允许调用方法传入T的引用
无限定通配符<?>
只能获取Object引用
只能传入null
可以用<T>消除<?>

### 可变参数

![image-20210119123037245](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210119123037245.png)

```java
package genericStudy;

public class KeBianCanShu {
    public static void main(String[] args) {
        int sum =sumMethot(10,20,40,50,100);
        System.out.println(sum);
    }

    public static int sumMethot(int... t){
        int sum = 0;
        for (int i : t){
            sum += i;
        }
        return sum;
    }
}
```

可变参数的使用

![image-20210119221824682](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210119221824682.png)

```java
 //可变参数的使用
        //Arrays 类中的方法 public static <T> List<T> asList(T... a)
        List<String> list1 = Arrays.asList("Hello","World", "Java" );
//        list1.add("!!!");//UnsupportedOperationException
//        list1.remove(0);//UnsupportedOperationException
        list1.set(2, "JAVA");
        for (String str : list1){
            System.out.println(str);
        }
        //List interface 中的方法 static <E> List<E> of(E... elements)
        List<? extends Number> numbers = List.of(100, 200.2, 1000000, 3.1415);
        for (Number n : numbers){
            System.out.println(n);
        }

        // Set interface 中的方法 static <E> Set<E> of(E... elements)
        Set<String> stringSet = Set.of("Hello", "world", "Java", "100");
        for (String str : stringSet){
            System.out.println(str);
```

# IO流

## 异常

![image-20210309095017665](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210309095017665.png)

### 异常处理方案

•try…catch…finally

•throws

### 自定义异常

l自定义异常

•继承自Exception

•继承自RuntimeException

**异常注意事项**

l子类重写父类方法时，子类的方法必须抛出相同的异常或父类异常的子类。(父亲坏了,儿子不能比父亲更坏)

l如果父类抛出了多个异常,子类重写父类时,只能抛出相同的异常或者是他的子集,子类不能抛出父类没有的异常

l如果被重写的方法没有异常抛出,那么子类的方法绝对不可以抛出异常,如果子类方法内有异常发生,那么子类只能try,不能throws

| File       |      |
| ---------- | ---- |
| 字节流     |      |
| 字符流     |      |
| 特殊操作流 |      |

## File

(1)IO流操作中大部分都是对文件的操作，所以Java就提供了File类供我们来操作文件

## 	(2)构造方法

A:File file = new File("e:\\demo\\a.txt");
​		B:File file = new File("e:\\demo","a.txt");
​		C:File file = new File("e:\\demo");
​		  File file2 = new File(file,"a.txt");

## 	(3)File类的功能(自己补齐)

A:创建功能
​		B:删除功能
​		C:重命名功能
​		D:判断功能
​		E:获取功能
​		F:高级获取功能
​		G:过滤器功能

## 	(4)案例：

​		A:输出指定目录下指定后缀名的文件名称
​			a:先获取所有的，在遍历的时候判断，再输出
​			b:先判断，再获取，最后直接遍历输出即可
​		B:批量修改文件名称

## 1:递归(理解)

​	(1)方法定义中调用方法本身的现象
​		举例：老和尚给小和尚讲故事，我们学编程
​	(2)递归的注意事项；
​		A:要有出口，否则就是死递归
​		B:次数不能过多，否则内存溢出
​		C:构造方法不能递归使用

### 	(3)递归的案例：

​		A:递归求阶乘
​		B:兔子问题
​		C:递归输出指定目录下所有指定后缀名的文件绝对路径
​		D:递归删除带内容的目录(小心使用)

1:递归(理解)
	(1)方法定义中调用方法本身的现象
		举例：老和尚给小和尚讲故事，我们学编程
	(2)递归的注意事项；
		A:要有出口，否则就是死递归
		B:次数不能过多，否则内存溢出
		C:构造方法不能递归使用
	(3)递归的案例：
		A:递归求阶乘
		B:兔子问题
		C:递归输出指定目录下所有指定后缀名的文件绝对路径
		D:递归删除带内容的目录(小心使用)

## 2:IO流(掌握)

​	(1)IO用于在设备间进行数据传输的操作	
​	(2)分类：
​		A:流向
​			输入流	读取数据
​			输出流	写出数据
​		B:数据类型
​			字节流	
​					字节输入流
​					字节输出流
​			字符流
​					字符输入流
​					字符输出流
​		注意：
​			a:如果我们没有明确说明按照什么分，默认按照数据类型分。
​			b:除非文件用windows自带的记事本打开我们能够读懂，才采用字符流，否则建议使用字节流。
​	(3)FileOutputStream写出数据
​		A:操作步骤
​			a:创建字节输出流对象
​			b:调用write()方法
​			c:释放资源
​			

		B:代码体现：
			FileOutputStream fos = new FileOutputStream("fos.txt");
			
			fos.write("hello".getBytes());
			
			fos.close();
			
		C:要注意的问题?
			a:创建字节输出流对象做了几件事情?
			b:为什么要close()?
			c:如何实现数据的换行?
			d:如何实现数据的追加写入?
	(4)FileInputStream读取数据
		A:操作步骤
			a:创建字节输入流对象
			b:调用read()方法
			c:释放资源
			
		B:代码体现：
			FileInputStream fis = new FileInputStream("fos.txt");
			
			//方式1
			int by = 0;
			while((by=fis.read())!=-1) {
				System.out.print((char)by);
			}
			
			//方式2
			byte[] bys = new byte[1024];
			int len = 0;
			while((len=fis.read(bys))!=-1) {
				System.out.print(new String(bys,0,len));
			}
			
			fis.close();
	(5)案例：2种实现
		A:复制文本文件
		B:复制图片
		C:复制视频
	(6)字节缓冲区流
		A:BufferedOutputStream
		B:BufferedInputStream
	(7)案例：4种实现
		A:复制文本文件
		B:复制图片
		C:复制视频

3:自学字符流
	IO流分类
		字节流：
			InputStream
				FileInputStream
				BufferedInputStream
			OutputStream
				FileOutputStream
				BufferedOutputStream
		字符流：
		Reader
			FileReader
			BufferedReader
		Writer
			FileWriter
			BufferedWriter

## 1:字符流(掌握)

​	(1)字节流操作中文数据不是特别的方便，所以就出现了转换流。
​	   转换流的作用就是把字节流转换字符流来使用。
​	(2)转换流其实是一个字符流
​		字符流 = 字节流 + 编码表
​	(3)编码表
​		A:就是由字符和对应的数值组成的一张表
​		B:常见的编码表
​			ASCII
​			ISO-8859-1
​			GB2312
​			GBK
​			GB18030
​			UTF-8
​		C:字符串中的编码问题
​			编码
​				String -- byte[]
​			解码
​				byte[] -- String
​	(4)IO流中的编码问题
​		A:OutputStreamWriter
​			OutputStreamWriter(OutputStream os):默认编码，GBK
​			OutputStreamWriter(OutputStream os,String charsetName):指定编码。
​		B:InputStreamReader
​			InputStreamReader(InputStream is):默认编码，GBK
​			InputStreamReader(InputStream is,String charsetName):指定编码
​		C:编码问题其实很简单
​			编码只要一致即可
​	(5)字符流
​		Reader
​			|--InputStreamReader
​				|--FileReader
​			|--BufferedReader
​		Writer
​			|--OutputStreamWriter
​				|--FileWriter
​			|--BufferedWriter
​	(6)复制文本文件(5种方式)

2:IO流小结(掌握)
	IO流
		|--字节流
			|--字节输入流
				InputStream
					int read():一次读取一个字节
					int read(byte[] bys):一次读取一个字节数组
				
```java
				|--FileInputStream
				|--BufferedInputStream
		|--字节输出流
			OutputStream
				void write(int by):一次写一个字节
				void write(byte[] bys,int index,int len):一次写一个字节数组的一部分
				
				|--FileOutputStream
				|--BufferedOutputStream
	|--字符流
		|--字符输入流
			Reader
				int read():一次读取一个字符
				int read(char[] chs):一次读取一个字符数组
				
				|--InputStreamReader
					|--FileReader
				|--BufferedReader
					String readLine():一次读取一个字符串
		|--字符输出流
			Writer
				void write(int ch):一次写一个字符
				void write(char[] chs,int index,int len):一次写一个字符数组的一部分
				
				|--OutputStreamWriter
					|--FileWriter
				|--BufferedWriter
					void newLine():写一个换行符
					
					void write(String line):一次写一个字符串
```

## 3:案例(理解 练习一遍)

​	A:复制文本文件 5种方式(掌握)
​	B:复制图片(二进制流数据) 4种方式(掌握)
​	C:把集合中的数据存储到文本文件
​	D:把文本文件中的数据读取到集合并遍历集合
​	E:复制单级文件夹
​	F:复制单级文件夹中指定的文件并修改名称
​		回顾一下批量修改名称
​	G:复制多级文件夹
​	H:键盘录入学生信息按照总分从高到低存储到文本文件
​	I:把某个文件中的字符串排序后输出到另一个文本文件中
​	J:用Reader模拟BufferedReader的特有功能
​	K:模拟LineNumberReader的特有功能
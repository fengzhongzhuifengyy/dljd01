# day04

## 今日内容

* Date类
* DateFormat类
* Calendar类
* JDK8时间相关类
* 正则表达式
* Arrays类
* 常见算法
* lambda表达式

## 教学目标

- [ ] 能够使用日期类输出当前日期

- [ ] 能够使用将日期格式化为字符串的方法

- [ ] 能够使用将字符串转换成日期的方法

- [ ] 了解JDK8时间相关类

- [ ] 能读懂正则表达式

- [ ] 能使用Arrays类中的方法

- [ ] 能够说出8种基本类型对应的包装类名称

- [ ] 能够说出自动装箱、自动拆箱的概念

- [ ] 能够将字符串转换为对应的基本类型

- [ ] 能够将基本类型转换为对应的字符串

- [ ] 会使用lambda表达式

  

# 第一章 Date类

心得：

​	如果以后我们仅仅要展示时间，那么可以用Date和SimpleDateFormat（格式化）

​	如果我们要拿着两个时间进行计算，用Date和SimpleDateFormat（解析）

​	如果我们要修改时间，可以使用Calendar简化开发。

## 1.1 Date概述

Date其实是JDK提供的Javabean类，用来描述时间的。

如果我们自己去编写一个描述时间的类，那么按照习惯，肯定要思考有什么属性，空参构造，带参构造还有对应的get和set方法，toString方法。

```java
class Date{
    //属性
    //毫秒值（从计算机的时间原点开始，到指定时间的毫秒值）
	long fastTime;
    
    //如果是空参，我们要给fastTime赋一个默认值
    //默认使用系统当前时间会更方便。
    public Date(){
        fastTime = System.currentTimeMillis();
    }
    
    //传递的是0，表示Date对象，记录的就是时间原点的那个时间。
     public Date(long time){
        this.fastTime = time;
    }
    
    //修改了当前Date对象记录的毫秒值。
    public void setTime(long time){
        this.fastTime = time;
    }
    
    public long getTime(){
        return fastTime;
    }
    
}
```

java.util.Date`类 表示特定的瞬间，精确到毫秒。

继续查阅Date类的描述，发现Date拥有多个构造函数，只是部分已经过时，我们重点看以下两个构造函数

- `public Date()`：从运行程序的此时此刻到时间原点经历的毫秒值,转换成Date对象，分配Date对象并初始化此对象，以表示分配它的时间（精确到毫秒）。
- `public Date(long date)`：将指定参数的毫秒值date,转换成Date对象，分配Date对象并初始化此对象，以表示自从标准基准时间（称为“历元（epoch）”，即1970年1月1日00:00:00 GMT）以来的指定毫秒数。

> tips: 由于中国处于东八区（GMT+08:00）是比世界协调时间/格林尼治时间（GMT）快8小时的时区，当格林尼治标准时间为0:00时，东八区的标准时间为08:00。

简单来说：使用无参构造，可以自动设置当前系统时间的毫秒时刻；指定long类型的构造参数，可以自定义毫秒时刻。例如：

```java
import java.util.Date;

public class Demo01Date {
    public static void main(String[] args) {
        // 创建日期对象，把当前的时间
        System.out.println(new Date()); // Tue Jan 16 14:37:35 CST 2020
        // 创建日期对象，把当前的毫秒值转成日期对象
        System.out.println(new Date(0L)); // Thu Jan 01 08:00:00 CST 1970
    }
}
```

> tips:在使用println方法时，会自动调用Date类中的toString方法。Date类对Object类中的toString方法进行了覆盖重写，所以结果为指定格式的字符串。

## 1.2 Date常用方法

Date类中的多数方法已经过时，常用的方法有：

- `public long getTime()` 把日期对象转换成对应的时间毫秒值。
- `public void setTime(long time)` 把方法参数给定的毫秒值设置给日期对象

示例代码

```java
public class DateDemo02 {
    public static void main(String[] args) {
        //创建日期对象
        Date d = new Date();
        
        //public long getTime():获取的是日期对象从1970年1月1日 00:00:00到现在的毫秒值
        //System.out.println(d.getTime());
        //System.out.println(d.getTime() * 1.0 / 1000 / 60 / 60 / 24 / 365 + "年");

        //public void setTime(long time):设置时间，给的是毫秒值
        //long time = 1000*60*60;
        long time = System.currentTimeMillis();
        d.setTime(time);

        System.out.println(d);
    }
}
```

> 小结：Date表示特定的时间瞬间，我们可以使用Date对象对时间进行操作。

## 1.3练习：当前时间加1小时

需求：

​	请打印：当前时间往后走1小时的时间 

代码示例：

```java
//1.创建一个Date对象，表示当前时间
Date d = new Date();
//2.获取当前时间毫秒值
long time = d.getTime();
//3.往后走1小时
//此时time计算完毕之后，表示的就是当前时间之后，1小时的时间毫秒值
time = time + 3600 * 1000;
//4.再设置回去
d.setTime(time);
//5.打印时间
System.out.println(d);
```

# 第二章 SimpleDateFormat类

SimpleDateFormat用于日期格式化，他的父类：`java.text.DateFormat` 是日期/时间格式化的抽象类，我们通过SimpleDateFormat类可以帮我们完成日期和文本之间的转换,也就是可以在Date对象与String对象之间进行来回转换。

- **格式化**：按照指定的格式，把Date对象转换为String对象。
- **解析**：按照指定的格式，把String对象转换为Date对象。

## 2.1 构造方法

由于DateFormat为抽象类，不能直接使用，所以需要常用的子类`java.text.SimpleDateFormat`。这个类需要一个模式（格式）来指定格式化或解析的标准。构造方法为：

- `public SimpleDateFormat(String pattern)`：用给定的模式和默认语言环境的日期格式符号构造SimpleDateFormat。参数pattern是一个字符串，代表日期时间的自定义格式。

## 2.2 格式规则

常用的格式规则为：

| 标识字母（区分大小写） | 含义 |
| ---------------------- | ---- |
| y                      | 年   |
| M                      | 月   |
| d                      | 日   |
| H                      | 时   |
| m                      | 分   |
| s                      | 秒   |

> 备注：更详细的格式规则，可以参考SimpleDateFormat类的API文档。

## 2.3 常用方法

DateFormat类的常用方法有：

- `public String format(Date date)`：将Date对象格式化为字符串。

- `public Date parse(String source)`：将字符串解析为Date对象。

  ```java
  public class SimpleDateFormatDemo {
      public static void main(String[] args) throws ParseException {
          //格式化：从 Date 到 String
          Date d = new Date();
          SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
          String s = sdf.format(d);
          System.out.println(s);
          System.out.println("--------");
  
          //从 String 到 Date
          String ss = "2048-08-09 11:11:11";
          //ParseException
          SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
          Date dd = sdf2.parse(ss);
          System.out.println(dd);
      }
  }
  ```

> 小结：DateFormat可以将Date对象和字符串相互转换。

## 2.4练习：格式化时间

需求：

​	将当前的时间按照以下格式打印：XXXX年XX月XX日 XX:XX:XX

代码示例：

```java
//创建对象，指定对应的模式
//如果空参构造，那么采取默认的模式
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");

//创建Date对象，表示当前时间
Date d = new Date();
//把当前时间格式化成指定的模式
String result = sdf.format(d);
//System.out.println(result);


Date date = sdf.parse("2021年11月11日 11:11:11");
System.out.println(date.getTime());
```

## 2.5练习：秒杀活动

需求：

​	肯打鸡品牌有一个秒杀活动：

​	活动开始时间为：2020年11月11日 0:0:0

​	活动结束时间为：2020年11月11日 0:10:0

现在，有两名同学参加了活动，小贾下单并付款的时间为：2020年11月11日 0:03:47。

小皮下单并付款的时间为：2020年11月11日 0:10:11

要求：

​	用代码说明这两位同学有没有参加上秒杀活动？

分析：

* 判断下单时间是否在开始到结束的范围内。
* 把字符串形式的时间变成毫秒值。

代码示例：

```java
//1.创建日期格式化对象，并指定对应的格式
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
//2.计算活动开始时间的毫秒值
String start = "2020年11月11日 0:0:0";//定义字符串表示活动开始时间
Date startDate = sdf.parse(start);//把字符串解析成Date对象
long startTime = startDate.getTime();//获取Date对象中的时间毫秒值
//3.计算活动结束时间的毫秒值
String end = "2020年11月11日 0:10:0";
Date endDate = sdf.parse(end);
long endTime = endDate.getTime();
//4.计算小贾同学下单时间的毫秒值
String user = "2020年11月11日 0:03:47";
Date userDate = sdf.parse(user);
long userTime = userDate.getTime();
//5.判断userTime是否在活动时间之内即可
if(userTime >= startTime && userTime <= endTime){
    System.out.println("参加活动成功");
}else{
    System.out.println("参加活动失败");
}
```

# 第三章 Calendar类

## 3.1 概述

- java.util.Calendar类表示一个“日历类”，可以进行日期运算。它是一个抽象类，不能创建对象，我们可以使用它的子类：java.util.GregorianCalendar类。
- 有两种方式可以获取GregorianCalendar对象：
  - 直接创建GregorianCalendar对象；
  - 通过Calendar的静态方法getInstance()方法获取GregorianCalendar对象【本次课使用】

## 细节：

* Calendar中的月份范围：0~11

​	解释：方法返回0，表示实际是1月份。返回11，表示实际是12月份

*  Calendar中的星期：老外眼中周日是一周中的第一天。

  解释：方法返回1，表示实际是周日。返回2，表示实际是周一

## 3.2 常用方法

| 方法名                                | 说明                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| public static Calendar getInstance()  | 获取一个它的子类GregorianCalendar对象。                      |
| public int get(int field)             | 获取某个字段的值。field参数表示获取哪个字段的值，<br />可以使用Calender中定义的常量来表示：<br />Calendar.YEAR : 年<br />Calendar.MONTH ：月<br />Calendar.DAY_OF_MONTH：月中的日期<br />Calendar.HOUR：小时<br />Calendar.MINUTE：分钟<br />Calendar.SECOND：秒<br />Calendar.DAY_OF_WEEK：星期 |
| public void set(int field,int value)  | 设置某个字段的值                                             |
| public void set(年，月，日)           | 设置年月日，还可以设置年月日时分秒，有很多重载方法           |
| public void setTime(Date date)        | 把一个date对象设置到日历类对象中                             |
| public void add(int field,int amount) | 为某个字段增加/减少指定的值                                  |

## 3.3 get方法示例

注意：get方法可以获取指定字段的值。

但是要注意细节：

- Calendar中的月份范围：0~11

​	解释：方法返回0，表示实际是1月份。返回11，表示实际是12月份

-  Calendar中的星期：老外眼中周日是一周中的第一天。

  解释：方法返回1，表示实际是周日。返回2，表示实际是周一

```java
public class Test5 {
    public static void main(String[] args) {
        //获取日历类的对象
        //扩展：子类有很多
        //针对于每一个时区都有一个子类对象
        //getInstance获取到一个日历的对象即可
        //扩展：getInstance方法底层，会根据当前的时区来选择不同的子类，并创建对象返回。
        Calendar c = Calendar.getInstance();

        //设置
        //在设置的时候，可以设置年月日，年月日时分秒，也可以设置单独的属性
        //c.set(2020,10,11);
        //c.set(Calendar.YEAR,2022);
        //修改
        c.add(Calendar.MONTH,3);
        //获取日历里面的信息
        int year = c.get(Calendar.YEAR);
        System.out.println(year + "年");

        //月份的范围：0~11 ，如果要变成现实生活中的，需要+1理解
        int month = c.get(Calendar.MONTH);
        System.out.println((month + 1) + "月");

        int day = c.get(Calendar.DAY_OF_MONTH);
        System.out.println(day + "号");

        //星期: 星期日 星期一  星期二  星期三  星期四 星期五 星期六
        //在老外的眼中，星期天是一周中的第一天。
        //如何把数字变成正确的星期，采用查表法
        int week = c.get(Calendar.DAY_OF_WEEK);
        System.out.println(getWeek(week));
    }


    public static String getWeek(int index){
        //查表法
        //数据 跟数字一一对应起来。
        String[] weeksArr = {"","星期日","星期一","星期二","星期三","星期四","星期五","星期六"};
        return weeksArr[index];
    }
}

```

## 3.4 set方法示例：

set方法，可以设置单独属性的值。还有一些重载的方法，可以设置年，月，日。或者设置年月日时分秒。

还有一个setTime方法，可以直接把Date对象设置给Calendar对象。

```java
public class Demo {
    public static void main(String[] args) {
        //设置属性——set(int field,int value):
		Calendar c1 = Calendar.getInstance();//获取当前日期

		//计算班长出生那天是星期几(假如班长出生日期为：1998年3月18日)
		c1.set(Calendar.YEAR, 1998);
		c1.set(Calendar.MONTH, 3 - 1);//转换为Calendar内部的月份值
		c1.set(Calendar.DAY_OF_MONTH, 18);

		int w = c1.get(Calendar.DAY_OF_WEEK);
		System.out.println("班长出生那天是：" + getWeek(w));

        
    }
    //查表法，查询星期几
    public static String getWeek(int index) {//w = 1 --- 7
        //做一个表(数组)
        //数据 跟数字一一对应起来。
        String[] weeksArr = {"","星期日","星期一","星期二","星期三","星期四","星期五","星期六"};
        return weeksArr[index];
    }
}
```

## 3.5 add方法示例：

add方法可以为某个字段增加/减少指定的值

但是要注意细节：

1，负数往前减，正数往后加。

2，如果时间为12月，再往后加一个月，会跳到下一年的1月。

```java
public class Demo {
    public static void main(String[] args) {
        //计算200天以后是哪年哪月哪日，星期几？
		Calendar c2 = Calendar.getInstance();//获取当前日期
        c2.add(Calendar.DAY_OF_MONTH, 200);//日期加200

        int y = c2.get(Calendar.YEAR);
        int m = c2.get(Calendar.MONTH) + 1;//转换为实际的月份
        int d = c2.get(Calendar.DAY_OF_MONTH);

        int wk = c2.get(Calendar.DAY_OF_WEEK);
        System.out.println("200天后是：" + y + "年" + m + "月" + d + "日" + getWeek(wk));

    }
     //查表法，查询星期几
    public static String getWeek(int index) {//w = 1 --- 7
        //做一个表(数组)
        //数据 跟数字一一对应起来。
        String[] weeksArr = {"","星期日","星期一","星期二","星期三","星期四","星期五","星期六"};
        return weeksArr[index];
    }
}
```

## 3.6获取方法示例：

代码示例：

```java
//1.创建日历类的对象
Calendar c = Calendar.getInstance();

//先获取date对象，再获取时间的毫秒值
Date date = c.getTime();
System.out.println(date);
System.out.println(date.getTime());

//直接获取毫秒值
long timeInMillis = c.getTimeInMillis();
System.out.println(timeInMillis);
```



# 第四章 JDK8时间相关类

## 4.1 概述

- LocalTime /LocalDate / LocalDateTime
- Instant
- DateTimeFormatter
- Duration/Period
- ChronoUnit

## 4.2 常用方法和常见代码

### 4.2.1LocalDate

表示年月日

```java
public class Demo01LocalDate {
    public static void main(String[] args) {
        // 1、获取本地日期对象。
        LocalDate nowDate = LocalDate.now();
        System.out.println("今天的日期：" + nowDate);

        int year = nowDate.getYear();
        System.out.println("year：" + year);


        int month = nowDate.getMonthValue();
        System.out.println("month：" + month);

        int day = nowDate.getDayOfMonth();
        System.out.println("day：" + day);

        //当年的第几天
        int dayOfYear = nowDate.getDayOfYear();
        System.out.println("dayOfYear：" + dayOfYear);

        //星期
        System.out.println("星期：" + nowDate.getDayOfWeek());
        System.out.println("星期：" + nowDate.getDayOfWeek().getValue());

        //月份
        System.out.println("月份：" +nowDate.getMonth());
        System.out.println("月份：" + nowDate.getMonth().getValue());

    }
}

```

### 4.2.2LocalTime

表示时分秒

```java
package com.itheima.a03jdk8time;

import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.Month;

public class Demo02LocalTime {
    public static void main(String[] args) {
        // 1、获取本地时间对象。
        LocalTime nowTime = LocalTime.now();
        System.out.println("今天的时间：" + nowTime);//今天的时间：

        int hour = nowTime.getHour();//时
        System.out.println("hour：" + hour);//hour：

        int minute = nowTime.getMinute();//分
        System.out.println("minute：" + minute);//minute：

        int second = nowTime.getSecond();//秒
        System.out.println("second：" + second);//second：

        int nano = nowTime.getNano();//纳秒
        System.out.println("nano：" + nano);//nano：

        System.out.println("-----");
        LocalTime time = LocalTime.of(8, 30);
        System.out.println(time);//时分
        System.out.println(LocalTime.of(8, 20, 30));//时分秒
        System.out.println(LocalTime.of(8, 20, 30, 150));//时分秒纳秒
        LocalTime mTime = LocalTime.of(8, 20, 30, 150);

        System.out.println("---------------");
        //设置年月日，时分
        System.out.println(LocalDateTime.of(2021, 11, 11, 8, 20));
        //设置年月日，时分秒
        System.out.println(LocalDateTime.of(2021, 11, 11, 8, 20, 30));
        //设置年月日，时分秒，纳秒
        System.out.println(LocalDateTime.of(2021, 11, 11, 8, 20, 30, 150));
    }
}

```

### 4.2.3LocalDateTime

表示年月日时分秒

```java
package com.itheima.a03jdk8time;

import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;

public class Demo03LocalDateTime {
    public static void main(String[] args) {
        // 日期 时间
        LocalDateTime nowDateTime = LocalDateTime.now();
        System.out.println("今天是：" + nowDateTime);//今天是：
        System.out.println(nowDateTime.getYear());//年
        System.out.println(nowDateTime.getMonthValue());//月
        System.out.println(nowDateTime.getDayOfMonth());//日
        System.out.println(nowDateTime.getHour());//时
        System.out.println(nowDateTime.getMinute());//分
        System.out.println(nowDateTime.getSecond());//秒
        System.out.println(nowDateTime.getNano());//纳秒
        //日：当年的第几天
        System.out.println("dayOfYear：" + nowDateTime.getDayOfYear());
        //星期
        System.out.println("星期" + nowDateTime.getDayOfWeek());
        System.out.println("星期" + nowDateTime.getDayOfWeek().getValue());
        //月份
        System.out.println("月份" + nowDateTime.getMonth());
        System.out.println("月份" + nowDateTime.getMonth().getValue());


        //LocalDateTime变成LocalDate
        LocalDate ld = nowDateTime.toLocalDate();
        System.out.println(ld);

        //LocalDateTime变成LocalTime
        LocalTime lt = nowDateTime.toLocalTime();
        System.out.println(lt.getHour());
        System.out.println(lt.getMinute());
        System.out.println(lt.getSecond());
    }
}

```

### 4.2.4修改时间

```java
package com.itheima.a03jdk8time;

import java.time.LocalDate;
import java.time.LocalTime;
import java.time.MonthDay;

public class Demo04UpdateTime {
    public static void main(String[] args) {
        LocalTime nowTime = LocalTime.now();
        System.out.println(nowTime);//当前时间
        System.out.println(nowTime.minusHours(1));//一小时前
        System.out.println(nowTime.minusMinutes(1));//一分钟前
        System.out.println(nowTime.minusSeconds(1));//一秒前
        System.out.println(nowTime.minusNanos(1));//一纳秒前
        System.out.println(nowTime);

        System.out.println("----------------");

        System.out.println(nowTime.plusHours(1));//一小时后
        System.out.println(nowTime.plusMinutes(1));//一分钟后
        System.out.println(nowTime.plusSeconds(1));//一秒后
        System.out.println(nowTime.plusNanos(1));//一纳秒后

        System.out.println("------------------");
        // 不可变对象，每次修改产生新对象！
        System.out.println(nowTime);

        System.out.println("---------------");
        LocalDate myDate = LocalDate.of(2021, 10, 30);
        LocalDate nowDate = LocalDate.now();

        //今天是2021-10-29吗？
        System.out.println("今天是2021-10-29吗？ " + nowDate.equals(myDate));

        //2021-10-30是否在nowDate之前？
        System.out.println(myDate + "是否在" + nowDate + "之前？ " + myDate.isBefore(nowDate));

        //2021-10-30是否在nowDate之后？ false
        System.out.println(myDate + "是否在" + nowDate + "之后？ " + myDate.isAfter(nowDate));
        System.out.println("---------------------------");

        // 判断今天是否是你的生日
        //110101202110101234
        LocalDate birDate = LocalDate.of(2000, 10, 29);
        LocalDate nowDate1 = LocalDate.now();

        // 提取你的月 和 日对象封装成月日对象
        MonthDay birMd = MonthDay.of(birDate.getMonthValue(), birDate.getDayOfMonth());
        MonthDay nowMd = MonthDay.from(nowDate1);
        System.out.println("今天是你的生日吗？ " + birMd.equals(nowMd));
    }
}

```

### 4.2.5Instant

表示时间戳，跟JDK7之前的Date类似

```java
package com.itheima.a03jdk8time;

import java.time.Instant;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.util.Date;

public class Demo05Instant {
    public static void main(String[] args) {
        // 1、得到一个Instant时间戳对象
        Instant instant = Instant.now();
        System.out.println(instant);

        // 2、系统此刻的时间戳怎么办？
        Instant instant1 = Instant.now();
        System.out.println(instant1.atZone(ZoneId.systemDefault()));

        // 3、如何去返回Date对象
        Date date = Date.from(instant);
        System.out.println(date);

        Instant i2 = date.toInstant();
        System.out.println(i2);

        LocalDateTime ldt = LocalDateTime.ofInstant(i2, ZoneId.systemDefault());
        System.out.println(ldt.getDayOfMonth());

    }
}

```

### 4.2.6DateTimeFormat

表示格式化时间，跟JDK7之前的SimpleDateFormat类似

```java
package com.itheima.a03jdk8time;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
public class Demo06DateTimeFormat {
    public static void main(String[] args) {
        //本地此刻日期时间对象
        LocalDateTime ldt = LocalDateTime.now();
        System.out.println(ldt);

        // 解析/格式化器
        DateTimeFormatter dtf =
                DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss EEE a");

        // 可以用解析器对象调用format方法进行格式化
        System.out.println(dtf.format(ldt));
        // 也可以用日期时间对象调用format方法进行格式化
        System.out.println(ldt.format(dtf));

        // 解析字符串时间
        DateTimeFormatter dtf1 = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        // 解析当前字符串时间成为本地日期时间对象
        LocalDateTime ldt1 = LocalDateTime.parse("2019-11-11 11:11:11" ,  dtf1);
        System.out.println(ldt1);
        System.out.println(ldt1.getDayOfYear());
    }
}

```

### 4.2.7Period

Period计算日期间隔，用于 LocalDate 之间的比较

```java
/*
* Period计算日期间隔，用于 LocalDate 之间的比较
*
* */
public class Demo07Period {
    public static void main(String[] args) {
        // 当前本地 年月日
        LocalDate today = LocalDate.now();
        System.out.println(today);// 2021-10-29

        // 生日的 年月日
        LocalDate birthDate = LocalDate.of(2020, 01, 01);
        System.out.println(birthDate); // 1998-10-13

        Period period = Period.between(birthDate, today);//第二个参数减第一个参数
        System.out.println(period.getYears());
        System.out.println(period.getMonths());
        System.out.println(period.getDays());
        System.out.println(period.toTotalMonths());
    }
}

```

### 4.2.8Duration

计算两个“时间”间隔，用于 LocalDateTime，Instant 之间的比较

```java
package com.itheima.a03jdk8time;

import java.time.Duration;
import java.time.LocalDateTime;

/*
 * Duration计算日期间隔，用于 LocalDateTime，Instant 之间的比较
 *
 * */
public class Demo08Duration {
    public static void main(String[] args) {
        // 本地日期时间对象。
        LocalDateTime today = LocalDateTime.now();
        System.out.println(today);

        // 出生的日期时间对象
        LocalDateTime birthDate = LocalDateTime.of(2020,01
                ,01,00,00,00);

        System.out.println(birthDate);

        Duration duration = Duration.between(  birthDate , today);//第二个参数减第一个参数

        System.out.println(duration.toDays());//两个时间差的天数
        System.out.println(duration.toHours());//两个时间差的小时数
        System.out.println(duration.toMinutes());//两个时间差的分钟数
        System.out.println(duration.toMillis());//两个时间差的毫秒数
        System.out.println(duration.toNanos());//两个时间差的纳秒数
    }
}

```

### 4.2.9ChronoUnit

ChronoUnit可用于在单个时间单位内测量一段时间，这个工具类是最全的了，可以用于比较所有的时间单位

```java
package com.itheima.a03jdk8time;

import java.time.LocalDateTime;
import java.time.temporal.ChronoUnit;


/*
 * ChronoUnit可用于在单个时间单位内测量一段时间，这个工具类是最全的了，可以用于比较所有的时间单位
 *
 * */
public class Demo09ChronoUnit {
    public static void main(String[] args) {
        // 本地日期时间对象：此刻的
        LocalDateTime today = LocalDateTime.now();
        System.out.println(today);

        // 生日时间
        LocalDateTime birthDate = LocalDateTime.of(2020,1,1,
                0,0,0);
        System.out.println(birthDate);

        System.out.println("相差的年数：" + ChronoUnit.YEARS.between(birthDate, today));
        System.out.println("相差的月数：" + ChronoUnit.MONTHS.between(birthDate, today));
        System.out.println("相差的周数：" + ChronoUnit.WEEKS.between(birthDate, today));
        System.out.println("相差的天数：" + ChronoUnit.DAYS.between(birthDate, today));
        System.out.println("相差的时数：" + ChronoUnit.HOURS.between(birthDate, today));
        System.out.println("相差的分数：" + ChronoUnit.MINUTES.between(birthDate, today));
        System.out.println("相差的秒数：" + ChronoUnit.SECONDS.between(birthDate, today));
        System.out.println("相差的毫秒数：" + ChronoUnit.MILLIS.between(birthDate, today));
        System.out.println("相差的微秒数：" + ChronoUnit.MICROS.between(birthDate, today));
        System.out.println("相差的纳秒数：" + ChronoUnit.NANOS.between(birthDate, today));
        System.out.println("相差的半天数：" + ChronoUnit.HALF_DAYS.between(birthDate, today));
        System.out.println("相差的十年数：" + ChronoUnit.DECADES.between(birthDate, today));
        System.out.println("相差的世纪（百年）数：" + ChronoUnit.CENTURIES.between(birthDate, today));
        System.out.println("相差的千年数：" + ChronoUnit.MILLENNIA.between(birthDate, today));
        System.out.println("相差的纪元数：" + ChronoUnit.ERAS.between(birthDate, today));
    }
}
```

# 第五章  包装类

## 5.1 概述

Java提供了两个类型系统，基本类型与引用类型，使用基本类型在于效率，然而很多情况，会创建对象使用，因为对象可以做更多的功能，如果想要我们的基本类型像对象一样操作，就可以使用基本类型对应的包装类，如下：

| 基本类型 | 对应的包装类（位于java.lang包中） |
| -------- | --------------------------------- |
| byte     | Byte                              |
| short    | Short                             |
| int      | **Integer**                       |
| long     | Long                              |
| float    | Float                             |
| double   | Double                            |
| char     | **Character**                     |
| boolean  | Boolean                           |

## 5.2 Integer类

- Integer类概述

  包装一个对象中的原始类型 int 的值

- Integer类构造方法及静态方法

| 方法名                                  | 说明                                      |
| --------------------------------------- | ----------------------------------------- |
| public Integer(int   value)             | 根据 int 值创建 Integer 对象**(过时)**    |
| public Integer(String s)                | 根据 String 值创建 Integer 对象**(过时)** |
| public static Integer valueOf(int i)    | 返回表示指定的 int 值的 Integer   实例    |
| public static Integer valueOf(String s) | 返回保存指定String值的 Integer 对象       |

- 示例代码

```java
public class IntegerDemo {
    public static void main(String[] args) {
        //public Integer(int value)：根据 int 值创建 Integer 对象(过时)
        Integer i1 = new Integer(100);
        System.out.println(i1);

        //public Integer(String s)：根据 String 值创建 Integer 对象(过时)
        Integer i2 = new Integer("100");
		//Integer i2 = new Integer("abc"); //NumberFormatException
        System.out.println(i2);
        System.out.println("--------");

        //public static Integer valueOf(int i)：返回表示指定的 int 值的 Integer 实例
        Integer i3 = Integer.valueOf(100);
        System.out.println(i3);

        //public static Integer valueOf(String s)：返回保存指定String值的Integer对象 
        Integer i4 = Integer.valueOf("100");
        System.out.println(i4);
    }
}
```

## 5.3 装箱与拆箱

基本类型与对应的包装类对象之间，来回转换的过程称为”装箱“与”拆箱“：

- **装箱**：从基本类型转换为对应的包装类对象。
- **拆箱**：从包装类对象转换为对应的基本类型。

用Integer与 int为例：（看懂代码即可）

基本数值---->包装对象

```java
Integer i = new Integer(4);//使用构造函数函数
Integer iii = Integer.valueOf(4);//使用包装类中的valueOf方法
```

包装对象---->基本数值

```java
int num = i.intValue();
```

## 5.4 自动装箱与自动拆箱

由于我们经常要做基本类型与包装类之间的转换，从Java 5（JDK 1.5）开始，基本类型与包装类的装箱、拆箱动作可以自动完成。例如：

```java
Integer i = 4;//自动装箱。相当于Integer i = Integer.valueOf(4);
i = i + 5;//等号右边：将i对象转成基本数值(自动拆箱) i.intValue() + 5;
//加法运算完成后，再次装箱，把基本数值转成对象。
```

## 5.5 基本类型与字符串之间的转换

### 基本类型转换为String

- 转换方式
- 方式一：直接在数字后加一个空字符串
- 方式二：通过String类静态方法valueOf()
- 示例代码

```java
public class IntegerDemo {
    public static void main(String[] args) {
        //int --- String
        int number = 100;
        //方式1
        String s1 = number + "";
        System.out.println(s1);
        //方式2
        //public static String valueOf(int i)
        String s2 = String.valueOf(number);
        System.out.println(s2);
        System.out.println("--------");
    }
}
```

### String转换成基本类型 

除了Character类之外，其他所有包装类都具有parseXxx静态方法可以将字符串参数转换为对应的基本类型：

- `public static byte parseByte(String s)`：将字符串参数转换为对应的byte基本类型。
- `public static short parseShort(String s)`：将字符串参数转换为对应的short基本类型。
- **`public static int parseInt(String s)`：将字符串参数转换为对应的int基本类型。**
- **`public static long parseLong(String s)`：将字符串参数转换为对应的long基本类型。**
- `public static float parseFloat(String s)`：将字符串参数转换为对应的float基本类型。
- `public static double parseDouble(String s)`：将字符串参数转换为对应的double基本类型。
- `public static boolean parseBoolean(String s)`：将字符串参数转换为对应的boolean基本类型。

代码使用（仅以Integer类的静态方法parseXxx为例）如：

- 转换方式
  - 方式一：先将字符串数字转成Integer，再调用valueOf()方法
  - 方式二：通过Integer静态方法parseInt()进行转换
- 示例代码

```java
public class IntegerDemo {
    public static void main(String[] args) {
        //String --- int
        String s = "100";
        //方式1：String --- Integer --- int
        Integer i = Integer.valueOf(s);
        //public int intValue()
        int x = i.intValue();
        System.out.println(x);
        //方式2
        //public static int parseInt(String s)
        int y = Integer.parseInt(s);
        System.out.println(y);
    }
}
```

> 注意:如果字符串参数的内容无法正确转换为对应的基本类型，则会抛出`java.lang.NumberFormatException`异常。

## 5.6练习：类型转换

需求：

定义一个集合，存5个键盘录入的整数。（键盘录入要求用next方法录入）
并把他们存入到集合中
要求1：求5个数的平均数
要求2：统计这5个数中，有多少个数字比平均大。

代码示例：

```java
//1.创建一个集合
ArrayList<Integer> list = new ArrayList<>();
//2.循环5次并存入集合
Scanner sc = new Scanner(System.in);
for (int i = 0; i < 5; i++) {
    System.out.println("请输入一个整数");
    String numberStr = sc.next();
    //把字符串变成整数
    int number = Integer.parseInt(numberStr);
    //直接添加
    list.add(number);//是在添加数据的时候，进行的装箱
}
//4.统计
//思路清晰，一个循环，只做一件事情
int sum = 0;
for (int i = 0; i < list.size(); i++) {
    //先把获取到的数据拆箱，变成基本数据类型，再跟sum进行相加
    sum = sum + list.get(i);//+=
}
//5.求平均数
int avg = sum / list.size();
//6.统计
int count = 0;
for (int i = 0; i < list.size(); i++) {
    if(list.get(i) > avg){
        count++;
    }
}
//7.打印
System.out.println(count + "个");
```

# 第六章 正则表达式

开发心得：看着正确数据，从左到右书写正则表达式

## 6.1 正则表达式的概念及演示

- 在Java中，我们经常需要验证一些字符串，例如：年龄必须是2位的数字、用户名必须是8位长度而且只能包含大小写字母、数字等。正则表达式就是用来验证各种字符串的规则。它内部描述了一些规则，我们可以验证用户输入的字符串是否匹配这个规则。
- 先看一个不使用正则表达式验证的例子：下面的程序让用户输入一个QQ号码，我们要验证：
  - QQ号码必须是6--20位长度
  - 而且必须全部是数字
  - 而且首位不能为0

```java
package com.itheima.a05regex;

public class Test1 {
    public static void main(String[] args) {
        // 假如现在要求校验一个qq号码是否正确，6位及20位之内，0不能在开头，必须全部是数字 。
        System.out.println(checkQQ("23658977452"));	
        System.out.println(checkQQ("236589775223589775223589775223589775223658977452"));
    }
    public static boolean checkQQ(String qq){
        //1.校验长度
        if(qq.length() < 6 || qq.length() > 20){
            return false;
        }
        //2.校验开头
        char c = qq.charAt(0);
        if(c == '0'){
            return false;
        }
        //3.校验内容
        for (int i = 0; i < qq.length(); i++) {
            //i索引 charAt(i)字符
            char at = qq.charAt(i);
            if(at < '0' || at > '9'){
                return false;
            }
        }
        //4.当循环结束之后，还没有发现不是数字的
        return true;
    }
}

```

- 使用正则表达式验证：

```java
public class Demo {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		System.out.println("请输入你的QQ号码：");
		String qq = sc.next();
		
		System.out.println(checkQQ2(qq));
	}
	//使用正则表达式验证
	private static boolean checkQQ2(String qq){
		String regex = "[1-9]\\d{5,19}";//正则表达式
		return qq.matches(regex);
	}
}
```

上面程序checkQQ2()方法中String类型的变量regex就存储了一个"正则表达式 "，而这个正则表达式就描述了我们需要的三个规则。matches()方法是String类的一个方法，用于接收一个正则表达式，并将"本对象"与参数"正则表达式"进行匹配，如果本对象符合正则表达式的规则，则返回true，否则返回false。

**我们接下来就重点学习怎样写正则表达式**

## 6.2 正则表达式-字符类

- 语法示例：

1. \[abc\]：代表a或者b，或者c字符中的一个。
2. \[^abc\]：代表除a,b,c以外的任何字符。
3. [a-z]：代表a-z的所有小写字符中的一个。
4. [A-Z]：代表A-Z的所有大写字符中的一个。
5. [0-9]：代表0-9之间的某一个数字字符。
6. [a-zA-Z0-9]：代表a-z或者A-Z或者0-9之间的任意一个字符。
7. [a-dm-p]：a 到 d 或 m 到 p之间的任意一个字符。 

- 代码示例：

```java
public class Demo {
	public static void main(String[] args) {
		String str = "ead";
		
		//1.验证str是否以h开头，以d结尾，中间是a,e,i,o,u中某个字符
		String regex = "h[aeiou]d";
		System.out.println("1." + str.matches(regex));
		
		//2.验证str是否以h开头，以d结尾，中间不是a,e,i,o,u中的某个字符
		regex = "h[^aeiou]d";
		System.out.println("2." +  str.matches(regex));
		
		//3.验证str是否a-z的任何一个小写字符开头，后跟ad
		regex = "[a-z]ad";
		System.out.println("3." + str.matches(regex));
		
		//4.验证str是否以a-d或者m-p之间某个字符开头，后跟ad
		regex = "[[a-d][m-p]]ad";
		System.out.println("4." + str.matches(regex));
	}
}

```

## 6.3 正则表达式-逻辑运算符

- 语法示例：
  1. &&：并且
  2. |    ：或者
- 代码示例：

```java
public class Demo {
	public static void main(String[] args) {
		String str = "had";
		
		//1.要求字符串是小写辅音字符开头，后跟ad
		String regex = "[a-z&&[^aeiou]]ad";
		System.out.println("1." + str.matches(regex));
		
		//2.要求字符串是aeiou中的某个字符开头，后跟ad
		regex = "[a|e|i|o|u]ad";//这种写法相当于：regex = "[aeiou]ad";
		System.out.println("2." + str.matches(regex));
	}
}

```



## 6.4 正则表达式-预定义字符

- 语法示例：
  1. "." ： 匹配任何字符。
  2. "\d"：任何数字[0-9]的简写；
  3. "\D"：任何非数字\[^0-9\]的简写；
  4. "\s"： 空白字符：[ \t\n\x0B\f\r] 的简写
  5. "\S"： 非空白字符：\[^\s\] 的简写
  6. "\w"：单词字符：[a-zA-Z_0-9]的简写
  7. "\W"：非单词字符：\[^\w\]
- 代码示例：

```java
public class Demo {
	public static void main(String[] args) {
		String str = "258";
		
		//1.验证str是否3位数字
		String regex = "\\d\\d\\d";
		System.out.println("1." + str.matches(regex));
		
		//2.验证手机号：1开头，第二位：3/5/8，剩下9位都是0-9的数字
        str = "13513153355";//要验证的字符串
		regex = "1[358]\\d\\d\\d\\d\\d\\d\\d\\d\\d";//正则表达式
		System.out.println("2." + str.matches(regex));
		
		//3.验证字符串是否以h开头，以d结尾，中间是任何字符
		str = "had";//要验证的字符串
		regex = "h.d";//正则表达式
		System.out.println("3." + str.matches(regex));
		
		//4.验证str是否是：had.
        str = "had.";//要验证的字符串
		regex = "had\\.";//\\.代表'.'符号，因为.在正则中被预定义为"任意字符"，不能直接使用
		System.out.println("4." + str.matches(regex));
		
	}
}
```

## 6.5 正则表达式-数量词

- 语法示例：
  1. X? : 0次或1次
  2. X* : 0次到多次
  3. X+ : 1次或多次
  4. X{n} : 恰好n次
  5. X{n,} : 至少n次
  6. X{n,m}: n到m次(n和m都是包含的)
- 代码示例：

```java
public class Demo {
	public static void main(String[] args) {
		String str = "";
		
		//1.验证str是否是三位数字
        str = "012";
		String regex = "\\d{3}";
		System.out.println("1." + str.matches(regex));
		
		//2.验证str是否是多位数字
        str = "88932054782342";
		regex = "\\d+";
		System.out.println("2." + str.matches(regex));
		
		//3.验证str是否是手机号：
        str = "13813183388";
		regex = "1[358]\\d{9}";
		System.out.println("3." + str.matches(regex));
		
		//4.验证小数:必须出现小数点，但是只能出现1次
		String s2 = "3.1";
		regex = "\\d*\\.{1}\\d+";
		System.out.println("4." + s2.matches(regex));
		
		//5.验证小数：小数点可以不出现，也可以出现1次
		regex = "\\d+\\.?\\d+";
		System.out.println("5." + s2.matches(regex));
		
		//6.验证小数：要求匹配：3、3.、3.14、+3.14、-3.
        s2 = "-3.";
		regex = "[+-]\\d+\\.?\\d*";
		System.out.println("6." + s2.matches(regex));
		
		//7.验证qq号码：1).5--15位；2).全部是数字;3).第一位不是0
        s2 = "1695827736";
		regex = "[1-9]\\d{4,14}";
		System.out.println("7." + s2.matches(regex));
	}
}

```

## 6.6 String的split方法中使用正则表达式

- String类的split()方法原型：

  ```java
  public String[] split(String regex)//参数regex就是一个正则表达式。可以将当前字符串中匹配regex正则表达式的符号作为"分隔符"来切割字符串。
  ```

- 代码示例：

```java
String names = "小路dqwefqwfqwfwq12312蓉儿dqwefqwfqwfwq12312小何";
//要求1：把字符串中的三个姓名切割出来
String[] arr = names.split("\\w+");
//String[] arr = names.split("\\.");

for (int i = 0; i < arr.length; i++) {
    System.out.print(arr[i] + " ");
}
System.out.println();

```

## 6.7 String类的replaceAll方法中使用正则表达式

- String类的replaceAll()方法原型：

```java
public String replaceAll(String regex,String newStr)
//参数regex就是一个正则表达式。可以将当前字符串中匹配regex正则表达式的字符串替换为newStr。
```

- 代码示例：

```java
//要求2：把字符串中三个姓名之间的字母替换为vs
String names = "小路dqwefqwfqwfwq12312蓉儿dqwefqwfqwfwq12312小何";
//切记：字符串本身是不可变的，不管是截取，还是替换还是其他操作，不能改变字符串本身
//只有返回值才是操作之后的结果
String result = names.replaceAll("\\w+", "vs");
System.out.println(result);
```

## 6.8练习1-匹配手机号码(掌握)

代码示例：

```java
//验证手机号码      13112345678  13712345667  13945679027
String regex1 = "[1-9][3-9]\\d{9}";
System.out.println("13112345678".matches(regex1));
System.out.println("13712345667".matches(regex1));
System.out.println("13945679027".matches(regex1));
System.out.println("139456790271".matches(regex1));    
```

## 6.9练习2-匹配座机号码(掌握)

代码示例：

```java
//验证座机电话号码  020-2324242 02122442 027-42424 0712-3242434
String regex2 = "0\\d{2,3}-?[1-9]\\d{4,9}";
System.out.println("020-2324242".matches(regex2));
System.out.println("02122442".matches(regex2));
System.out.println("027-42424".matches(regex2));
System.out.println("0712-3242434".matches(regex2));
```

## 6.10练习3-匹配邮箱(了解)

代码示例：

```java
//验证邮箱3232323@qq.com  zhangsan@itcast.cnn  dlei0009@163.com  dlei0009@pci.com.cn
//把邮箱分为三段来写：
//第一段：@的左半部分，任意的字母数字下划线，至少出现一次  		\\w+
//第二段：@只能出现一次   									@
//第三段：比较难，我们可以把第三段继续分为三段
//3-1：点的左半部分：字母和数字可以出现2~8次。 				  [\w^_]{2,8}
//3-2：点     											  \\.
//3-3：点的右半部分：大写字母和小写字母只能出现2~3次。				[a-zA-Z]{2,3}
//又因为，有的邮箱，还有可能会有点，所以把3-2和3-3看成一个整体，这个整体可以出现1~2次。
String regex3 = "\\w+@[\\w^_]{2,8}(\\.[a-zA-Z]{2,3}){1,2}";
System.out.println("3232323@qq.com".matches(regex3));
System.out.println("zhangsan@itcast.cnn".matches(regex3));
System.out.println("dlei0009@163.com".matches(regex3));
System.out.println("dlei0009@pci.com.cn".matches(regex3));
```

# 第七章 Arrays类

## 7.1常见方法

| 方法名                                           | 说明                                                     |
| ------------------------------------------------ | -------------------------------------------------------- |
| public static String toString(类型[] a)          | 把数组变成字符串返回                                     |
| public static void sort(类型[] a)                | 对数组进行默认升序排序                                   |
| public static int binarySearch(int[] a, int key) | 二分搜索数组中的数据，存在返回索引，不存在返回负插入点-1 |

代码示例：

```java
public class Demo1 {
    public static void main(String[] args) {
        //1.定义数组
        //int[] arr = {4,1,3,2,5};
        //对数组中的数据进行排序
        //Arrays.sort(arr);

        //把数组里面所有的数据，拼接在一起，变成一个字符串给你返回
        //String result = Arrays.toString(arr);
        //System.out.println(result);
        //System.out.println(Arrays.toString(arr));


        //binarySearch作用：查找某一个数据在数组中出现的索引
        //前提：数组必须是从小到大，有序的
        //如果要查询的数据不存在，那么返回负数
        //了解：-插入点-1
        int[] arr = {1,2,3,4,5,7,9,10,20,30};
        int index = Arrays.binarySearch(arr, 0);
        System.out.println(index);
    }
}

```

# 第八章  Lambda表达式

## 8.1 Lambda的格式

### 标准格式:

Lambda省去面向对象的条条框框，格式由**3个部分**组成：

- 一些参数
- 一个箭头
- 一段代码

Lambda表达式的**标准格式**为：

```
(参数类型 参数名称) -> { 代码语句 }

```

**格式说明：**

- 小括号内的语法与传统方法参数列表一致：无参数则留空；多个参数则用逗号分隔。
- `->`是新引入的语法格式，代表指向动作。
- 大括号内的语法与传统方法体要求基本一致。

**匿名内部类与lambda对比:**

```java
new Thread(new Runnable() {
			@Override
			public void run() {
				System.out.println("多线程任务执行！");
			}
}).start();
```

仔细分析该代码中，`Runnable`接口只有一个`run`方法的定义：

- `public abstract void run();`

即制定了一种做事情的方案（其实就是一个方法）：

- **无参数**：不需要任何条件即可执行该方案。
- **无返回值**：该方案不产生任何结果。
- **代码块**（方法体）：该方案的具体执行步骤。

同样的语义体现在`Lambda`语法中，要更加简单：

```java
() -> System.out.println("多线程任务执行！")
```

- 前面的一对小括号即`run`方法的参数（无），代表不需要任何条件；
- 中间的一个箭头代表将前面的参数传递给后面的代码；
- 后面的输出语句即业务逻辑代码。

### 省略格式:

**省略规则**

在Lambda标准格式的基础上，使用省略写法的规则为：

1. 小括号内参数的类型可以省略；
2. 如果小括号内**有且仅有一个参**，则小括号可以省略；
3. 如果大括号内**有且仅有一个语句**，则无论是否有返回值，都可以省略大括号、return关键字及语句分号。

> 备注：掌握这些省略规则后，请对应地回顾本章开头的多线程案例。

**可推导即可省略**

Lambda强调的是“做什么”而不是“怎么做”，所以凡是可以根据上下文推导得知的信息，都可以省略。例如上例还可以使用Lambda的省略写法：

```java
Runnable接口简化:
1. () -> System.out.println("多线程任务执行！")
Comparator接口简化:
2. Arrays.sort(array, (a, b) -> a.getAge() - b.getAge());
```

## 8.2 Lambda的前提条件

Lambda的语法非常简洁，完全没有面向对象复杂的束缚。但是使用时有几个问题需要特别注意：

1. 使用Lambda必须具有接口，且要求**接口中有且仅有一个抽象方法**。
   无论是JDK内置的`Runnable`、`Comparator`接口还是自定义的接口，只有当接口中的抽象方法存在且唯一时，才可以使用Lambda。
2. 使用Lambda必须具有**上下文推断**。
   也就是方法的参数或局部变量类型必须为Lambda对应的接口类型，才能使用Lambda作为该接口的实例。

> 备注：有且仅有一个抽象方法的接口，称为“**函数式接口**”。



# 第九章常见算法

## 9.1 排序概述

- 另外一种排序的方式，每一次比较完毕之后，本次循环中最大的数字就跑到右边去了

## 9.3 代码实现

```java

```

# 第十章 二分查找

## 10.1 普通查找和二分查找

**普通查找**

原理：遍历数组，获取每一个元素，然后判断当前遍历的元素是否和要查找的元素相同，如果相同就返回该元素的索引。如果没有找到，就返回一个负数作为标识(一般是-1)

**二分查找**

原理: 每一次都去获取数组的中间索引所对应的元素，然后和要查找的元素进行比对，如果相同就返回索引；

如果不相同，就比较中间元素和要查找的元素的值；

如果中间元素的值大于要查找的元素，说明要查找的元素在左侧，那么就从左侧按照上述思想继续查询(忽略右侧数据)；

如果中间元素的值小于要查找的元素，说明要查找的元素在右侧，那么就从右侧按照上述思想继续查询(忽略左侧数据)；

**二分查找对数组是有要求的,数组必须已经排好序**

## 10.2 二分查找图解

假设有一个给定有序数组(10,14,21,38,45,47,53,81,87,99),要查找50出现的索引

则查询过程如下图所示:

![5](/imgs/7.png)

## 10.3 二分查找代码实现

```java
public static void main(String[] args) {
    int[] arr = {10, 14, 21, 38, 45, 47, 53, 81, 87, 99};
    int index = binarySerach(arr, 38);
    System.out.println(index);
}
/**
 * 二分查找方法
 * @param arr 查找的目标数组
 * @param number 查找的目标值
 * @return 找到的索引,如果没有找到返回-1
 */
public static int binarySerach(int[] arr, int number) {
    int start = 0;
    int end = arr.length - 1;

    while (start <= end) {
        int mid = (start + end) / 2;
        if (number == arr[mid]) {
            return mid + 1;
        } else if (number < arr[mid]) {
            end = mid - 1;
        } else if (number > arr[mid]) {
            start = mid + 1;
        }
    }
    return -1;  //如果数组中有这个元素，则返回
}
```


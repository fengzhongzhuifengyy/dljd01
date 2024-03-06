# day08

# 第一章 日志

## 1.1 作用：

​	记录程序在运行过程中的点点滴滴。

## 1.2 使用步骤：

​	不是java的，也不是自己写的，是第三方提供的代码。

* 把第三方的代码导入到当前的项目当中

  新建lib文件夹，把jar粘贴到lib文件夹当中，全选后右键点击选择add as a ....

  检测导入成功：导入成功后jar包可以展开。在项目重构界面可以看到导入的内容

* 把配置文件粘贴到src文件夹下

* 在代码中获取日志对象

* 调用方法打印日志

## 1.3 日志级别

```
TRACE, DEBUG, INFO, WARN, ERROR
```

还有两个特殊的：

​	ALL：输出所有日志

​	OFF：关闭所有日志

日志级别从小到大的关系：

​	TRACE < DEBUG < INFO < WARN < ERROR

## 1.4 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!--
        CONSOLE ：表示当前的日志信息是可以输出到控制台的。
    -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <!--输出流对象 默认 System.out 改为 System.err-->
        <target>System.out</target>
        <encoder>
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度
                %msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%-5level]  %c [%thread] : %msg%n</pattern>
        </encoder>
    </appender>

    <!-- File是输出的方向通向文件的 -->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <!--日志输出路径-->
        <file>C:/code/itheima-data.log</file>
        <!--指定日志文件拆分和压缩规则-->
        <rollingPolicy
                       class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <!--通过指定压缩文件名称，来确定分割文件方式-->
            <fileNamePattern>C:/code/itheima-data2-%d{yyyy-MMdd}.log%i.gz</fileNamePattern>
            <!--文件拆分大小-->
            <maxFileSize>1MB</maxFileSize>
        </rollingPolicy>
    </appender>

    <!--

    level:用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF
   ， 默认debug
    <root>可以包含零个或多个<appender-ref>元素，标识这个输出位置将会被本日志级别控制。
    -->
    <root level="info">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE" />
    </root>
</configuration>
```

# 第二章 File类

## 2.1 概述

`java.io.File` 类是文件和目录路径名的抽象表示，主要用于文件和目录的创建、查找和删除等操作。

## 2.2 构造方法

- `public File(String pathname) ` ：通过将给定的**路径名字符串**转换为抽象路径名来创建新的 File实例。  
- `public File(String parent, String child) ` ：从**父路径名字符串和子路径名字符串**创建新的 File实例。
- `public File(File parent, String child)` ：从**父抽象路径名和子路径名字符串**创建新的 File实例。  

- 构造举例，代码如下：

```java
// 文件路径名
String pathname = "D:\\aaa.txt";
File file1 = new File(pathname); 

// 文件路径名
String pathname2 = "D:\\aaa\\bbb.txt";
File file2 = new File(pathname2); 

// 通过父路径和子路径字符串
 String parent = "d:\\aaa";
 String child = "bbb.txt";
 File file3 = new File(parent, child);

// 通过父级File对象和子路径字符串
File parentDir = new File("d:\\aaa");
String child = "bbb.txt";
File file4 = new File(parentDir, child);
```

> 小贴士：
>
> 1. 一个File对象代表硬盘中实际存在的一个文件或者目录。
> 2. 无论该路径下是否存在文件或者目录，都不影响File对象的创建。

## 2.3 常用方法

### 获取功能的方法

- `public String getAbsolutePath() ` ：返回此File的绝对路径名字符串。

- ` public String getPath() ` ：将此File转换为路径名字符串。 

- `public String getName()`  ：返回由此File表示的文件或目录的名称。  

- `public long length()`  ：返回由此File表示的文件的长度。 

  方法演示，代码如下：

  ```java
  public class FileGet {
      public static void main(String[] args) {
          File f = new File("d:/aaa/bbb.java");     
          System.out.println("文件绝对路径:"+f.getAbsolutePath());
          System.out.println("文件构造路径:"+f.getPath());
          System.out.println("文件名称:"+f.getName());
          System.out.println("文件长度:"+f.length()+"字节");
  
          File f2 = new File("d:/aaa");     
          System.out.println("目录绝对路径:"+f2.getAbsolutePath());
          System.out.println("目录构造路径:"+f2.getPath());
          System.out.println("目录名称:"+f2.getName());
          System.out.println("目录长度:"+f2.length());
      }
  }
  输出结果：
  文件绝对路径:d:\aaa\bbb.java
  文件构造路径:d:\aaa\bbb.java
  文件名称:bbb.java
  文件长度:636字节
  
  目录绝对路径:d:\aaa
  目录构造路径:d:\aaa
  目录名称:aaa
  目录长度:4096
  ```

> API中说明：length()，表示文件的长度。但是File对象表示目录，则返回值未指定。

### 绝对路径和相对路径

- **绝对路径**：从盘符开始的路径，这是一个完整的路径。
- **相对路径**：相对于项目目录的路径，这是一个便捷的路径，开发中经常使用。

```java
public class FilePath {
    public static void main(String[] args) {
      	// D盘下的bbb.java文件
        File f = new File("D:\\bbb.java");
        System.out.println(f.getAbsolutePath());
      	
		// 项目下的bbb.java文件
        File f2 = new File("bbb.java");
        System.out.println(f2.getAbsolutePath());
    }
}
输出结果：
D:\bbb.java
D:\idea_project_test4\bbb.java
```

### 判断功能的方法

- `public boolean exists()` ：此File表示的文件或目录是否实际存在。
- `public boolean isDirectory()` ：此File表示的是否为目录。
- `public boolean isFile()` ：此File表示的是否为文件。

方法演示，代码如下：

```java
public class FileIs {
    public static void main(String[] args) {
        File f = new File("d:\\aaa\\bbb.java");
        File f2 = new File("d:\\aaa");
      	// 判断是否存在
        System.out.println("d:\\aaa\\bbb.java 是否存在:"+f.exists());
        System.out.println("d:\\aaa 是否存在:"+f2.exists());
      	// 判断是文件还是目录
        System.out.println("d:\\aaa 文件?:"+f2.isFile());
        System.out.println("d:\\aaa 目录?:"+f2.isDirectory());
    }
}
输出结果：
d:\aaa\bbb.java 是否存在:true
d:\aaa 是否存在:true
d:\aaa 文件?:false
d:\aaa 目录?:true
```

### 创建删除功能的方法

- `public boolean createNewFile()` ：当且仅当具有该名称的文件尚不存在时，创建一个新的空文件。 
- `public boolean delete()` ：删除由此File表示的文件或目录。  
- `public boolean mkdir()` ：创建由此File表示的目录。
- `public boolean mkdirs()` ：创建由此File表示的目录，包括任何必需但不存在的父目录。

方法演示，代码如下：

```java
public class FileCreateDelete {
    public static void main(String[] args) throws IOException {
        // 文件的创建
        File f = new File("aaa.txt");
        System.out.println("是否存在:"+f.exists()); // false
        System.out.println("是否创建:"+f.createNewFile()); // true
        System.out.println("是否存在:"+f.exists()); // true
		
     	// 目录的创建
      	File f2= new File("newDir");	
        System.out.println("是否存在:"+f2.exists());// false
        System.out.println("是否创建:"+f2.mkdir());	// true
        System.out.println("是否存在:"+f2.exists());// true

		// 创建多级目录
      	File f3= new File("newDira\\newDirb");
        System.out.println(f3.mkdir());// false
        File f4= new File("newDira\\newDirb");
        System.out.println(f4.mkdirs());// true
      
      	// 文件的删除
       	System.out.println(f.delete());// true
      
      	// 目录的删除
        System.out.println(f2.delete());// true
        System.out.println(f4.delete());// false
    }
}
```

> API中说明：delete方法，如果此File表示目录，则目录必须为空才能删除。

#### 扩展：

**细节1：**

​	createNewFile只能创建文件，即使调用者的File对象没有后缀名，创建的也是一个没有后缀的文件。

```java
File file = new File("C:\\Users\\moon\\Desktop\\aaa");
//创建了一个没有后缀的文件
System.out.println(file.createNewFile());
```

​	mkdir：只能创建一个单级的文件夹（以后就不用了）

​	mkdirs：可以创建单级的，也可以创建多级的文件夹。

​			如果调用者的File对象，有后缀的，创建出来的也是一个有后缀的文件夹。

```java
File file = new File("C:\\Users\\moon\\Desktop\\aaa.txt");
//创建了一个文件夹，文件名名字为aaa.txt
System.out.println(file.mkdirs());
```

​	**总结：**创建出来的东西，是文件还是文件夹，不看后缀，只看调用者的方法。

**细节2：**

​	如果在某一个文件夹中已经有了一个叫做aaa.txt的文件夹。那么我们无法再次创建一个aaa.txt的文件。因为这两者的路径是一样的，不管是在windows系统中，还是在Java中，路径一定只能是唯一的。

```java
File file = new File("C:\\Users\\moon\\Desktop\\aaa.txt");
//创建了一个文件夹，文件名名字为aaa.txt
System.out.println(file.mkdirs());
//再次创建一个aaa.txt的文件失败，因为路径重复了
System.out.println(file.createNewFile());
```



## 2.4 目录的遍历

- `public String[] list()` ：返回一个String数组，表示该File目录中的所有子文件或目录。

- `public File[] listFiles()` ：返回一个File数组，表示该File目录中的所有的子文件或目录。  

```java
public class FileFor {
    public static void main(String[] args) {
        File dir = new File("d:\\java_code");
      
      	//获取当前目录下的文件以及文件夹的名称。
		String[] names = dir.list();
		for(String name : names){
			System.out.println(name);
		}
        
        //获取当前目录下的文件以及文件夹对象，只要拿到了文件对象，那么就可以获取更多信息
        File[] files = dir.listFiles();
        for (File file : files) {
            System.out.println(file);
        }
    }
}
```

> 小贴士：
>
> 调用listFiles方法的File对象，表示的必须是实际存在的目录，否则返回null，无法进行遍历。

**细节扩展：**

* 当调用者File表示的路径不存在时，返回null
* 当调用者File表示的路径是文件时，返回null
* 当调用者File表示的路径是一个空文件夹时，返回一个长度为0的数组
* 当调用者File表示的路径是一个有内容的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回
* 当调用者File表示的路径是一个有隐藏文件的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回，包含隐藏文件
* 当调用者File表示的路径是需要权限才能访问的文件夹时，返回null

# 第三章 黑马商城

## 3.1 整体结构：

整体的Map集合就是商城

商城里面有很多店铺

键：店铺的名称（字符串表示）

值：店铺的商品列表（ArrayList表示）

![黑马商场整体架构](.\assets\黑马商场整体架构.png)

## 3.2业务需求

完成以下逻辑：

* 查询所有商品
* 添加商品
* 删除商品
* 修改商品
* 查询单个商品
* 退出系统

代码示例:

```java
package com.itheima.a03shoppingsystem;

import java.util.*;

public class ShoppingSystem {

    //shoppingMap 表示整个的商城
    //键：店铺的名称
    //值：店铺所有的商品列表 ArrayList集合
    static HashMap<String, ArrayList<Goods>> shoppingMap = new HashMap<>();

    //因为静态代码块只执行一次。
    //作用：系统刚启动的时候进行初始化数据的。
    static {
        //添加第一个店铺
        ArrayList<Goods> list1 = new ArrayList<>();
        list1.add(new Goods("《数据库从删库到跑路》", 50.27, "图书", 50, "作者：高斯林", new Date()));
        list1.add(new Goods("《颈椎病康复指南》", 20.34, "图书", 20, "作者：XX教授", new Date()));
        list1.add(new Goods("《金瓶梅》", 120.91, "图书", 10, "作者：阿玮", new Date()));
        list1.add(new Goods("《富婆通讯录》", 200, "图书", 5, "作者：张三", new Date()));
        shoppingMap.put("黑马图书专卖", list1);

        //添加第二个店铺
        ArrayList<Goods> list2 = new ArrayList<>();
        list2.add(new Goods("《霸王防脱洗发露》", 10.27, "生活商品", 100, "越洗越脱", new Date()));
        list2.add(new Goods("《生姜生发指南》", 20.14, "生活商品", 20, "越擦越亮", new Date()));
        list2.add(new Goods("《富婆快乐球》", 120.91, "生活商品", 5, "越擦越快乐", new Date()));
        list2.add(new Goods("《奥特曼泡酒》", 1200, "生活商品", 150, "越喝越上头", new Date()));
        shoppingMap.put("黑马生活专卖", list2);
    }

    public static void main(String[] args) {
        while (true) {
            System.out.println("====================欢迎进入黑马商场管理系统=========================");
            System.out.println("1.查看全部商品信息");
            System.out.println("2.上架商品信息");
            System.out.println("3.下架商品信息");
            System.out.println("4.修改商品信息");
            System.out.println("5.查询单个的商品信息");
            System.out.println("6.退出系统");
            Scanner sc = new Scanner(System.in);
            System.out.println("请您输入命令:");
            String choose = sc.nextLine();
            switch (choose) {
                case "1" -> printGoodsInfo();
                case "2" -> addGoods();
                case "3" -> deleteGoods();
                case "4" -> updateGoos();
                case "5" -> queryGood();
                case "6" -> {
                    System.out.println("退出系统");
                    System.exit(0);
                }
                default -> {
                    System.out.println("没有这种选择");
                }
            }
        }
    }

    private static void queryGood() {
        //这一段跟删除的前半端代码是一样的
        //1.先键盘录入店铺的名称
        //存在 --- 拿到对应的ArrayList集合
        //不存在 --- 重新输入店铺的名称


        //2.输入要查询的商品名称
        //到ArrayList集合中查询对应的索引 findIndex
        //得到一个商品对象，最后再进行打印


    }

    private static void updateGoos() {
        //这一段跟删除的前半端代码是一样的
        //1.先键盘录入店铺的名称
        //存在 --- 拿到对应的ArrayList集合
        //不存在 --- 重新输入店铺的名称


        //2.输入要修改的商品
        //跟商品添加的代码是一样的。
        //。。。
        //。。。
        //。。。
        //。。。

    }

    private static void deleteGoods() {
        System.out.println("=============商品下架页面=====================");
        Scanner sc = new Scanner(System.in);
        ArrayList<Goods> goodsList;
        while (true) {
            System.out.println("请输入店铺名称");
            String shoppingName = sc.nextLine();
            //判断shoppingName是否存在
            if (shoppingMap.containsKey(shoppingName)) {
                //存在
                //找到店铺所对应的ArrayList集合
                goodsList = shoppingMap.get(shoppingName);
                break;
            } else {
                //不存在
                //如果不存在，直接提示并重新输入
                System.out.println("当前店铺" + shoppingName + "不存在，请重新输入");
                continue;
            }
        }

        //当代码执行到这里，表示已经拿到了店铺的名称和对应的ArrayList
        //再次让用户输入要删除的商品，并进行删除
        while (true) {
            System.out.println("请输入要删除的商品名称");
            String name = sc.nextLine();
            int index = findIndex(goodsList, name);
            if(index >= 0){
                //存在
                goodsList.remove(index);
                System.out.println("删除成功");
                break;
            }else{
                //不存在
                System.out.println("要删除的商品不存在");
                continue;
            }
        }


    }

    public static void addGoods() {
        System.out.println("=============商品上架页面=====================");
        Scanner sc = new Scanner(System.in);
        ArrayList<Goods> list;
        String shoppingName;
        while (true) {
            System.out.println("请输入店铺的名称");
            shoppingName = sc.nextLine();
            if (shoppingMap.containsKey(shoppingName)) {
                //存在
                list = shoppingMap.get(shoppingName);
                break;
            } else {
                //不存在
                System.out.println("是否需要重新输入店铺的名称Y/N");
                String choose = sc.nextLine();
                if ("Y".equalsIgnoreCase(choose)) {
                    System.out.println("用户手误，此时重新输入");
                    continue;
                } else if ("N".equalsIgnoreCase(choose)) {
                    System.out.println("当前店铺不存在，新建一个新的店铺");
                    list = new ArrayList<>();
                    break;
                } else {
                    System.out.println("你是来捣乱的吧");
                    continue;
                }
            }
        }
        //拿到ArrayList再添加商品信息
        //创建一个商品的对象，把所有商品的信息添加到商品对象当中
        Goods goods = new Goods();

        while (true) {
            //校验商品名称的格式
            System.out.println("请输入商品的名称");
            String name = sc.nextLine();
            try {
                goods.setName(name);
            } catch (GoodsNameException e) {
                System.out.println(name + "名称格式有误");
                continue;
            }

            //校验商品名称是否重复
            boolean flag = contains(list, name);
            if (flag) {
                //已存在
                System.out.println("当前商品" + name + "已经存在");
                continue;
            } else {
                //不存在 --- 可以使用
                break;
            }
        }

        //键盘录入价格并校验
        while (true) {//abc
            try {
                System.out.println("请输入商品的价格");
                String priceStr = sc.nextLine();
                double price = Double.parseDouble(priceStr);
                goods.setPrice(price);
                break;
            } catch (NumberFormatException e) {
                System.out.println("商品的价格格式有误，请输入小数");
                continue;
            } catch (PriceoutofboundsException e) {
                System.out.println("商品的价格一定要为正数");
                continue;
            }
        }

        //键盘录入商品的类型
        while (true) {
            System.out.println("请输入商品的类型");
            String type = sc.nextLine();
            try {
                goods.setType(type);
                break;
            } catch (TypeException e) {
                System.out.println("商品的类型不能为空");
                continue;
            }
        }

        //键盘录入商品的数量
        while (true) {
            System.out.println("请输入商品的数量");
            try {
                String countrStr = sc.nextLine();
                int count = Integer.parseInt(countrStr);
                goods.setCount(count);
                break;
            } catch (NumberFormatException e) {
                System.out.println("商品数量格式有误，请输入数字");
                continue;
            } catch (CountException e) {
                System.out.println("商品数量必须要大于等于1");
                continue;
            }
        }

        //键盘录入商品的描述
        System.out.println("请输入商品的描述");
        String type = sc.nextLine();
        goods.setDesc(type);

        //上架时间不需要键盘录入，采取默认即可
        Date date = new Date();
        goods.setTime(date);
        //把商品对象添加到集合当中
        list.add(goods);
        //把店铺名称 和 对应的 ArrayList集合再添加到Map当中
        shoppingMap.put(shoppingName, list);

        //提示
        System.out.println("商品:" + goods.getName() + "添加成功");

    }

    //判断name在list中是否存在
    private static boolean contains(ArrayList<Goods> list, String name) {
        return findIndex(list, name) >= 0;
    }

    //查找name在list中的索引
    private static int findIndex(ArrayList<Goods> list, String name) {
        for (int i = 0; i < list.size(); i++) {
            Goods goods = list.get(i);
            if (goods.getName().equals(name)) {
                return i;
            }
        }
        return -1;
    }

    public static void printGoodsInfo() {
        //如果map集合中不存在数据，需要有对应的提示
        if (shoppingMap.isEmpty()) {
            System.out.println("没有任何商品信息，请添加后再查询");
            return;
        }
        //当代码执行到这一行，表示map集合中一定是存在数据
        //遍历map集合
        System.out.println("=============商品展示页面=====================");
        Set<String> keys = shoppingMap.keySet();
        for (String key : keys) {
            System.out.println("店铺：" + key);
            //通过键（店铺名称）去获取值（店铺里面所有的商品信息）
            ArrayList<Goods> goodsList = shoppingMap.get(key);
            for (Goods goods : goodsList) {
                System.out.println("\t\t" + "商品名称：" + goods.getName());
                System.out.println("\t\t" + "商品价格：" + goods.getPrice());
                System.out.println("\t\t" + "商品类型：" + goods.getType());
                System.out.println("\t\t" + "商品库存：" + goods.getCount());
                System.out.println("\t\t" + "商品描述：" + goods.getDesc());
                System.out.println("\t\t" + "商品上架时间：" + goods.getTime());
                System.out.println("--------------------------");
            }
        }
    }
}
```

```java
package com.itheima.a03shoppingsystem;

import java.text.SimpleDateFormat;
import java.util.Date;

public class Goods {
    private String name;   //商品名称
    private double price; //商品价格
    private String type;   //商品类型
    private int count;   //商品库存
    private String desc;   //商品描述
    private Date time;  // 上架时间


    public Goods() {
    }

    public Goods(String name, double price, String type, int count, String desc, Date time) {
        this.name = name;
        this.price = price;
        this.type = type;
        this.count = count;
        this.desc = desc;
        this.time = time;
    }

    /**
     * 获取
     *
     * @return name
     */
    public String getName() {
        return name;
    }

    /**
     * 设置
     *
     * @param name
     */
    public void setName(String name) {
        if (name.matches("《.{2,10}》")) {
            this.name = name;
        } else {
            //告诉调用者这里有问题
            throw new GoodsNameException(name + "格式有误");
        }
    }

    /**
     * 获取
     *
     * @return price
     */
    public double getPrice() {
        return price;
    }

    /**
     * 设置
     *
     * @param price
     */
    public void setPrice(double price) {
        if (price > 0) {
            this.price = price;
        } else {
            throw new PriceoutofboundsException(price + "超出了范围");
        }

    }

    /**
     * 获取
     *
     * @return type
     */
    public String getType() {
        return type;
    }

    /**
     * 设置
     *
     * @param type
     */
    public void setType(String type) {
        if(type.length() != 0){
            this.type = type;
        }else{
            //没有内容
            throw new TypeException(type + "的内容不能为空");
        }

    }

    /**
     * 获取
     *
     * @return count
     */
    public int getCount() {
        return count;
    }

    /**
     * 设置
     *
     * @param count
     */
    public void setCount(int count) {
        if(count >= 1){
            this.count = count;
        }else{
            throw new CountException(count + "的数量一定要大于等于1");
        }

    }

    /**
     * 获取
     *
     * @return desc
     */
    public String getDesc() {
        return desc;
    }

    /**
     * 设置
     *
     * @param desc
     */
    public void setDesc(String desc) {
        if(desc.length() == 0){
            this.desc = "这个家伙很懒，没有添加内容";
        }else{
            this.desc = desc;
        }
    }

    /**
     * 获取
     *
     * @return time
     */
    public String getTime() {
        //按照指定的格式进行转化
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss EEE a");
        String format = sdf.format(time);
        return format;
    }

    /**
     * 设置
     *
     * @param time
     */
    public void setTime(Date time) {
        this.time = time;
    }

    public String toString() {
        return "Goods{name = " + name + ", price = " + price + ", type = " + type + ", count = " + count + ", desc = " + desc + ", time = " + getTime() + "}";
    }
}
```

以下为自定义异常类：

```java
public class CountException extends RuntimeException{

    public CountException() {
    }

    public CountException(String message) {
        super(message);
    }
}
```

```java
public class GoodsNameException extends RuntimeException{

    public GoodsNameException() {
    }

    public GoodsNameException(String message) {
        super(message);
    }
}
```

```java
public class PriceoutofboundsException extends RuntimeException{
    public PriceoutofboundsException() {
    }

    public PriceoutofboundsException(String message) {
        super(message);
    }
}
```

```java
public class TypeException extends RuntimeException{

    public TypeException() {
    }

    public TypeException(String message) {
        super(message);
    }
}
```




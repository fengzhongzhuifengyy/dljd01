## 1. 循环高级

### 1.1 无限循环

for、while、do...while都有无限循环的写法。

最为常用的是while格式的。

因为无限循环是不知道循环次数的，所以用while格式的

代码示例：

```java
while(true){
    
}
```



### 1.2  跳转控制语句（掌握）

* 跳转控制语句（break）
  * 跳出循环，结束循环
* 跳转控制语句（continue）
  * 跳过本次循环，继续下次循环
* 注意： continue只能在循环中进行使用！

#### 练习1：逢7过

需求：

​	朋友聚会的时候可能会玩一个游戏：逢7过 

​	游戏规则：从任意一个数字开始报数，当你要报的数字是包含7或者是7的倍数时都要说过：过

​	需求：使用程序在控制台打印出1-100之间的满足逢七必过规则的数据

代码示例：

```java
public class LoopDemo4 {
    public static void main(String[] args) {
        //目标：听懂并且自己独立写出来
        //核心：
        //不报数字的条件：
        //个位是7   十位是7  7的倍数
        //三个条件只要满足一个，就不报数字

        //游戏的规则
        //1 2 3 4 5 6 过 8 9 10 11 12 13 过 15 16 过 18 19 20 过...

        //循环的开始条件：1
        //结束条件：100
        for (int i = 1; i <= 100; i++) {
            //i依次表示这个范围之内的每一个数字
            if (i % 10 == 7 || i / 10 % 10 == 7 || i % 7 == 0) {
                System.out.println("过");
                continue;
            }
            System.out.println(i);
        }

    }
}

```

#### 练习2：求平方根 

需求：

​	键盘录入一个大于等于2的整数 x ，计算并返回 x 的 平方根 。

​	结果只保留整数部分 ，小数部分将被舍去 。

代码示例：

```java
public class LoopDemo5 {
    public static void main(String[] args) {
        //目标：听懂思路并且自己独立写出来

        //键盘录入10
        // 1 * 1 = 1 < 10
        // 2 * 2 = 4 < 10
        // 3 * 3 = 9 < 10
        // 4 * 4 = 16 > 10
        //说明：10的平方根在3~4之间。保留整数之后：3


        //键盘录入20
        //1 * 1 = 1 < 20
        // 2 * 2 = 4 < 20
        // 3 * 3 = 9 < 20
        // 4 * 4 = 16 < 20
        // 5 * 5 = 25 > 20
        //说明：20的平方根在4~5之间，保留整数之后：4

        //键盘录入40
        //1 * 1 = 1 < 40
        // 2 * 2 = 4 < 40
        // 3 * 3 = 9 < 40
        // 4 * 4 = 16 < 40
        // 5 * 5 = 25 < 40
        // 6 * 6 = 36 < 40
        // 7 * 7 = 49 > 40
        //说明：40的平方根在6~7之间，保留整数之后：6

        //思路：
        //从1开始循环，拿着循环得到的每一个数求平方。
        //拿着结果，跟键盘录入的数字比较。
        //如果平方之后的结果 == 录入的数字    表示当前数字就是我们题目的结果
        //如果平方之后的结果 <  录入的数字    循环继续
        //如果平方之后的结果 >  录入的数字    表示前一个数字就是我们题目的结果
        //循环的开始条件：1
        //循环的结束条件：键盘录入的数字

        //1.键盘录入数字
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个整数");
        int number = sc.nextInt();
        //2.循环找结果
        for (int i = 1; i <= number; i++) {
            //循环体
            if (i * i == number) {
                System.out.println(number + "的平方根为" + i);
                //如果已经找到了，那么后面就不需要再运行了，循环直接停止
                break;
            }else if(i * i > number){
                System.out.println(number + "的平方根为" + (i - 1));
                //如果已经找到了，那么后面就不需要再运行了，循环直接停止
                break;
            }
        }


    }
}
```

#### 练习3：求质数

需求：

​	键盘录入一个正整数 x ，判断该整数是否为一个质数。  

代码示例：

```java
public class LoopDemo6 {
    public static void main(String[] args) {
        //目标：听懂思路并且自己独立写出来

        //质数：只能被1和本身整除的数。
        //7 是一个质数
        //13 是一个质数
        //8 不是一个质数
        //25 不是一个质数

        //1.键盘录入一个正整数
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入一个整数");
        int number = sc.nextInt();
        //2.利用循环进行判断
        //思路一：从1开始到本身number结束，在这个范围之内，有多少个数字能被键盘录入的number整除
        //如果只有两个，那么表示number是一个质数。

        //思路二：从2开始到number-1结束，在这个范围之内，有多少个数字能被number整除
        //如果没有数字，那么表示number是一个质数。

        //思路二循环的开始条件：2
        //循环的结束条件：number-1
        int count = 0;
        for (int i = 2; i < number; i++) {
            //i依次表示，2~ number-1 这个范围之内的每一个数字
            //要求的是，在这个范围之内，有多少个数字能被number整除
            if(number % i == 0){
                count++;
            }
        }
        //当循环结束之后，表示我们已经找到了，在2 ~ number-1 这个范围之内，有多少个数字能被number整除
        if(count == 0){
            System.out.println(number + "是一个质数");
        }else{
            System.out.println(number + "不是一个质数");
        }
    }
}
```

## 2. Random

####  Random产生随机数（掌握）

* 概述：

  * Random类似Scanner，也是Java提供好的API，内部提供了产生随机数的功能
    * API后续课程详细讲解，现在可以简单理解为Java已经写好的代码

* 使用步骤：

  1. 导入包

     import java.util.Random;

  2. 创建对象

     Random r = new Random();

  3. 产生随机数

     int num = r.nextInt(10);

     解释： 10代表的是一个范围，如果括号写10，产生的随机数就是0-9，括号写20，参数的随机数则是0-19
     
     扩展点：如果第三步没有指定范围，那么会在int的取值范围中获取一个随机数。

* 示例代码：

```java
import java.util.Random;
public class RandomDemo {
	public static void main(String[] args) {
		//创建对象
		Random r = new Random();
		//用循环获取10个随机数
		for(int i=0; i<10; i++) {
			//获取随机数
			int number = r.nextInt(10);
			System.out.println("number:" + number);
		}
		//需求：获取一个1-100之间的随机数
		int x = r.nextInt(100) + 1;
		System.out.println(x);
	}
}
```

#### Random练习-猜数字小游戏

* 需求：

  程序自动生成一个1-100之间的数字，使用程序实现猜出这个数字是多少？

  当猜错的时候根据不同情况给出相应的提示

  A. 如果猜的数字比真实数字大，提示你猜的数据大了

  B. 如果猜的数字比真实数字小，提示你猜的数据小了

  C. 如果猜的数字与真实数字相等，提示恭喜你猜中了

* 示例代码：

```java
import java.util.Random;
import java.util.Scanner;

public class RandomTest {
	public static void main(String[] args) {
		//要完成猜数字的游戏，首先需要有一个要猜的数字，使用随机数生成该数字，范围1到100
		Random r = new Random();
		int number = r.nextInt(100) + 1;
		
		while(true) {
			//使用程序实现猜数字，每次均要输入猜测的数字值，需要使用键盘录入实现
			Scanner sc = new Scanner(System.in);
			
			System.out.println("请输入你要猜的数字：");
			int guessNumber = sc.nextInt();
			
			//比较输入的数字和系统产生的数据，需要使用分支语句。
             //这里使用if..else..if..格式，根据不同情况进行猜测结果显示
			if(guessNumber > number) {
				System.out.println("你猜的数字" + guessNumber + "大了");
			} else if(guessNumber < number) {
				System.out.println("你猜的数字" + guessNumber + "小了");
			} else {
				System.out.println("恭喜你猜中了");
				break;
			}
		}
		
	}
}
```

#### 猜数字小游戏扩展1:（掌握）

​	生成随机数的代码不能写在循环里面。

​	否则，每猜一次，都会产生一个新的随机数。

#### 猜数字小游戏扩展2:（了解）

​	很多游戏中，都会有保底机制。

​	假设，在猜数字小游戏中，三次猜不中就触发保底机制

代码示例：

```java
import java.util.Random;
import java.util.Scanner;

public class RandomDemo2 {
    public static void main(String[] args) {
        //分析：
        //1.生成一个1-100之间的随机数字
        Random r = new Random();
        //扩展1：生成随机数的代码，不能写在循环里面，否则每一次都会生成一个新的随机数
        //扩展2：保底机制3次
        
        //定义变量记录猜的次数
        int count = 0;
        int number = r.nextInt(100) + 1;
        System.out.println(number);
        
        //2.使用键盘录入的方式去猜
        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("请输入你要猜的数字");
            int guessNumber = sc.nextInt();
            
            
            //每猜一次，count就++
            count++;
            //判断当前是第几次
            //如果达到了保底，直接提示猜中了
            if(count == 3){
                System.out.println("猜中了");
                break;
            }
           
            
            //3.拿着键盘输入的数字 跟 随机数比较判断
            if (guessNumber > number) {
                System.out.println("大了");
            } else if (guessNumber < number) {
                System.out.println("小了");
            } else {
                System.out.println("猜中了");
                break;
            }
        }

    }
}

```

## 3.数组

### 3.1什么是数组

​	数组就是存储数据长度固定的容器，存储多个数据的数据类型要一致。 

### 3.2数组定义格式

#### 3.2.1第一种（常用）

​	数据类型[] 数组名

​	示例：

```java
int[] arr;        
double[] arr;      
char[] arr;
```

#### 3.2.2第二种

​	数据类型 数组名[]

​	示例：

```java
int arr[];
double arr[];
char arr[];
```

### 3.3数组静态初始化

#### 3.3.1什么是静态初始化

​	在创建数组时，直接将元素确定	

#### 3.3.2静态初始化格式

- 完整版格式

  ```java
  数据类型[] 数组名 = new 数据类型[]{元素1,元素2,...};
  举例：
  int[] arr = new int[]{1,2,3,4};
  ```

- 简化版格式

  ```java
  数据类型[] 数组名 = {元素1,元素2,...};
  举例：
  int[] arr = {1,2,3,4};
  ```

#### 3.3.3静态初始化格式详解

- 等号左边：
  - int:数组的数据类型
  - []:代表这是一个数组
  - arr:代表数组的名称
- 等号右边：
  - new:为数组开辟内存空间
  - int:数组的数据类型
  - []:代表这是一个数组
  - {}:表示数组里面存储的所有元素

#### 3.4数组元素访问

#### 3.4.1什么是索引

​	每一个存储到数组的元素，都会自动的拥有一个编号，从0开始。

​	这个自动编号称为数组索引(index)，可以通过数组的索引访问到数组中的元素。 	

#### 3.4.2访问数组元素格式

```java
数组名[索引];
```

#### **3.4.3索引的作用：**

* 利用索引可以获取数组中的元素
* 利用索引可以把数据存储到数组中

#### 3.4.4示例代码

```java
public class ArrayDemo {
    public static void main(String[] args) {
        int[] arr = new int[3];

        //输出数组名
        System.out.println(arr); //[I@880ec60

        //输出数组中的元素
        System.out.println(arr[0]);
        System.out.println(arr[1]);
        System.out.println(arr[2]);
    }
}
```

#### 练习1：数组的遍历

遍历：把数组里面所有的元素都一一获取出来

代码示例：

```java
 //1.定义数组
        int[] arr = {1,2,3,4,5};
        //2.遍历数组
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
        System.out.println("---------------快速生成最终代码--------------------");
        //数组名.fori
```

#### 练习2：累加求和

需求：

​	定义一个数组，存储1,2,3,4,5

​	遍历数组得到每一个元素，求数组里面所有的数据和

代码示例：

```java
//1.定义数组
int[] arr = {1,2,3,4,5};

//2.定义一个累加变量sum
int sum = 0;

//3.遍历数组得到数组里面的每一个元素
for (int i = 0; i < arr.length; i++) {
    //i 依次表示数组中的每一个索引
    //arr[i] 依次表示数组中的每一个元素
    //System.out.println(arr[i]);
    sum = sum + arr[i];
}

//4.当循环结束之后，表示所有的数据都已经累加成功
System.out.println(sum);
```

#### 练习3：统计个数

需求：

​	定义一个数组，存储1,2,3,4,5,6,7,8,9,10
 	遍历数组得到每一个元素，统计数组里面一共有多少个能被3整除的数字

代码示例：

```java
//1.定义数组1,2,3,4,5,6,7,8,9,10
int[] arr = {1,2,3,4,5,6,7,8,9,10};
//2.定义统计变量
int count = 0;
//3.遍历数组得到每一个元素
for (int i = 0; i < arr.length; i++) {
    //3.判断当前元素是否满足
    if(arr[i] % 3 == 0){
        count++;
    }
}
//4.循环结束之后，就表示数组里面所有的元素都已经统计完毕
System.out.println(count);
```

#### 练习4：变化数据

需求：

​	定义一个数组，存储1,2,3,4,5,6,7,8,9,10

​	遍历数组得到每一个元素。

要求：

​	1，如果是奇数，则将当前数字扩大两倍

​	2，如果是偶数，则将当前数字变成二分之一

代码示例：

```java
public class ArrDemo7 {
    public static void main(String[] args) {
        /*
        定义一个数组，存储1,2,3,4,5,6,7,8,9,10
        遍历数组得到每一个元素。
        要求：
        1，如果是奇数，则将当前数字扩大两倍
        2，如果是偶数，则将当前数字变成二分之一
        */
        // 1  2  3  4  5  6  7   8  9   10
        // 2  1  6  2  10 3  14  4  18  5
        //1.定义数组
        int[] arr = {1,2,3,4,5,6,7,8,9,10};
        //2.遍历数组得到每一个元素
        for (int i = 0; i < arr.length; i++) {
            //3.判断每一个元素是奇数还是偶数
            if(arr[i] % 2 == 0){
                // 偶数，则将当前数字变成二分之一
                arr[i] = arr[i] / 2;
            }else{
                // 奇数，则将当前数字扩大两倍
                arr[i] = arr[i] * 2;
            }
        }
        //3.当循环结束之后，数组里面所有的数据都已经改变完毕
        //再遍历验证一下即可
        //一个循环只干一件事情。
        //如果想要遍历，再写一个循环即可
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

### 3.5数组动态初始化

#### 3.5.1什么是动态初始化

​	数组动态初始化就是只给定数组的长度，由系统给出默认初始化值。

#### 3.5.2动态初始化格式

```java
数据类型[] 数组名 = new 数据类型[数组长度];
```

```java
int[] arr = new int[3];
```

#### 3.5.3动态初始化格式详解

- 等号左边：
  - int:数组的数据类型
  - []:代表这是一个数组
  - arr:代表数组的名称
- 等号右边：
  - new:为数组开辟内存空间
  - int:数组的数据类型
  - []:代表这是一个数组
  - 5:代表数组的长度

### 3.6数组操作的两个常见小问题

#### 3.6.1索引越界异常

- 出现原因

  ```java
  public class ArrayDemo {
      public static void main(String[] args) {
          int[] arr = new int[3];
          System.out.println(arr[3]);
      }
  }
  ```

  数组长度为3，索引范围是0~2，但是我们却访问了一个3的索引。

  程序运行后，将会抛出ArrayIndexOutOfBoundsException 数组越界异常。在开发中，数组的越界异常是不能出现的，一旦出现了，就必须要修改我们编写的代码。 

- 解决方案

  将错误的索引修改为正确的索引范围即可！

#### 3.6.2空指针异常

- 出现原因

  ```java
  public class ArrayDemo {
      public static void main(String[] args) {
          int[] arr = new int[3];
  
          //把null赋值给数组
          arr = null;
          System.out.println(arr[0]);
      }
  }
  ```

  arr = null 这行代码，意味着变量arr将不会再保存数组的内存地址，也就不允许再操作数组了，因此运行的时候会抛出 NullPointerException 空指针异常。在开发中，数组的越界异常是不能出现的，一旦出现了，就必须要修改我们编写的代码。

- 解决方案

  给数组一个真正的堆内存空间引用即可！

### 3.7数组的练习

#### 3.7.1数组最值

- 最大值获取：从数组的所有元素中找出最大值。

- 实现思路：

  - 定义变量，保存数组0索引上的元素
  - 遍历数组，获取出数组中的每个元素
  - 将遍历到的元素和保存数组0索引上值的变量进行比较
  - 如果数组元素的值大于了变量的值，变量记录住新的值
  - 数组循环遍历结束，变量保存的就是数组中的最大值 

- 代码实现：

  ```java
  //扩展1：max的值能不能写0？最好不要写0。
  //      一般都会赋值数组中的某个元素
  
  //扩展2：循环从0开始，或者循环从1开始，对结果有没有影响？
  //       没有
  //      如果循环从0开始，那么多了一次，自己跟自己比的过程。
  //      对结果没有任何影响，只不过多循环了一次而已。
  
  
  //1.定义数组存储元素
  int[] arr = {33,5,22,44,55};
  //2.定义一个变量max
  int max = arr[0];
  //3.遍历数组
  for (int i = 1; i < arr.length; i++) {
      // i 索引  arr[i] 元素
      if(arr[i] > max){
          max = arr[i];
      }
  }
  //4.当循环结束之后，max记录的就是最大值
  System.out.println(max);
  ```

#### 3.7.2遍历数组统计个数

需求：生成10个1~100之间的随机数存入数组

1）求出所有数据的和

2）求所有数据的平均数

3）统计有多少个数据比平均值小

代码示例：

```java
import java.util.Random;

public class ArrDemo13 {
    public static void main(String[] args) {
        //1.定义数组
        int[] arr = new int[10];
        //2.生成10个随机数存入数组
        Random r = new Random();
        for (int i = 0; i < arr.length; i++) {
            //生成随机数
            int number = r.nextInt(100) + 1;
            //把生成的随机数存入数组
            arr[i] = number;
        }
        //3.求和
        int sum = 0;
        for (int i = 0; i < arr.length; i++) {
            //累加
            sum = sum + arr[i];
        }
        //4.平均数
        int avg = sum / arr.length;
        //5.统计
        int count = 0;
        for (int i = 0; i < arr.length; i++) {
            if(arr[i] < avg){
                count++;
            }
        }
        //6.循环结束之后打印count
        System.out.println(count);
    }
}
```

## 以下的练习day05学习

#### 3.7.3交换数组中的数据

本题的前置练习1：

```
交换两个变量的值

int a = 10;
int b = 20;

//把a原本的值做了临时存储
int c = a;

//把b的值交给了a。
a = b;

//把c的值交给b
b = c;

System.out.println(a);
System.out.println(b);

```

本题的前置练习2：

```java
把0索引的元素跟最后一个元素进行交换

int[] arr = {1,2,3,4,5};
//第一个和最后一个交换
// 5 2 3 4 1

int temp = arr[0];
arr[0] = arr[4];
arr[4] = temp;

for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

需求：

​	需求：定义一个数组，存入1,2,3,4,5。交换首尾索引对应的元素。

交换前：1,2,3,4,5

交换后：5,4,3,2,1

代码示例：

```java
//1.定义数组
int[] arr = {1, 2, 3, 4, 5};
//2.循环交换数据
for (int i = 0, j = arr.length - 1; i < j; i++, j--) {
    //交换i和j指向的元素即可
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
//3.遍历数组
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

#### 3.7.4打乱数组中的数据

需求：

​	定义一个数组，存入1~5。要求打乱数组中所有数据的顺序。

代码示例：

```java
import java.util.Random;

public class ArrDemo18 {
    public static void main(String[] args) {
        //需求：定义一个数组，存入1~5。要求打乱数组中所有数据的顺序。

        //核心：
        //从0索引开始进行遍历
        //拿着遍历到的数据，跟数组中的随机索引处，进行位置交换


        //1.定义数组
        int[] arr = {1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16};
        //2.利用循环交换每一个索引上对应的元素
        Random r = new Random();
        for (int i = 0; i < arr.length; i++) {
            //生成一个新的随机索引
            int randomIndex = r.nextInt(arr.length);
            //拿着随机索引对应的元素 跟 i 对应的元素进行交换
            int temp = arr[i];
            arr[i] = arr[randomIndex];
            arr[randomIndex] = temp;
        }
        //当循环结束之后，就表示数组里面的每一个元素，都跟随机索引进行了交换
        //当循环结束之后，此时就好比已经打乱了数组中的数据。
        //3.遍历数组
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

# 面向对象思想
## Java引入了对象和类的概念
对象 = 属性 + 方法
* 对象是一个变量，即具体的东西
* 类就是类型，是规范，是定义，从千万对象中抽取共性
* 
在C语言的结构体中只能含有多种变量
struct example{
    int a;
    double b;
}
而类中可以包含多种变量+方法
```
public class Class1{
    private int a;
    public void method(){

    }
}
```
# 类和对象
* 
最简单的类 class A{}
对象 A obj = new A();
相同类型的对象在不同的内存，所以这两个对象是不一样的
obj在C++成为指针，java中成为Reference
* 
对象赋值是Reference赋值
基本类型是值拷贝
* 
new出一个对象是 内部属性值的默认值是：
short/int/  0
long  0L
boolen  false
char '\u0000'
byte  0
float 0.0f
double  0.0d 

## 构造函数
构造函数名称必须与类名一样，没有返回值
没有析构函数
* 
每个类都有构造函数
如果没有定义构造函数，java编译器会自动生成一个无参的构造函数
但是如果定义了构造函数，编译器就不会生成无参构造函数，此时如果没有定义无参构造而调用将报错

* 
可以有多个构造函数，形参列表应有差别

## 信息隐藏与this

* 
信息隐藏：通过类的方法来间接访问类的属性，而不是直接访问类的属性

* 
this指针指向本类中的成员变量
this可以代替类名

# 作业
```
import java.util.Date;
import java.util.Scanner;

public class 第四章第一次作业 {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        int a = 0, b = 0, c = 0;
        if (sc.hasNext()) {
            a = sc.nextInt();
        }
        if (sc.hasNext()) {
            b = sc.nextInt();
        }
        if (sc.hasNext()) {
            c = sc.nextInt();
        }
        long begin = System.currentTimeMillis();
        System.out.println(begin);
        if (a == 0 || b == 0 || c == 0) {

            System.out.println("输入不能为0");
            System.exit(-1);
        }
        MyNumber obj1, obj2, obj3;
        //开始

        obj1 = new MyNumber();
        obj1.num = a;
        obj2 = new MyNumber();
        obj2.num = b;
        obj3 = new MyNumber();
        obj3.num = c;
        if(a == b){
            if( a < c)
            {
                swap(obj1,obj3);
            }
        }else if(a < b)
        {
            swap(obj1,obj2);
            if(a <c){
                swap(obj2,obj3);
            }
        }else{
            if(b <c)
            {
                swap(obj2,obj3);
            }
        }

        System.out.println(a);
        System.out.println(b);
        System.out.println(c);
        long end = System.currentTimeMillis();
        System.out.println(end);
        long time = end - begin;
        System.out.println(time);
    }
    public static void swap(MyNumber m, MyNumber n)
    {
        if(m.num > n.num)
        {
            int s = m.num;
            m.num = n.num;
            n.num = s;
        }
    }
}

class MyNumber

{
    int num;
}

```

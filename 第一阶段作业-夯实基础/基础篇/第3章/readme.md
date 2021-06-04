# 第三章
## Class类
* 
类名必须与文件名一致
例如对于HelloWorld.java，它的源文件必须为
```
public class HelloWorld{
    public static void main(String args[]){

    }
}
```

* 
一个文件里可以有多个class,但是只能有一个public class

* 
一个class文件只能有一个main函数，类也可以没有main函数
main函数必须在类里面，main函数是一个Java程序总入口

* 
main函数的形参String[] args可以在运行前进行配置
Run configuration

## 基本类型
* boolen true/false
* byte 1 byte = 8 bits
* short 16位 2 bytes
* int 32位 4 bytes
（Integer 可以存储无限长）
* long 64位 8 bytes
定义long类型时，数字末尾要加一个L，例如
long num = 23028321408L；
* float 4 bytes
定义时要加一个f
float f = 1.2890f;
* double 8 bytes
* char 16位

* 
运算符 
>> 右移 * 2
<< 左移 / 2 
## 选择和循环结构
```
if (){

}else {

}
```

```
int a;
switch(a){
    case 1:
        ...;
        break;
    ...
    default:
        ...;
        break;
}
```
## 自定义函数
public static int method(int m,int n){
    return m + n;
}
* 函数必须定义在类中
* 函数的重载，函数名称相同，函数形参列表不同

# 作业
## 作业1
```
public class 第三章作业 {
    public static void main(String args[]){
        int num = 1;
        for (int i = 0; i < Integer.parseInt(args[0]); i++) {
            for (int i1 = 0; i1 < Integer.parseInt(args[0]); i1++) {
                System.out.print(num+" ");
                num++;
            }
            System.out.println();
        }
    }
}
```
## 作业2
```
public static void main(String[] args) {
        int i = 1;
        while (i <= Integer.parseInt(args[0])) {
            for (int i1 = 0; i1 < i; i1++) {
                System.out.print("*");
            }
            System.out.println();
            i += 2;
        }
        i -= 4;
        while (i >= 1) {
            for (int i1 = 0; i1 < i; i1++) {
                System.out.print("*");
            }
            System.out.println();
            i -= 2;
        }
    }
```

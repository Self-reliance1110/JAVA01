# 第二章
## IDE
IDE是不会编译与运行程序的，程序的编译与运行需要JDK的支持。
## JAVA的编写到运行

* 编写：利用记事本/IDE等完成代码文件（.java）的编写
* 编译：利用JDK中的Javac.exe将代码（.java）编译成字节码文件（.class）
* 运行：java.exe读入并解释字节码文件（.class），最终在JVM上运行
# 作业
```
public class HelloWorld {
    public static void main(String args[]) {
        System.out.println("Hello World");
        int a = 1;
        a = a + 1;
        a = a + 2;
        System.out.println("a is " + a);
        a = a + 3;
        a = a + 4;
        System.out.println("a is " + a);
    }
}
```

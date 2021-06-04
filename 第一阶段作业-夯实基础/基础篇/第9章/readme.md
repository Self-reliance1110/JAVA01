# java异常
异常：程序不正确的行为或状态
异常处理：
* 程序返回到安全状态
* 允许用户保存结果

# java异常分类
## Throwable
### Error
系统内部错误或资源耗尽

### Exception
程序有关的异常
#### RuntimeExpection
是Unchecked Excepection，不会辅助检查，需要程序猿自己解决
程序本身的错误，如
空指针，数组越界等...

#### 非RuntimeExpection
Checked Excepection IDEA会辅助检查
外界相关的错误，如
打开一个不存在的文件...


# 异常处理
##
try-catch-finally:一种保护代码正常运行的机制
try必须有，catch和finally必须有其中一个
try是正常代码，catch如果有异常发生时执行，否则跳过，fianlly代码块是执行到最后必须执行的一个部分
catch可以有多个，在try中出现异常之后就会进入到对应的catch块中，之后不会返回到try代码块也不会执行其他的catch代码，直接到finally
* 一个异常只能进入一个catch代码块
所以在写catch时，一般写在前面的异常范围是小的

##
throws：方法中可能有异常，但不处理，将异常抛出
调用有throws的方法，本身也需要throws该异常或者抛出更大范围的异常
子类重写父类抛出异常的方法不能抛出比父类范围更大的异常

# 自定义异常

继承Excepection就变成Checked Excepection
继承RuntimeExcepection就变成 Unchecked Excepection
```
public class MyExccpection extends Exception{
    
    public MyExccpection()
    {
        super();
    }
}
```
调用自定义异常时就是把异常new出来然后调用其中的方法

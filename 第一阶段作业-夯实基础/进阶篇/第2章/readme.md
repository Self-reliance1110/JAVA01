# 软件测试

## 软件测试的定义
在规定的条件下对程序进行操作，以发现程序错误，衡量软件质量，并对其是否能满足设计要求进行评估的过程

## 单元测试
单元测试是指对软件中最小的可测试单元进行检查和验证，通常是一个函数
单元测试是已知代码结构进行的测试，属于白盒测试

## 集成测试
集成测试是将多个单元相互作用，形成一个整体，对整体的协调性进行测试
一般从构成系统的最小单元开始，持续进行到单元之间的接口直到集成为一个完整的软件系统为止


...

## Junit
JUnit是Java语言单元测试框架
优点：
* 显示界面直观
* 测试效率高
```
//判断是否为三角形
public class demo01 {
    public static boolean judge(int a,int b, int c)
    {
        if(a<= 0||b<= 0||c<= 0)
            return false;
        if((a + b )<= c)
            return false;
        if((b+c )<= a)
            return false;
        if((a + c )<= b)
            return false;
        return true;
    }
}




//测试类
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class demo02 {

    @Test
    public void test01()
    {
        assertEquals(false,demo01.judge(1,2,3));
    }
}
```

在测试方法上面加一个注解@Test，JUnit就会自动去测试它

# 作业

```
//DateUtil类
public class DateUtil {
    public static boolean isLeepYear(int year)
    {
        if(year >0 && year <= 10000)
        {
            if(year%100==0)
            {
                if(year%400==0)
                    return true;
                else
                    return false;
            }
            else{
                if(year%4==0)
                    return true;
                else
                    return false;
            }

        }
        else
            return false;
    }
}
//DateUtilTest

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class DateUtilTest {
    @Test
    public void isLeapYearTest()
    {
        assertEquals(true,DateUtil.isLeepYear(2000));
        assertEquals(true,DateUtil.isLeepYear(1000));
        assertEquals(true,DateUtil.isLeepYear(20000));
        assertEquals(true,DateUtil.isLeepYear(2020));
        assertEquals(true,DateUtil.isLeepYear(2019));
        assertEquals(true,DateUtil.isLeepYear(2000));
        assertEquals(true,DateUtil.isLeepYear(1900));
    }
}
```

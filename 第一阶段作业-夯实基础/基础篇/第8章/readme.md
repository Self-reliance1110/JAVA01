# Java数字类
* 整数 Short Integer Long

* 浮点数 Double Float

* 大数类 BigInteger BigDecimal 表示的数字位数没有限制，可用内存多大就能用多大

* 随机数类 Random
nextInt()返回一个随机int
nextInt(int a)返回一个[0,a)之间的随机整数
nextDouble()返回一个[0.0,1.0]之间的double小数
ints方法批量返回随机数组

* 工具类 java.lang.Math
绝对值函数abs
对数函数log
比较函数max，min
幂函数pow
四舍五入函数round
向下取整floor
向上取整ceil

# String类
String是一个不可变对象，使用频率最高
常用方法有
* charAt(int index) 
返回 char指定索引处的值。 
* concat(String str) 
将指定的字符串连接到该字符串的末尾。 
* contains(CharSequence s) 
当且仅当此字符串包含指定的char值序列时才返回true。 
* endsWith(String suffix) 
测试此字符串是否以指定的后缀结尾。
* equals(Object anObject) 
将此字符串与指定对象进行比较。 
* equalsIgnoreCase(String anotherString) 
将此 String与其他 String比较，忽略案例注意事项。 
* hashCode() 
返回此字符串的哈希码。 
* indexOf(int ch) 
返回指定字符第一次出现的字符串内的索引。 
* length() 
返回此字符串的长度。 
...等等方法

# 可变字符串
StringBuilder,StringBuffer
常用方法有append/insert/delete/replace/substring
trimToSize()去除空隙

# 时间相关类
## 
java.util.Date类基本已经被废弃掉了，一般使用的是他的getTime()方法，可以获取从1970年1月1日到现在为止的毫秒值
java.sql.Date使用数据库的时候才会用到这个类
现在使用的比较多的是java.util.Calendar，它是一个抽象类
Calendar gc = Calendar.getInstance();会返回一个Calendar的直接子类GregorianCalendar
其实效果跟GregorianCalendar gc = new GregorianCalendar();是一样的

常用方法：
Calendar gc = Calendar.getInstance();
        int year = gc.get(Calendar.YEAR);
        int month = gc.get(Calendar.MONTH)+1;//国外的月份是0-11,所以获取之后需要+1
        int day = gc.get(Calendar.DAY_OF_MONTH);
        int hour = gc.get(Calendar.HOUR);HOUR 就是12小时制，HOUR_OF_DAY是24小时制
        int minute = gc.get(Calendar.MINUTE);
        int second = gc.get(Calendar.SECOND);

## java.time包
包下有
LocalTime类，LocalDate类还有LocalDateTime类
Instant时间戳

# 格式化类
java.text包下
* NumberFormat 数字格式化，抽象类
* MessageFormat 字符串格式化
* DateFormat 日期/时间格式化，抽象类
java.time.Format
DateTimeFormatter

# 作业
```
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Scanner;

public class demo06 {
    public static void main(String[] args) {
        String idCardStr = null;
        Scanner sc = new Scanner(System.in);
        idCardStr = sc.nextLine();
        if(idCardStr.length() == 18)//判断是否为18位
        {
            char[] chars = idCardStr.toCharArray();
            for(int i = 0;i<17;i++)
            {
                //判断前17位是否为数字
                if(!(chars[17]<=57&&chars[17]>=48)){
                    System.out.println("0000-00-00");
                    return;
                }
            }
            if((chars[17]<=57&&chars[17]>=48)||(chars[17]>=97 && chars[17]<=122))//判断第18位是否为数字或小写字母
            {
                //提取年月日
                StringBuilder stringBuilder = new StringBuilder();
                stringBuilder.append(idCardStr.substring(6,10));
                stringBuilder.append("-");
                stringBuilder.append(idCardStr.substring(10,12));
                stringBuilder.append("-");
                stringBuilder.append(idCardStr.substring(12,14));
                System.out.println(stringBuilder);
                String date = stringBuilder.toString();

                SimpleDateFormat sdf = new SimpleDateFormat("yyyy-mm-dd");//设置日期格式
                boolean isExists = true;
                try {
                    sdf.parse(date);//对日期进行转换，如果报异常说明日期不存在
                } catch (ParseException e) {
                    isExists = false;
                    System.out.println("0000-00-00");
                }
            }
            else

                System.out.println("0000-00-00");

        }else
            System.out.println("0000-00-00");
    }
}

```

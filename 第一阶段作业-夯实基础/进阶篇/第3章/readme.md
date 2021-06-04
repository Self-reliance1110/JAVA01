# 高级文本处理
## Java字符编码
### 字符编码
字符是人创造的，可以理解的，不可分割
但是计算机无法识别字符，只能理解0和1
计算机通过0和1的不同组合来表示各种字符

#### ASCII码
American Standard Code for Information Interchange，美国信息交换标准代码
采用1Byte，8bits，最多256个字符
不够用

#### Unicode(字符集)
目标：不断扩充，存储全世界所有的字符


#### 中文编码
* GB2312:7445个字符，其中有6763个简体字，还包括了拉丁字母、希腊字母、日文片假名等等
* GBK：包括了GB2312和Big5
* GB18030：包括了GBK和GB2312
* Big5：繁体中文

#### ANSI编码
* Windows上非Unicode的默认编码
* 在简体中文操作系统，ANSI代表GBK
* 在繁体中文操作系统，ANSI代表Big5
* 记事本默认使用ANSI保存
* ANSI编码文件不能兼容使用

#### Java的字符编码
* 源文件：采用UTF-8编码
* 程序内部采用UTF-16编码，非程序员控制
* 和外界进行输入输出尽量使用UTF-8，不能使用一种编码输入，另一种编码输出

## Java国际化编程
Internationalization，缩写为i18n
Java是第一个设计成支持国际化的编程语言
java.util.ResourceBundle用于加载一个语言_国家语言包
java.util.Locale定义一个语言_国家
使用如下
```
Locale myLocale =  new Locale("en","US");
System.out.println(myLocale);
ResourceBundle bundle = ResourceBundle.getBundle("message",myLocale);
System.out.println(bundle.getString("hello"));//会输出message对应的properties文件中hello对应的值
```
### Locale类
* Locale(ebx1,ebx2)
语言:zh,en...
国家:CN,US...

* Locale方法
getDefault() 获取Java虚拟机的此实例的默认语言环境的当前值。
getAvailableLocales() 返回所有已安装区域设置的数组。 

* 语言文件
Properties文件
命名规则：包名+语言+地区.properties
例如：message_zh_CN.properties，message_zh.properties，message_CN.properties，message.properties都是可以的
存储文件必须是ASCII文件
如果是ASCII以外的文字，需要使用Unicode里面的/u....来表示
可以通过native2ascii.exe进行转化

## Java高级字符串处理

### Java的正则表达式
java.util.regex包下有Pattern类和Matcher类 
必须首先将正则表达式（指定为字符串）编译为此类的实例。 然后将所得的图案可以被用来创建一个Matcher对象可以匹配任意character sequences针对正则表达式。 执行匹配的所有状态都驻留在匹配器中，所以许多匹配者可以共享相同的模式。 
因此，典型的调用序列 
'''
 Pattern p = Pattern.compile("a*b");
 Matcher m = p.matcher("aaaaab");
 boolean b = m.matches();
'''

### 其他字符串操作
* 字符串和集合互转
```
ArrayList<String> names = new ArrayList<>();
        names.add("张三");
        names.add("李四");
        names.add("王五");
        names.add("赵六");
        //ArrayList转换为字符串
        String str1 = String.join(",",names);
        System.out.println(str1);
        //字符串转化为List
        List<String> names1 = Arrays.asList(str1.split(","));
        System.out.println(names1);
```

输出结果为
张三,李四,王五,赵六
[张三, 李四, 王五, 赵六]

除此之外还有Apache Commons Lang第三方jar包的StringUtils.jion方法可用
* 字符串转义

* 变量名字格式化

* 字符串输入流

# 作业
```
        Locale locale = Locale.getDefault();//获取默认国家语言
        ResourceBundle bundle = ResourceBundle.getBundle("msg",locale);
        System.out.println(bundle.getString("name"));
        System.out.println(bundle.getString("university"));
        locale = new Locale("en","NZ");
        bundle = ResourceBundle.getBundle("msg",locale);
        System.out.println(bundle.getString("name"));
        System.out.println(bundle.getString("university"));
        locale = new Locale("en","US");
        bundle = ResourceBundle.getBundle("msg",locale);
        System.out.println(bundle.getString("name"));
        System.out.println(bundle.getString("university"));
```
输出结果为
```
张三
重庆邮电大学
张三
重庆邮电大学
Tom
Exception in thread "main" java.util.MissingResourceException: Can't find resource for bundle java.util.PropertyResourceBundle, key university
	at java.base/java.util.ResourceBundle.getObject(ResourceBundle.java:564)
	at java.base/java.util.ResourceBundle.getString(ResourceBundle.java:521)
	at demo05.main(demo05.java:17)

Process finished with exit code 1

```

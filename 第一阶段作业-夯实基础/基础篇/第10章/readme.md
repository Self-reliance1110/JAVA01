# 数组
数组是一种可以存储多个数据的容器
* 数据是同一种类型
* 所有的数据都是线性规则排列* 可以通过位置索引来访问数据数据
* 在创建数组的时候需要规定数组的大小

# List列表
有序的容器
允许重复的元素
可以有嵌套
在不同步的情况下建议优先使用ArrayList类

# 集合Set
* 集合类能确定任意一个对象是否在集合当中
* 集合内的对象是互异的
* 集合是无序的
* 集合内的元素只能是对象
## HashSet
基于HashMap实现，可以容纳null元素，是不同步的
Set s = Collections.synchronizedSet(new HasaSet(..))可以创建一个同步的HashMap
常用方法，add,remove,clear,contains,retainAll(计算两个集合的交集)
如果对HashSet进行遍历的话建议使用for-each进行遍历

## LinkedHashSet
继承于HashSet，通过一个双向链表维护插入顺序，所以LinkedHashSet的插入顺序跟遍历顺序是一样的
其他的基本与HashSet一致

## TreeSet
基于TreeMap实现，不可以容纳null元素
不支持同步但是同样可以通过Collections的方法来创建一个同步的TreeSet对象
对于TreeSet的遍历是按照升序排列进行输出的


## 
HashSet和LinkedHashSet判断元素是否重复是根据两个元素的hashCode的返回值是否相同，
若不同则返回false，若相同则返回true并通过equals方法再次判断是否相同
而TreeSet是通过继承Comparable接口，里面有个方法是compareTo

# Map
Java中的Map有
Hashtable，同步，速度慢，数据量小
HashMap，不同步，速度快，数据量大
Properties不同步，以文件形式存储，数据量小

## HashTable
K-V中K和V都不允许是null
是同步的，数据量小，无序
主要方法有clear、contains、get、put、remove、size

当
Hashtable.put(1,1);
执行之后下面又有一个put使用的是同一个key，原先的value将被覆盖
Hashtable.put(1,2);
Key=1，对应的Vlaue=2

## LinkedHashMap
通过双向链表维持插入顺序，是有序的

## TreeMap

## Properties
继承于Hashtable，可以将K-V对存储在文件中，适合存储数据量小的
从文件加载的方法load
写入到文件的方法store
获取属性getProperty
设置属性setProperty

# 工具类
## Arrays类
处理对象是数组
排序：sort/parallelSort
查找：binarySearch，从一个数组中查找一个元素
批量拷贝：copyOf，从原数组复制元素到目标数组
批量赋值：fill
等价比较：equals，比较两个数组内容是否相同

## Collections类
处理对象是Collections及其子类，一般是基于List进行处理
排序：sort
对于Integer此类对象不需要管，但是对于自定义的对象
则需要自定义类实现Comparable接口，重写compareTo方法
如果对象类是不可变的，只能在sort参数中新建一个Comparator
搜索：binarSearch
批量赋值：fill
最大，最小值：max、min
反序：将List反序排列，reverse

# 作业
```
class Currency implements Comparable<Currency>{

    private String name;       
    private int originalValue;  
    private int value;          

    public static String[] CURRENCY_NAME = { "CNY", "HKD", "USD", "EUR" };
    public static int[] CURRENCY_RATIO = { 100, 118, 15, 13 };


    public Currency(String name,int originalValue,int value)
    {
        this.name = name;
        this.originalValue = originalValue;
        this.value = value;
    }


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }



    public int getOriginalValue() {

        return originalValue;

    }

    public void setOriginalValue(int originalValue) {

        this.originalValue = originalValue;

    }

    public int getori() {

        return value;

    }

    public void setValue(int value) {

        this.value = value;

    }

    @Override
    public int compareTo(Currency o) {
        if((double)((100/this.getOriginalValue())*this.getori()) - (double)((100/o.getOriginalValue())*o.getori())>0)
            return 1;
        else if((double)((100/this.getOriginalValue())*this.getori()) - (double)((100/o.getOriginalValue())*o.getori())<0)
            return -1;
        else
            return 0;

    }
}


public class EUR extends Currency {
   
    public EUR(int a)
    {
        super("EUR",a,13);
    }
}

public class HKD extends Currency {


    public HKD(int a)
    {
        super("HKD",a,118);
    }
}

public class USD extends Currency{
   
    public USD(int a)
    {
        super("USD",a,15);
    }
}


import java.util.Scanner;

public class demo10 {
    public static void main(String[] args) {

        Currency[] cs = new Currency[3];

        Scanner sc = new Scanner(System.in);

        int a = 0;
        int b = 0;
        int c = 0;

        if (sc.hasNext()) {
            a = sc.nextInt();
            cs[0] = new HKD(a);
        }

        if (sc.hasNext()) {
            b = sc.nextInt();
            cs[1] = new USD(b);
        }

        if (sc.hasNext()) {
            c = sc.nextInt();
            cs[2] = new EUR(c);
        }

        if(cs[0].compareTo(cs[1]) ==1||cs[0].compareTo(cs[1]) ==0)
        {
            if(cs[0].compareTo(cs[2]) ==1)
            {
                System.out.println(cs[0].getName()+cs[0].getOriginalValue());
                if(cs[1].compareTo(cs[2]) ==1||cs[1].compareTo(cs[2]) ==0) {
                    System.out.println(cs[1].getName() + cs[1].getOriginalValue());
                    System.out.println(cs[2].getName() + cs[2].getOriginalValue());
                }
                else {
                    System.out.println(cs[2].getName() + cs[2].getOriginalValue());
                    System.out.println(cs[1].getName()+cs[1].getOriginalValue());
                }
            }

        }else if (cs[1].compareTo(cs[2]) ==1)
        {
            System.out.println(cs[1].getName() + cs[1].getOriginalValue());
            if(cs[0].compareTo(cs[2]) ==1||cs[0].compareTo(cs[2]) ==0)
            {
                System.out.println(cs[0].getName()+cs[0].getOriginalValue());
                System.out.println(cs[2].getName()+cs[2].getOriginalValue());

            }
            else{
                System.out.println(cs[2].getName()+cs[2].getOriginalValue());
                System.out.println(cs[0].getName()+cs[0].getOriginalValue());
            }

        }else{
            System.out.println(cs[2].getName()+cs[2].getOriginalValue());
            System.out.println(cs[1].getName()+cs[1].getOriginalValue());
            System.out.println(cs[0].getName()+cs[0].getOriginalValue());


        }

    }
}

```

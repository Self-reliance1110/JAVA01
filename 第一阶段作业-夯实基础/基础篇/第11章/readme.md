# Java文件类File
java.io.File是目录和文件的重要类
常用方法有
creatNewFile,delete,exists,getAbsolutePath,getName,....等等
File类不涉及文件的内容，只涉及文件的基本属性

# java io包的概述
java读写文件只能以流的方式读写文件
文件是以字节形式保存的，所以将程序中变量保存到文件中需要进行类型转换

## 节点类
直接操作文件类
* InputStream,OutputStream(以字节形式)
    FileInputStream,FileOutputStream
* Reader,Writer(以字符的形式)
    FileReader,FileWriter

## 转换类
InputStreamReader:从文件以字节形式读取然后转化为Java能看懂的字符型
OutputStreamWriter:在Java中将字符转化为字节写入到文件中

## 装饰类
DataInputStream,DataOutputStream
BufferInputStream,BufferOutpuStream
BufferReader,BufferWriter



总的来的说，Java的IO流包括字节流，字符流，File类文件操作还有System.in/System.out/System.err

# 文本文件的读写操作
文件很大，Java注定只能以流的方式进行处理
try - resource语句，自动关闭资源，可以提高编程效率
写操作
```
 try (BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("D:/abc.txt")))) {
            bw.write("你好");
            bw.newLine();
            bw.write("abc");
            bw.newLine();
        } catch (Exception ex) {
            ex.printStackTrace();
        }
```
读操作
```
try (BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("D:/abc.txt")))) {
            bw.write("你好");
            bw.newLine();
            bw.write("abc");
            bw.newLine();
        } catch (Exception ex) {
            ex.printStackTrace();
        }
```
尽量使用这种try - resource的写法，可以自动关闭资源

# 二进制文件的读写
跟读写文本文件类似，使用的是FileOutputStream,BufferedOutputStream,DataInputStreamWriter(转化类)
构建关系DataInputStreamWriter(BufferedOutputStream(FileOutputStream))

# zip文件的读写
也就是压缩与解压缩
java zip 包支持对zip，gzip文件进行操作
ZipInputStream,ZipOutputStream
ZipEntry 压缩项

## 压缩
* 打开输出的zip文件
* 添加一个ZipEntry文件，压缩包里面一个文件就相当于一个ZipEntry
* 打开输入的文件，读数据，向ZipEntry写数据，关闭输入文件
* 重复上面的步骤，写多个Entry
* 关闭输出的zip文件

解压缩就是反着来
* 打开输入的zip文件
* 获取下一个ZipEntry
* 新建一个目标文件，从ZipEntry中读取数据，向目标文件写入数据，关闭目标文件
* 重复上面的步骤，写多个目标文件
* 关闭zip文件
# 作业
```
import java.io.*;
import java.util.*;

public class Works {
    public static void main(String[] args) {
        File a  =new File("D:/a.txt");
        ArrayList<String> arr = new ArrayList<>();
        try(BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream(a)))){
            String str = null;
            while((str = br.readLine())!=null)
            {

                String[] split1 = str.split("=");

                for (int i = 0; i < split1.length; i++) {


                    String[] s = split1[i].split(" ");

                    for (int i1 = 0; i1 < s.length; i1++) {


                        if(s[i1].length()>0)
                            arr.add(s[i1]);


                    }
                }
            }

        }catch (Exception e) {
            e.printStackTrace();
        }
//        for (int i = 0; i < arr.size(); i++) {
//            System.out.println(arr.get(i));
//        }

        HashMap<String,Integer> hm = new HashMap<>();
        for (int i = 0; i < arr.size(); i++) {

            if(hm.containsKey(arr.get(i).toLowerCase()))
            {
                hm.put(arr.get(i).toLowerCase(),hm.get(arr.get(i))+1);
            }else
                hm.put(arr.get(i).toLowerCase(),1);
        }


        Iterator<String> iterator1 = hm.keySet().iterator();
//        while(iterator1.hasNext())
//        {
//            System.out.println(iterator1.next()+","+hm.get(iterator1.next()));
//        }

        File b = new File("D:/b.txt");
        ArrayList<Map.Entry<String, Integer>> value= new ArrayList<>();

        Set<Map.Entry<String, Integer>> entries = hm.entrySet();
        Iterator<Map.Entry<String, Integer>> iterator = entries.iterator();


        while(iterator.hasNext())
        {
            value.add(iterator.next());
        }
        value.sort(new Comparator<Map.Entry<String, Integer>>() {
            @Override
            public int compare(Map.Entry<String, Integer> o1, Map.Entry<String, Integer> o2) {
                return o2.getValue()- o1.getValue();
            }
        });

        try(BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(new FileOutputStream(b)))) {
            bw.write("============");
            bw.newLine();

            for (int i = 0; i < value.size(); i++) {
                bw.write(value.get(i).getKey()+","+value.get(i).getValue());
                bw.newLine();
            }
            bw.write("============");

        }catch (Exception e)
        {
            e.printStackTrace();
        }
    }
}

```

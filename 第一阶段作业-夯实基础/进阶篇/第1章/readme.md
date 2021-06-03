# Maven
如果要添加第三方库，使用传统方法操作非常的烦琐
需要搜索，确定版本，下载jar包等一系列操作，不但工作量大而且很容易出现错误

Maven就是一款能够自动下载、管理jar包又能配置buildpath的构建工具

## 使用Maven的方法
* 首先创建一个Maven项目
* 到Maven的中央仓库mvnrepository.com，搜索所需的类库
* 以Common Math为例，选择其中的一个版本之后将依赖文本添加到Maven项目中的pom.xml当中
依赖文本像这样
```

<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-math3 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-math3</artifactId>
    <version>3.6.1</version>
</dependency>

```

## Maven构建工具的作用
* 自动甄别和下载第三方库
* 可以完成项目的编译，测试
* 可以完成项目的打包

## 创建Maven项目
* 创建Maven Project，也可以先创建一个Java Projec，然后再转化为Maven项目
* 设置信息：
    groupId：组织名称
    artifactId：项目名
    Name：别名
    Description:描述
* 修改pom.xml文件，添加依赖语句


# 作业
```
import net.sourceforge.pinyin4j.PinyinHelper;
import net.sourceforge.pinyin4j.format.HanyuPinyinCaseType;
import net.sourceforge.pinyin4j.format.HanyuPinyinOutputFormat;
import net.sourceforge.pinyin4j.format.HanyuPinyinToneType;
import net.sourceforge.pinyin4j.format.HanyuPinyinVCharType;
import net.sourceforge.pinyin4j.format.exception.BadHanyuPinyinOutputFormatCombination;

public static String getPinyin(String hanzi)
    {
        HanyuPinyinOutputFormat format = new HanyuPinyinOutputFormat();
        format.setCaseType(HanyuPinyinCaseType.LOWERCASE);//输出大小写设置
        format.setToneType(HanyuPinyinToneType.WITHOUT_TONE);//设置音标，这里没有设置音标
        format.setVCharType(HanyuPinyinVCharType.WITH_U_UNICODE);//...
        char[] hanYuArr = hanzi.trim().toCharArray();

        StringBuilder pinYin = new StringBuilder();
        try {
            for (int i = 0, len = hanYuArr.length; i < len; i++) {

                if (Character.toString(hanYuArr[i]).matches("[\\u4E00-\\u9FA5]+")) {

                    String[] pys = PinyinHelper.toHanyuPinyinStringArray(hanYuArr[i], format);
                    pinYin.append(pys[0]).append(" ");
                } else {
                    pinYin.append(hanYuArr[i]).append(" ");
                }
            }
        } catch (BadHanyuPinyinOutputFormatCombination badHanyuPinyinOutputFormatCombination) {
            badHanyuPinyinOutputFormatCombination.printStackTrace();
        }
        return pinYin.toString();

    }
    public static void main(String[] args) {
        System.out.println(getPinyin("你好啊"));
    }
```
输出结果为ni hao a 

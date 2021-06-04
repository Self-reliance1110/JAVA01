# package,import和classpath
## package
package cn.moximoxi.demo
在类中出现这个的意思就是这个类.java文件在cn目录下的moximoxi目录下的demo的目录下
类的完整名字就是cn.moximoxi.demo.XX.java
包名尽量做到唯一

## import
import可以用来引入类
例如 import cn.moximoxi.demo.XX.java
improt必须放在package后面，class前面

## jar文件的导出与导入
jar文件的优势
* jar文件可以包括多个class，比多层目录更加简洁实用
* 便于网络下载和传播
* 可以规定版本号，更容易控制版本更迭
* jar文件只有.class文件，没有.java文件，可以保护源代码的知识产权

## java的访问权限
private：私有的，只有本类可以访问
default：同一个包访问
protected：同一个包，子类可访问
public：公开

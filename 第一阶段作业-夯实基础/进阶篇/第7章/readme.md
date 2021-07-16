# 数据库
<--！自己查阅资料学习的数据库-->

1.数据库
SHOW DATABASES;-- 数据库列表
CREATE DATABASE 数据库名称;-- 创建数据库
CREATE DATABASE IF NOT EXISTS 数据库名称;-- 创建数据库
SHOW CREATE DATABASE 数据库名称;-- 查看数据库信息
ALTER DATABASE 数据库名称 CHARACTER SET 编码;-- 修改数据库
DROP DATABASE 数据库名称;-- 删除数据库
DROP DATABASE IF EXISTS 数据库名称;-- 删除数据库
USE 数据库名称;-- 使用数据库
2.表
SHOW TABLES;-- 表列表
SHOW CREATE TABLE 表名称;-- 查看表信息
DESC 表名称；-- 查看表结构
CREATE TABLE 表名称（列名称1 数据类型1，
		       列名称2 数据类型2
		       ....		    ）；-- 创建表
DROP TABLE 表名称；-- 删除表
DROP TABLE IF EXISTS 表名称；-- 删除表
CREATE TABLE 创建表名称 LIKE 被复制的表名称；--复制表
ALTER TABLE 表名称 RENAME TO 新表名称;-- 修改表名称
ALTER TABLE 表名称 ADD 列名 数据类型;-- 添加一列
ALTER TABLE 表名称 CHANGE 列名 新列名 新数据类型；-- 修改列名，数据类型
ALTER TABLE 表名称 MODIFY 列名 新数据类型；-- 修改数据类型
ALTER TABLE 表名称 DROP 列名；-- 删除列

##DQL操作
1.INSERT INTO 表名称(列名1，列名2...）VALUES(N1,N2...);-- 增加数据
2.DELETE FROM 表名称 WHERE 条件;-- 删除数据
3.DELETE FROM 表名称;-- 删除表中所有数据，不推荐
4.TRUNCATE TABLE 表名称;-- 删除表，然后再创建一个一摸一样的空表
5.UPDATE 表名称 SET 列名1 = ...,列名2 = ... WHERE 条件；-- 修改数据，如果不加条件，则表中所有数据都将被修改


查询语法
SELECT
	字段列表
FROM
	表名称
WHERE
	条件列表
GROUP BY
	分组字段
HAVING
	分组后的条件限定
ORDER BY
	排序
LIMIT
	分页限定

关键字DISTINCT
SELECT DISTINCT GENDER FROM STU3;-- 去重复

WHERE
BETWEEN AND
AND 
OR
IN(集合)
IS
IS NOT
模糊查询：
LIKE
	占位符：
		_:单个任意字符
		%:多个任意字符


查询语句
1.排序查询
	语法： ORDER BY 排序字段1 排序方式1,  排序字段2 排序方式2...;
		SELECT * FROM STU3 ORDER BY MATH ASC ,ID DESC;
	排序方式：
		ASC:升序，默认；
		DESC:降序
如果有多个排序条件，则当第一个条件一样时才会进行第二个条件的判断

2.聚合函数
	COUNT:计算个数
		COUNT(非空列)
		COUNT(IFNULL())
		COUNT(*)
	MIN:计算最小值
	MAX:计算最大值
	AVG:计算平均值
	SUM:计算和
语法：      SELECT AVG(列名称) FROM 表名称;
	默认不计NULL值项
	SELECT COUNT(IFNULL(ENG,0)) FROM STU3;
3.分组查询
1.语法：
GROUD BY 分组字段;
SELECT GENDER,COUNT(ID),AVG(MATH) FROM STU3 WHERE MATH >= 91 GROUP BY GENDER;
SELECT GENDER,COUNT(ID),AVG(MATH) FROM STU3 WHERE MATH >= 91 GROUP BY GENDER HAVING COUNT(ID)>2 ;
WHERE在分组前进行限定，HAVING在分组后进行限定
SELECT GENDER,COUNT(ID) AS NUM,AVG(MATH) FROM STU3 WHERE MATH >= 91 GROUP BY GENDER HAVING NUM>2 ;-- NUM为COUNT(ID)的别名；
4.分页查询
	语法：LIMIT 开始的索引，查询条数
SELECT * FROM STU3 LIMIT 2,2;



##约束
1.非空约束：NOT NULL
CREATE TABLE STU4(
ID INT,
NAME VARCHAR(10) NOT NULL);-- 创建时添加了非空约束

ALTER TABLE STU4 MODIFY NAME VARCHAR(10);-- 创建表后取消了非空约束
ALTER TABLE STU4 MODIFY NAME VARCHAR(10) NOT NULL;--创建表后添加了非空约束，注意在添加约束前应删除或改变表内含有NULL的值

2.唯一约束：UNIQUE
CREATE TABLE STU4(
ID INT,
NAME VARCHAR(10) UNIQUE);-- 创建表时添加了唯一约束
ALTER TABLE STU4 DROP INDEX NAME ;-- 创建表后删除了唯一约束 
ALTER TABLE STU4 MODIFY NAME VARCHAR(10) UNIQUE;创建表后添加了唯一约束

3.主键约束：PRIMARY KEY
	含义：
		非空且唯一
		一张表有且只有一个字段为主键
		主键就是表中记录的唯一标识
CREATE TABLE STU4(
ID INT PRIMARY KEY,
NAME VARCHAR(10));-- 创建表时添加主键
ALTER TABLE STU4 DROP PRIMARY KEY;-- 创建表后删除主键约束
ALTER TABLE STU4 MODIFY ID INT PRIMARY KEY;-- 创建表后添加主键约束

自动增长//一般跟主键一起使用
auto_increment
CREATE TABLE STU4(
ID INT PRIMARY KEY AUTO_INCREMENT-- 给ID添加主键约束，自动增长,
NAME VARCHAR(10));
INSERT INTO STU4 VALUE(NULL,'BBB');
ALTER TABLE STU4 MODIFY ID INT;-- 删除自动增长，不会删除主键约束
ALTER TABLE STU4 MODIFY ID INT ATUO_INCREMENT;-- 添加自动增长

4.外键约束

CREATE TABLE EMP(
	ID INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(30),
	AGE INT,
	DEP_NAME VARCHAR(30),
	DEP_LOCATION VARCHAR(30));
INSERT INTO EMP(NAME,AGE,DEP_NAME,DEP_LOCATION) VALUES('张三',20,'研发部','广州');
INSERT INTO EMP(NAME,AGE,DEP_NAME,DEP_LOCATION) VALUES('李四',21,'研发部','广州');
INSERT INTO EMP(NAME,AGE,DEP_NAME,DEP_LOCATION) VALUES('王五',20,'研发部','广州');
INSERT INTO EMP(NAME,AGE,DEP_NAME,DEP_LOCATION) VALUES('老王',20,'销售部','深圳');
INSERT INTO EMP(NAME,AGE,DEP_NAME,DEP_LOCATION) VALUES('小王',22,'销售部','深圳');
INSERT INTO EMP(NAME,AGE,DEP_NAME,DEP_LOCATION) VALUES('大王',18,'销售部','深圳');

+----+--------+------+-----------+--------------+
| ID | NAME   | AGE  | DEP_NAME  | DEP_LOCATION |
+----+--------+------+-----------+--------------+
|  1 | 张三   |   20 | 研发部    | 广州         |
|  2 | 李四   |   21 | 研发部    | 广州         |
|  3 | 王五   |   20 | 研发部    | 广州         |
|  4 | 老王   |   20 | 销售部    | 深圳         |
|  5 | 小王   |   22 | 销售部    | 深圳         |
|  6 | 大王   |   18 | 销售部    | 深圳         |
+----+--------+------+-----------+--------------+


--数据有冗余


#外键约束：FOREIGN KEY

CREATE TABLE DEPARTMENT(
ID INT PRIMARY KEY AUTO_INCREMENT,
NAME VARCHAR(20),
LOCATION VARCHAR(20)); 


CREATE TABLE EMPLOYEE(
ID INT PRIMARY KEY AUTO_INCREMENT,
NAME VARCHAR(10),
AGE INT,
DEP_ID INT,
CONSTRAINT EMP_DEP FOREIGN KEY (DEP_ID) REFERENCES DEPARTMENT(ID)-- 添加外键关联
);

CONSTRAINT 外键名称 FOREIGN KEY (外键列名称) REFERENCES 主表名称(主表列主键名称)
ALTER TABLE EMPLOYEE DROP FOREIGN KEY 外键名称;-- 删除外键
ALTER TABLE EMPLOYEE ADD CONSTRAINT EMP_DEP FOREIGN KEY (DEP_ID) REFERENCES DEPARTMENT(ID);-- 添加外键

级联操作
CONSTRAINT 外键名称 FOREIGN KEY (外键列名称) REFERENCES 主表名称(主表列主键名称) ON UPDATE CASCADE;--添加外键，并设置级联更新
CONSTRAINT 外键名称 FOREIGN KEY (外键列名称) REFERENCES 主表名称(主表列主键名称) ON UPDATE CASCADE ON DELETE CASCADE;--添加外键，并设置级联更新,级联删除


##数据库的设计
1.一对多关系
外键
2.多对多关系
第三张表
3.一对一关系
给外键添加唯一


CREATE TABLE TAB_CATEGORY
(ID INT PRIMARY KEY AUTO_INCREMENT,
CNAME VARCHAR(30) NOT NULL UNIQUE);

CREATE TABLE TAB_ROUTE(
RID INT PRIMARY KEY AUTO_INCREMENT,
RNAME VARCHAR(30) NOT NULL UNIQUE,
PRICE INT,
RDATE DATE,
CID INT,
CONSTRAINT RID_CID FOREIGN KEY (CID) REFERENCES TAB_CATEGORY(ID) ON UPDATE CASCADE);


CREATE TABLE TAB_USER(
UID INT PRIMARY KEY AUTO_INCREMENT,
USERNAME VARCHAR(30) NOT NULL UNIQUE,
PASSWORD VARCHAR(30) NOT NULL,
NAME VARCHAR(30));


CREATE TABLE TAB_FAVORRITE
(RID INT,
UID INT,
PRIMARY KEY(RID,UID),
CONSTRAINT RIDKEY FOREIGN KEY (RID) REFERENCES TAB_ROUTE(RID),
CONSTRAINT UIDKEY FOREIGN KEY (RID) REFERENCES TAB_USER(UID));

##数据库的备份与还原
命令行：
	备份：mysqldump -uroot -p59192584 db5 > d://a.sql
	还原：use db1;
	          source d://a.sql;


##多表查询
多表查询的分类：
1.内连接查询：
	1.隐式内连接：使用where条件消除无用数据
		SELECT A.ID,A.NAME,A.DEP_ID,B.NAME
		FROM EMPLOYEE A,DEPARTMENT B 
		WHERE A.DEP_ID = B.ID;
	2.显式内连接：
	SELECT * 
	FROM EMPLOYEE 
	INNER JOIN DEPARTMENT
 	ON EMPLOYEE.DEP_ID = DEPARTMENT.ID;
2.外连接查询：
	1.左外连接：SELECT EMPLOYEE.*,DEPARTMENT.NAME FROM EMPLOYEE LEFT JOIN DEPARTMENT ON EMPLOYEE.DEP_ID = DEPARTMENT.ID;

	2.右外连接：SELECT EMPLOYEE.*,DEPARTMENT.NAME FROM EMPLOYEE RIGHT JOIN DEPARTMENT ON EMPLOYEE.DEP_ID = DEPARTMENT.ID;

3.子查询：
	查询中嵌套查询，称嵌套的查询为子查询


##事务
1.开启事务：START TRANSACTION;
2.回滚:ROLLBACK;
3.提交：COMMIT；


MYSQL默认自动提交
开启事务时才是手动提交

SELECT @@AUTOCOMMIT;-- 提交方式：1代表自动提交 0代表手动提交
SET @@ATUOCOMMIT;

# 数据库编程
Java操作数据库需要搭建桥梁，如JDBC

# 作业
```
import javax.mail.*;
import javax.mail.internet.AddressException;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import java.sql.*;
import java.util.Properties;

public class SelectTest {
    public static void main(String[] args){    	
    	  
        Connection conn = null;
        try {
        	
            //从Druid获取
            conn = DruidFactory1.getConnection();
            
            
            //构建数据库执行者
            Statement stmt = conn.createStatement(); 
            
            //执行SQL语句并返回结果到ResultSet
            ResultSet rs = stmt.executeQuery("select _ff,_tt,subject,content from t_mail where id = 1");


            //开始遍历ResultSet数据
            while(rs.next())
            {
                SendMessage(rs.getString(1),rs.getString(2),rs.getString(3),rs.getString(4),"yzofqljtdoojcahj");
            }
            
            rs.close();
            stmt.close();
            
        } catch (Exception e){
            e.printStackTrace();
        } finally {
        	try	{
        		if(null != conn) {
            		conn.close();
            	}
        	} catch (SQLException e){
                e.printStackTrace();
        	}        	
        }

    }
    public static void SendMessage(String from,String to,String subject,String content,String AuthorizationCode)
    {
        try {
            //创建Properties 类用于记录邮箱的一些属性
            final Properties props = new Properties();
            //表示SMTP发送邮件，必须进行身份验证
            props.put("mail.smtp.auth", "true");
            //此处填写SMTP服务器
            props.put("mail.smtp.host", "smtp.qq.com");
            //端口号，
            props.put("mail.smtp.port", "587");
            //此处填写账号
            props.put("mail.user", from);
            //此处的密码就是16位STMP口令
            props.put("mail.password", AuthorizationCode);
            //构建授权信息，用于进行SMTP进行身份验证
            Authenticator authenticator = new Authenticator() {

                protected PasswordAuthentication getPasswordAuthentication() {
                    // 用户名、密码
                    String userName = props.getProperty("mail.user");
                    String password = props.getProperty("mail.password");
                    return new PasswordAuthentication(userName, password);
                }
            };
            //使用环境属性和授权信息，创建邮件会话
            Session mailSession = Session.getInstance(props, authenticator);
            //创建邮件消息
            MimeMessage message = new MimeMessage(mailSession);
            //设置发件人
            InternetAddress form = new InternetAddress(props.getProperty("mail.user"));
            message.setFrom(form);

            //设置收件人的邮箱
            InternetAddress To = new InternetAddress(to);
            message.setRecipient(MimeMessage.RecipientType.TO, To);

            //设置邮件标题
            message.setSubject(subject);

            String msg = content;
            //编写内容
            StringBuilder sb = new StringBuilder();
            sb.append(msg);

            //设置邮件的内容体
            message.setContent(sb.toString(), "text/html;charset=UTF-8");

            //最后当然就是发送邮件啦
            Transport.send(message);
        } catch (AddressException e) {
            e.printStackTrace();
        } catch (MessagingException e) {
            e.printStackTrace();
        }
    }
}
```

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

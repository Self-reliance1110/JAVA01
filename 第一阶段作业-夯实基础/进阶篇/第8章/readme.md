# Java混合编程
* Java程序和其他应用程序进行通讯和数据交互
* Java和Java/C/JS/Python/Web Service/命令行的混合编程

## 可序列化对象
* 实现Serializable接口。
* 对象可以序列化为字节，在多个JVM中传递。
* 客户端和服务端的JVM里面，都有对象的拷贝。
## 远程对象
* 实现Remote接口。对象不可以序列化。
* 对象只存在服务端中，客户端拥有指向这个远程对象的指针，
* 但客户端JVM里面没有对象的值的拷贝。
* 服务端的抽象实现类需要继承UnicastRemoteObject类
## 安全权限
必要时需要提高RMI的权限，以进行保护级别更高的操作
# 作业
//客户端代码
```
import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NameClassPair;
import javax.naming.NamingException;
import java.rmi.RemoteException;
import java.util.Enumeration;
import java.util.Scanner;

public class ClientTest {
    public static void main(String[] args) throws RemoteException, NamingException {
        Context namingContext = new InitialContext();

        //开始查找RMI注册表上有哪些绑定的服务
        System.out.print("RMI 注册表绑定列表: ");
        Enumeration<NameClassPair> e = namingContext.list("rmi://127.0.0.1:8001/");
        while (e.hasMoreElements())
            System.out.println(e.nextElement().getName());


        String url = "rmi://127.0.0.1:8001/warehouse1";
        WareHouse centralWarehouse = (WareHouse) namingContext.lookup(url);


        //输入命令 输出结果
        Scanner sc = new Scanner(System.in);
        String descr = sc.nextLine();
        centralWarehouse.Execute1(descr);

    }
}
```
//接口
```
import java.rmi.Remote;
import java.rmi.RemoteException;

public interface WareHouse extends Remote {
    void Execute1(String statement) throws RemoteException;

}
```
//服务端代码
```
import java.rmi.Naming;
import java.rmi.registry.LocateRegistry;

public class ServerTest {
    public static void main(String[] args) throws Exception
    {
        WareHouseImpl centralWarehouse = new WareHouseImpl();
        LocateRegistry.createRegistry(8001);//定义端口号
        Naming.rebind("rmi://127.0.0.1:8001/warehouse1", centralWarehouse);
        System.out.println("等待调用");
    }
}
```
//接口实现类
```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.rmi.RemoteException;
import java.rmi.server.UnicastRemoteObject;

public class WareHouseImpl extends UnicastRemoteObject implements WareHouse{

    public WareHouseImpl() throws RemoteException {
        super();
    }

    @Override
    public void Execute1(String statement) throws RemoteException {
        String [] cmd={"cmd",statement};
        try {
            Process proc =Runtime.getRuntime().exec(cmd);
            BufferedReader stdInput = new BufferedReader(new InputStreamReader(proc.getInputStream()));
            BufferedReader stdError = new BufferedReader(new InputStreamReader(proc.getErrorStream()));
            System.out.println("标准输出命令");
            String s = null;

            while ((s = stdInput.readLine()) != null) {
                System.out.println(s);
            }
            while ((s = stdError.readLine()) != null) {
                System.out.println(s);
            }
            System.out.println("完了");

        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```

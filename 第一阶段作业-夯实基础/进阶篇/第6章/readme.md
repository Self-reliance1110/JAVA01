# 网络编程
## 网络基础知识
### IP地址:每个网卡/机器都有一个或多个IP地址
* IPV4:192.168.0.100，每段从0到255
* IPV6: 128bit长，分成8段，每段4个16进制数
### port：端口,0-65535
* 0~1023, OS已经占用了，80是Web，23是telnet
* 1024~65535，一般程序可使用(谨防冲突)
* 两台机器通讯就是在IP+Port上进行的
### 通讯协议：TCP和UDP
#### TCP(Transmission Control Protocol) 
* 传输控制协议，面向连接的协议
* 两台机器的可靠无差错的数据传输
* 双向字节流传递
#### UDP(User Datagram Protocol) 
* 用户数据报协议，面向无连接协议
* 不保证可靠的数据传输
* 速度快，也可以在较差网络下使用

## UDP编程
### 计算机通讯
数据从一个IP的port出发（发送方），运输到另外一个IP的port（接收方）
#### UDP：无连接无状态的通讯协议，
* 发送方发送消息，如果接收方刚好在目的地，则可以接受。如果不在，那这个消息就丢失了
* 发送方也无法得知是否发送成功
* UDP的好处就是简单，节省，经济

### 主要类：
1.DatagramSocket：通讯的数据管道
* send 和receive方法
* 绑定一个IP和Port
2.DatagramPacket
* 集装箱：封装数据
* 地址标签：目的地IP+Port
注意：接收方必须早于发起方执


## TCP编程
TCP协议：有链接、保证可靠的无误差通讯
### TCP运行步骤
1.服务器创建一个ServerSocket，等待连接
3.服务器ServerSocket接收到连接，创建一个Socket和客户的Socket建立专线连接，后续服务器和客户机的对话(这一对Socket)会在一个单独的线程（服务端）上运行
4.服务器的ServerSocket继续等待连接，返回第一步
* 服务端等待响应时，处于阻塞状态
* 服务端可以同时响应多个客户端
* 服务端每接受一个客户端，就启动一个独立的线程与之对应
* 客户端或者服务端都可以选择关闭这对Socket的通道
注意：
服务端先启动，且一直保留
客户端后启动，可以先退出
### 主要类
1.ServerSocket
* 需要绑定port
* 如果有多块网卡，需要绑定一个IP地址
2.Socket
* 客户端需要绑定服务器的地址和Port
* 客户端往Socket输入流写入数据，送到服务端
* 客户端从Socket输出流取服务器端过来的数据


## Java的Http编程
### Java HTTP编程 (java.net包)
* 支持模拟成浏览器的方式去访问网页
* URL , Uniform Resource Locator，代表一个资源
* URLConnection
* 获取资源的连接器
* 根据URL的openConnection()方法获得URLConnection
* connect方法，建立和资源的联系通道
* getInputStream方法，获取资源的内容

# 作业
```
public class Main {//主函数
    public static void main(String[] args) throws InterruptedException {
        ServerTest server = new ServerTest();
        ClientTest client1 = new ClientTest("客户端1");
        ClientTest client2 = new ClientTest("客户端2");
        new Thread(server).start();
        new Thread(client1).start();
        Thread.sleep(1000);
        new Thread(client2).start();
    }
}

//客户端类
mport java.io.*;
import java.net.InetAddress;
import java.net.Socket;


class ClientTest implements Runnable{
    private String Name;
    public ClientTest(String name)
    {
        this.Name=name;
    }
    @Override
    public void run() {
        try {
            Socket s = new Socket(InetAddress.getByName("127.0.0.1"), 8001);
            sendMessage sent = new sendMessage(s,Name);
            receiveMessage receive = new receiveMessage(s,Name);
            new Thread(sent).start();
            new Thread(receive).start();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
class sendMessage extends Thread
{
    private Socket socket =null ;
    public String clientName = null;

    public sendMessage(Socket socket, String clientName) {
        this.socket = socket;
        this.clientName = clientName;
    }

    @Override
    public void run() {

            OutputStream ops = null;  //开启通道的输出流
            try {
                ops = socket.getOutputStream();
                DataOutputStream dos = new DataOutputStream(ops);
                while (true)
                {
                    String message = clientName+":我发送了一条消息"+ System.getProperty("line.separator");
                    System.out.println(message);
                    ops.write(message.getBytes());
                    Thread.sleep(5000);
                }

            } catch (IOException | InterruptedException e) {
                e.printStackTrace();
            }
        }

}

class receiveMessage extends Thread
{
    private Socket socket =null ;
    public String clientName = null;

    public receiveMessage(Socket s,String name)
    {
        this.socket = s;
        this.clientName=name;
    }

    @Override
    public void run() {

        while (true)
        {
            InputStream ips = null;    //开启通道的输入流
            try {
                ips = socket.getInputStream();
                BufferedReader brNet = new BufferedReader(new InputStreamReader(ips));
                while(true)
                {
                    String message = null;
                    if((message = brNet.readLine())!=null)
                        System.out.println(clientName+":我收到了一条消息,"+message);
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

//服务器类
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

public class ServerTest implements Runnable {

    @Override
    public void run() {
        try {
            ServerSocket ss = new ServerSocket(8001);
            while(true)
            {
                Socket s = ss.accept();

                new Thread(new Worker(s)).start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
import java.io.*;
import java.net.Socket;
import java.util.ArrayList;
import java.util.List;

public class Worker implements Runnable{

    public static List<Socket> list = new ArrayList<>();
    private Socket s;
    Boolean flag = false;

    public Worker(Socket s) {
        this.s = s;
        list.add(s);
    }

    @Override
    public void run() {
        try {
            InputStream ips=s.getInputStream();     //有人连上来，打开输入流
            BufferedReader br = new BufferedReader(new InputStreamReader(ips));

            String message;
            while (true)
            {
                message = br.readLine()+ System.getProperty("line.separator");
                System.out.println("服务器收到一条消息->"+message);//输出客户端发送的消息
                sent(message);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public void sent(String message)
    {
        for (int i = 0; i < list.size(); i++) {
            Socket s =list.get(i);
            OutputStream ops= null;   //打开输出流
            try {
                ops = s.getOutputStream();
                ops.write(message.getBytes());
            } catch (IOException e) {
                e.printStackTrace();
            }


        }
    }
}


```

## RMI介绍实例

​      webservice主要是可以跨语言实现项目间的方法调用 ，rmi只是java内部语言进行的远程方法调用。这里我们把远程这个概念用服务端表示，调用者用客户端表示。

​     它的底层是由socket和java序列化和反序列化支撑起来的，它具体的调用过程如下图所示。而远程对象stub和skeleton负责了对象和数据参数返回值的打包和序列化与反序列化  

​    首先Remote接口用于标示其方法可以从非本地虚拟机上调用的接口。任何远程对象都必须直接或者间接实现此接口。在"远程接口"（扩展java.rmi.Remote的接口）中指定的这些方法才可以远程使用。也就是说要远程调用的方法必须在扩展remote接口的接口中声明，并且要抛出RemoteException异常才能被远程调用。远程对象必须实现java.server.UniCastRemoteObject。这样才能保证客户端访问获得远程对象的时候，该远程对象将会把自身的一个拷贝序列化后以socket的形式发送给客户端，此时客户端就会获得这个拷贝作为存根stub，而服务器本身已存在的远程对象称之为骨架。 

![img](D:\study\github\github-track\doc\rpc\image\rmi001) 



代码

服务端接口

```java
public interface IHelloService extends Remote{

    public String sayHelllo(String msg) throws RemoteException;
}
```

实现类

```java
public class HelloServiceImpl extends UnicastRemoteObject implements IHelloService {
    protected HelloServiceImpl() throws RemoteException {
        super();
    }
    @Override
    public String sayHelllo(String msg) {
        return "Hello:"+msg;
    }
}
```

服务端绑定类

```java
public class Server {

    public static void main(String[] args) {
        try {
            IHelloService helloService = new HelloServiceImpl();//创建服务 HelloServiceImpl_stub   继承了UnicastRemoteObject  Remote
            LocateRegistry.createRegistry(1099);//启动服务  相当于注册中心  向注册中心注册
            try {
                Naming.rebind("rmi://127.0.0.1:1099/Hello",helloService);  //绑定服务  基于socket 进行转递服务
                System.out.println("发布对象");
            } catch (MalformedURLException e) {
                e.printStackTrace();
            }
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }
}
```

客户端调用类

```java
public class ClientDemo {
    public static void main(String[] args) throws RemoteException, NotBoundException, MalformedURLException {
        //向服务中心调用服务通过URI 来获取
        IHelloService helloService =(IHelloService) Naming.lookup("rmi://127.0.0.1:1099/Hello");//有一个stub
        System.out.println(helloService.sayHelllo("YFG"));
    }
}
```
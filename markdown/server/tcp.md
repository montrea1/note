# TCP

### 通信步骤
服务器端：ServerSocket   指定端口并且处理监听  
客户端：Socket  向服务器端发送请求  

``` java
/****  服务器端
    1.创建serversocket(int port)对象
    2.在socket上监听客户端的连接请求
    3.堵塞，等待连接建立
    4.接收并处理请求信息
    5.将处理结果返回给客户端
    6.关闭流和socket对象
****/

/****  客户端
    1.创建socket(String host,int port)对象
    2.向服务器发送连接请求
    3.向服务器发送服务请求
    4.接收服务器端返回结果
    5.关闭流和socket对象
****/
```
服务器端
```java
public class Server {
    public static void main(String[] args) throws IOException {
        System.out.println("------server-------");
        //1.指定服务器端口
        ServerSocket server=new ServerSocket(8888);

        //2.等待客户端连接
        Socket client=server.accept();
        System.out.println("a client accepted ...");

        //接收客户端发送的请求
        DataInputStream dataInputStream=new DataInputStream(client.getInputStream());
        String data=dataInputStream.readUTF();
        System.out.println("getMsg:"+data);

        dataInputStream.close();
        client.close();
    }
}

```
客户端
``` java
public class Client {
    public static void main(String[] args) throws IOException {
        System.out.println("------client-------");

        //1.创建客户端,指定服务器地址端口
        Socket client=new Socket("localhost",8888);

        //2.向服务器端发送请求
        DataOutputStream dataOutputStream=new DataOutputStream(client.getOutputStream());
        String msg="hello";
        dataOutputStream.writeUTF(msg);
        dataOutputStream.flush();

        dataOutputStream.close();
        //释放资源
        client.close();
    }
}

```

### 应用

文件传输
```java
public class FileServer {
    public static void main(String[] args) throws IOException {
        System.out.println("======server======");
        ServerSocket server=new ServerSocket(8888);

        Socket client= server.accept();
        System.out.println("a client accepted");

        InputStream is=new BufferedInputStream(client.getInputStream());
        OutputStream os=new BufferedOutputStream(new FileOutputStream("abc.txt"));

        byte[] flush=new byte[1024*60];
        int len=-1;
        while ((len=is.read(flush))!=-1){
            os.write(flush,0,len);
        }

        os.flush();
        os.close();
        is.close();
        client.close();
    }
}

```

```java
public class FileClient {
    public static void main(String[] args) throws IOException {
        System.out.println("=====client=====");
        Socket client=new Socket("localhost",8888);

        InputStream is=new BufferedInputStream(new FileInputStream("target.txt"));
        OutputStream os=new BufferedOutputStream(client.getOutputStream());

        byte[] flush=new byte[1024*60];
        int len=-1;
        while ((len=is.read(flush))!=-1){
            os.write(flush,0,len);
        }

        //释放资源
        os.flush();
        os.close();
        client.close();
    }
}

```

用户登录
```java
public class LoginServer {
    public static void main(String[] args) throws IOException {
        System.out.println("=====server=====");
        ServerSocket server=new ServerSocket(8888);

        Socket client= server.accept();
        System.out.println("a client accepted");

        //接收客户端数据
        DataInputStream dataInputStream=new DataInputStream(client.getInputStream());
        String data=dataInputStream.readUTF();

        String[] dataArray=data.split("&");
        String username="";
        String password="";
        //解析客户端数据
        for (String info :dataArray){
            String[] userInfo=info.split("=");
            if(userInfo[0].equals("username")){
                System.out.println("username:"+userInfo[1]);
                username=userInfo[1];
            }else if(userInfo[0].equals("password")){
                System.out.println("password:"+userInfo[1]);
                password=userInfo[1];
            }
        }

        //响应处理
        DataOutputStream dataOutputStream=new DataOutputStream(client.getOutputStream());
        if("mengli".equals(username)&&"123456".equals(password)){
            String response=" login success";
            dataOutputStream.writeUTF(response);
        }else {
            String response=" login defalut";
            dataOutputStream.writeUTF(response);
        }
        dataOutputStream.flush();
        
        //释放资源
        dataOutputStream.close();
        dataInputStream.close();
    }
}
```

```java
public class LoginClient {
    public static void main(String[] args) throws IOException {
        System.out.println("=====client=====");
        BufferedReader console=new BufferedReader(new InputStreamReader(System.in));
        System.out.println("请输入用户名：");
        String username=console.readLine();
        System.out.println("请输入密码：");
        String passowrd=console.readLine();

        Socket client=new Socket("localhost",8888);

        DataOutputStream dataOutputStream=new DataOutputStream(client.getOutputStream());
        dataOutputStream.writeUTF("username="+username+"&"+"password="+passowrd);
        dataOutputStream.flush();

        DataInputStream dataInputStream=new DataInputStream(client.getInputStream());
        String msg= dataInputStream.readUTF();
        System.out.println(msg);

        dataInputStream.close();
        dataOutputStream.close();
//        client.close();
    }
}
```
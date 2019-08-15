
 # udp
 #### server
 ``` java
 public class UdpServer {
    public static void main(String[] args) throws IOException {
        System.out.println("server starting ...");
        //1.指定端口
        DatagramSocket server=new  DatagramSocket(8888);

        //2.封装DatagramPacket
        byte[] buf=new byte[1024];
        DatagramPacket receivePacket=new DatagramPacket(buf,0,buf.length);

        //3.接受信息包
        server.receive(receivePacket);

        //4.解析数据包
        byte[] datas= receivePacket.getData();
        int len=receivePacket.getLength();
        System.out.println("getClientMsg:"+new String(datas,0,len));

        //5.释放资源
        server.close();
    }
}
 ```

 #### client
 ```java
 public class UdpClient {
    public static void main(String[] args) throws IOException {
        System.out.println("client starting ...");
        //1.指定端口
        DatagramSocket client=new DatagramSocket(9999);

        //2.封装发送信息包
        String data="mengli";
        byte[] datas= data.getBytes();
        DatagramPacket packet=new DatagramPacket(datas,0,datas.length,
                new InetSocketAddress("localhost",8888));//接收方端口
        
        //3.发送信息
        client.send(packet);

        //4.释放资源
        client.close();
    }
}
 ```
#### 数据类型接收发送
``` java
/* 接收方 */
public class UdpTypeServer {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        System.out.println("server starting ...");

        DatagramSocket server =new DatagramSocket(8888);

        byte[] datas=new byte[1024*60];

        DatagramPacket receivePacket=new DatagramPacket(datas,0,datas.length);
        server.receive(receivePacket);


        //解析接收的数据类型
        //基本类型
//        DataInputStream dataInputStream=new DataInputStream(
//                new BufferedInputStream(new ByteArrayInputStream(datas)));
//        String msg=dataInputStream.readUTF();
//        int age=dataInputStream.readInt();
//        boolean flage=dataInputStream.readBoolean();
//        char ch=dataInputStream.readChar();

        //基本类型
//        System.out.println(msg);
//        System.out.println(age);
//        System.out.println(flage);
//        System.out.println(ch);

        //对象类型
//        ObjectInputStream objectInputStream=new ObjectInputStream(
//                new BufferedInputStream(new ByteArrayInputStream(datas)));
//        Object object= objectInputStream.readObject();
//
//        if (object instanceof Emp){
//            Emp emp=(Emp)object;
//            System.out.println(emp.toString());
//        }

        //文件类型
        byte[] getFile= receivePacket.getData();
        byteArrayToFile(getFile,"target.txt");
        System.out.println("接收成功");

        //关闭资源
        server.close();

    }

    public static void byteArrayToFile(byte[] src,String path){
        File file=new File(path);

        OutputStream outputStream=null;
        ByteArrayInputStream byteArrayInputStream=null;

        try {
            byteArrayInputStream=new ByteArrayInputStream(src);

            outputStream =new FileOutputStream(path);

            byte[] flush=new byte[1024];
            int len=-1;

            while ((len=byteArrayInputStream.read(flush))!=-1){
                outputStream.write(flush,0,flush.length);
            }
            outputStream.flush();

        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if(outputStream!=null){
                try {
                    outputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(byteArrayInputStream!=null){
                try {
                    byteArrayInputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}

```

```java
/* 发送方 */
public class UdpTypeClient {
    public static void main(String[] args) throws IOException {
        System.out.println("client starting ....");

        DatagramSocket client=new DatagramSocket(9999);

        //发送信息封装
        ByteArrayOutputStream byteArrayOutputStream=new ByteArrayOutputStream();

        //基本类型
//        DataOutputStream dataOutputStream=new DataOutputStream(byteArrayOutputStream);
//        dataOutputStream.writeUTF("hello server,this is client");
//        dataOutputStream.writeInt(22);
//        dataOutputStream.writeBoolean(false);
//        dataOutputStream.writeChar('m');

        //对象类型
//        Emp emp=new Emp("mengli",22);
//        ObjectOutputStream objectOutputStream=new ObjectOutputStream(byteArrayOutputStream);
//        objectOutputStream.writeObject(emp);

        //发送打包
//        byte[] sendData=byteArrayOutputStream.toByteArray();
//        DatagramPacket datagramPacket=new DatagramPacket(sendData,0,sendData.length,
//                new InetSocketAddress("localhost",8888));

        //文件类型
        byte[] sendFile= fileToByteArray("abc.txt");
        DatagramPacket datagramPacket=new DatagramPacket(sendFile,0,sendFile.length,
                new InetSocketAddress("localhost",8888));


        //发送信息
        client.send(datagramPacket);

        //关闭资源
        client.close();
    }


    public static byte[] fileToByteArray(String path){
        File file=new File(path);

        InputStream inputStream=null;
        ByteArrayOutputStream byteArrayOutputStream=null;

        try {
            inputStream=new FileInputStream(file);
            byteArrayOutputStream=new ByteArrayOutputStream();
            byte[] flush=new byte[1024*60];
            int len=-1;
            while ((len=inputStream.read(flush))!=-1){
                byteArrayOutputStream.write(flush,0,flush.length);
            }
            byteArrayOutputStream.flush();
            return byteArrayOutputStream.toByteArray();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                if (byteArrayOutputStream!=null){
                    byteArrayOutputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

            try {
                if(inputStream!=null){
                    inputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return null;
    }


}
```

#### 多线程
```java
/* 接收端 */
public class UdpThreadServer implements Runnable{
    private DatagramSocket server;
    private BufferedReader bufferedReader;
    private String from;

    public UdpThreadServer(int port,String from) {
        this.from=from;
        try {
            server=new DatagramSocket(port);
        } catch (SocketException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void run() {
        //构建接收包
        byte[] flush=new byte[1024*60];
        DatagramPacket packet=new DatagramPacket(flush,0,flush.length);

        while (true){
            try {
                server.receive(packet);
                //解析接收数据
                byte[] datas=packet.getData();
                int len=packet.getLength();
                String msg=new String(datas,0,len);


                System.out.println(from+":"+msg);
                if (msg.equals("bye")){
                    break;
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        server.close();
    }
}

```

```java
/* 发送端 */
public class UdpThreadClient implements Runnable{
    private DatagramSocket client;
    private BufferedReader bufferedReader;
    private DatagramPacket packet;
    private String toIP;
    private int toPort;

    public UdpThreadClient(int port,String toIP,int toPort) {
        this.toIP=toIP;
        this.toPort=toPort;
        try {
            client=new DatagramSocket(port);
            bufferedReader=new BufferedReader(new InputStreamReader(System.in));
        } catch (SocketException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void run() {
        while (true){
            try {
                //封装发送数据
                String data=bufferedReader.readLine();
                byte[] datas=data.getBytes();
                packet=new DatagramPacket(datas,0,datas.length,
                        new InetSocketAddress(toIP,toPort));
                //发送数据包
                client.send(packet);
                if(data.equals("bye")){
                    break;
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        //释放资源
        client.close();
    }
}

```
调用类
```java
public class TalkStudent {
    public static void main(String[] args) {
        new Thread(new UdpThreadClient(7777,"localhost",9999)).start();
        new Thread(new UdpThreadServer(8888,"老师")).start();
    }
}


public class TalkTeacher {
    public static void main(String[] args) {
        new Thread(new UdpThreadServer(9999,"学生")).start();
        new Thread(new UdpThreadClient(6666,"localhost",8888)).start();
    }
}

```
# Socket

socket分类

-   udp
    -   不区分客户端/服务器端
    -   面向无连接,数据不安全
    -   速度快
-   tcp
    -   端对端传输,区分客户端服/务器端
    -   面向连接(三次握手),数据安全
        -   三次握手:client  Request→ server  server  Response→ client  数据传输
    -   速度相对慢

#### UDP

-   数据以包的形式发送/接收

**发送端**

```java
public class UdpClient {
    public static void main(String[] args) throws IOException {
        System.out.println();
        DatagramSocket client= new DatagramSocket(8888);
        BufferedReader bufferedReader=new BufferedReader(new InputStreamReader(System.in));

        while (true){
            String msg=bufferedReader.readLine();
            byte[] flush=msg.getBytes();
            DatagramPacket packet=new DatagramPacket(flush,0,flush.length,
                    new InetSocketAddress("localhost",9999));
            client.send(packet);
            if (msg.equals("bye")){
                break;
            }
        }
        client.close();
    }
}

```

**接收端**

```java
public class UdpServer {
    public static void main(String[] args) throws IOException {
        System.out.println("server starting ---");
        DatagramSocket server =new DatagramSocket(9999);

        while (true){
            byte[] flush=new byte[1024*60];
            DatagramPacket packet=new DatagramPacket(flush,0,flush.length);
            server.receive(packet);
            byte[] data=packet.getData();
            String msg=new String(data,0,data.length);
            System.out.println("msg="+msg);
            if(msg.equals("bye")){
                break;
            }
        }
         server.close();
    }
}

```

TCP

-   数据以流的形式接收/发送
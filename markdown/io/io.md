# IO流

### 四大抽象类
    字节流
    InputStream
        read
        close
    OutputStream
        write
        flush
        close

    字符流
    Reader
        read
        close
    Writer
        writer
        flush
        close

### 操作流程
    创建源
    选择流
    操作
    释放资源

``` java
public class IoTest {
    public static void main(String[] args)  {
        //1.创建源
        File file=new File("abc.txt");
        //2.选择流
        InputStream inputStream = null;
        try {
            inputStream =new FileInputStream(file);

            //3.操作
            int temp;
            while ((temp=inputStream.read())!=-1){
                System.out.print((char)temp);
            }

            //4.释放资源
            inputStream.close();

        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                if (null!=inputStream){//close
                    inputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```
``` java
    /* 指定flush */
    public class IoFlushTest {
    public static void main(String[] args) {
        //创建源
        File file=new File("abc.txt");
        //选流
        InputStream inputStream=null;

        //操作
        try {
            inputStream=new FileInputStream(file);

            //接收长度 flush
            byte[] buf=new byte[1024];
            int len=-1;

            while ((len=inputStream.read(buf))!=-1){
                String string=new String(buf,0,len);
                System.out.print(string);
            }

            inputStream.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (inputStream!=null){
                try {
                    inputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```

``` java

/*文件字节输入流*/
public class IoFileByteWriterTest {
    public static void main(String[] args) {
        //创建源
        File file =new File("abc.txt");

        //选择流
        OutputStream outputStream=null;

        try {
            //操作
            outputStream=new  outputStream=new FileOutputStream(file,true);

            String data="hello";
            byte[] datas= data.getBytes();

            outputStream.write(datas,0,datas.length);
            outputStream.flush();

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {//释放资源
            if (outputStream!=null){
                try {
                    outputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}
```

``` java
/*文件拷贝*/
public class IoCopy {
    public static void main(String[] args) {
        //源
        File source=new File("abc.txt");
        File target=new File("target.txt");

        //流
        InputStream inputStream=null;
        OutputStream outputStream=null;

        //操作
        try {
            inputStream=new FileInputStream(source);
            outputStream=new FileOutputStream(target);

            byte[] flush=new byte[1024];
            int len=-1;
            while ((len=inputStream.read(flush))!=-1){
                outputStream.write(flush,0,len);
            }
            outputStream.flush();

        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                if (outputStream!=null){
                    outputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

            try {
                if (inputStream!=null){
                    inputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### 文件字符流
``` java
/*文件字符流*/
public class IoFileStream {
    public static void main(String[] args) {
        File file=new File("abc.txt");
//        reader(file);
        writer(file);
    }

    /*read*/
    public static void reader(File file){
        Reader reader=null;

        try {
            reader=new FileReader(file);

            char[] flush=new char[1024];
            int len=-1;

            while ((len=reader.read(flush))!=-1){
                String string = new String (flush,0,len);
                System.out.println(string);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(reader!=null){
                try {
                    reader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }


    public static void writer(File file){
        FileWriter fileWriter=null;
        File target =new File("target.txt");
        try {

            char[] datas= readerUtil(file);

            fileWriter =new FileWriter(target);
            fileWriter.write(datas,0,datas.length);
            fileWriter.flush();

        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (fileWriter!=null){
                try {
                    fileWriter.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }


    public static char[] readerUtil(File file){
        FileReader fileReader=null;
        char[] datas=null;
        try {
            fileReader = new FileReader(file);

            char[] buf=new char[1024];
            int len=-1;
            String msg="";
            while ((len=fileReader.read(buf))!=-1){
                msg=new String(buf,0,buf.length);
            }
            datas = msg.toCharArray();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (fileReader!=null){
                try {
                    fileReader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            return datas;
        }
    }
}
```

#### 字节数组流
``` java
/*字节数组流*/
public class IoByteArrayStream {
    public static void main(String[] args) {
        byte[] datas=fileToByteArray("abc.txt");
        System.out.println(datas.length);
        byteArrayToFile(datas,"target.txt");
    }

    public static byte[] fileToByteArray(String path){
        File file=new File(path);

        InputStream inputStream=null;
        ByteArrayOutputStream byteArrayOutputStream=null;

        try {
            inputStream=new FileInputStream(file);
            byteArrayOutputStream=new ByteArrayOutputStream();
            byte[] flush=new byte[1024];
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
#### 缓冲流

```java
public class IonBufferStream {
    public static void main(String[] args) {
        inputStream();
        outputStream();
    }

    public static void inputStream(){
        File file =new File("abc.txt");

        InputStream inputStream=null;

        try {
            inputStream=new BufferedInputStream(new FileInputStream(file));

            byte[] flush=new byte[1024];
            int len=-1;

            while ((len=inputStream.read(flush))!=-1){
                String string=new String(flush,0,flush.length);
                System.out.println(string);
            }

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                if(inputStream!=null){
                    inputStream.close();
                 }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }

    public static void outputStream(){
        File file=new File("abc.txt");
        File target=new File("target.txt");

        InputStream inputStream=null;
        OutputStream outputStream=null;

        try {
            inputStream=new BufferedInputStream(new FileInputStream(file));
            outputStream=new BufferedOutputStream(new FileOutputStream(target));

            byte[] flush=new byte[1024*10];
            int len=-1;
            while ((len=inputStream.read(flush))!=-1){
                outputStream.write(flush,0,flush.length);
            }
            outputStream.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                if(inputStream!=null){
                    inputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

            try {
                if (outputStream!=null){
                    outputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### 字符缓冲流
``` java
public class IoBufferedStream {
    public static void main(String[] args) {
        reader();
        writer();
    }

    public static void reader(){
        File file=new File("abc.txt");

        BufferedReader bufferedReader=null;

        try {
            bufferedReader=new BufferedReader(new FileReader(file));
            String line=null;
            while ((line=bufferedReader.readLine())!=null){
                System.out.println(line);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                if (bufferedReader!=null){
                    bufferedReader.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }


    }

    public static void writer(){
        File file=new File("abc.txt");
        File tartget=new File("target.txt");

        BufferedReader bufferedReader=null;
        BufferedWriter bufferedWriter=null;

        try {
            bufferedReader=new BufferedReader(new FileReader(file));
            bufferedWriter=new BufferedWriter(new FileWriter(tartget));

            String line=null;
            while ((line=bufferedReader.readLine())!=null){
                bufferedWriter.write(line);
                bufferedWriter.newLine();

            }
            bufferedWriter.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                if(bufferedReader!=null){
                    bufferedReader.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

            try {
                if(bufferedWriter!=null){
                    bufferedWriter.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
```

#### 转换流
```java
public class IoConverteStream {
    public static void main(String[] args) {
        demoNetStream();
    }

    public static void demo(){
        try(BufferedReader bufferedReader=new BufferedReader(new InputStreamReader(System.in));
            BufferedWriter bufferedWriter=new BufferedWriter(new OutputStreamWriter(System.out));
        ) {
            String msg="";
            while (!msg.equalsIgnoreCase("exit")){
                msg=bufferedReader.readLine();
                bufferedWriter.write(msg);
                bufferedWriter.newLine();
                bufferedWriter.flush();
            }

        } catch (IOException e) {
            e.printStackTrace();
            System.out.print("system error");
        }
    }


    public static void demoNetStream(){
        File target =new File("target.txt");
        BufferedWriter bufferedWriter=null;

        try (BufferedReader reader= new BufferedReader(new InputStreamReader(
                new URL("http://www.baidu.com").openStream(),"utf-8"))){

            bufferedWriter= new BufferedWriter(new OutputStreamWriter(new FileOutputStream(target)));

            String line=null;
            while ((line=reader.readLine())!=null){
                System.out.print(line);
                bufferedWriter.write(line);
                bufferedWriter.newLine();
                bufferedWriter.flush();
            }

        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                if(bufferedWriter!=null){
                    bufferedWriter.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

}

```

#### 数据流
```java
public class IoDataStream {
    public static void main(String[] args) throws IOException {
        ByteArrayOutputStream byteArrayOutputStream=new ByteArrayOutputStream();
        DataOutputStream dataOutputStream=new DataOutputStream(byteArrayOutputStream);

        dataOutputStream.writeUTF("hardcode");
        dataOutputStream.writeInt(18);
        dataOutputStream.writeBoolean(false);
        dataOutputStream.writeChar('c');
        dataOutputStream.flush();

        byte[] datas=byteArrayOutputStream.toByteArray();

        DataInputStream dataInputStream=new DataInputStream(new ByteArrayInputStream(datas));
        String msg=dataInputStream.readUTF();
        int code=dataInputStream.readInt();
        boolean flag=dataInputStream.readBoolean();
        char target=dataInputStream.readChar();
        System.out.println(code);
    }
}

```

#### 对象流
```java
public class IoObjectStream {
    public static void main(String[] args) throws IOException, ClassNotFoundException {

        ByteArrayOutputStream byteArrayOutputStream=new ByteArrayOutputStream();
        ObjectOutputStream objectOutputStream=new ObjectOutputStream(new BufferedOutputStream(byteArrayOutputStream));

        Emp emp=new Emp("mengli",22);
        objectOutputStream.writeObject(emp);
        objectOutputStream.flush();

        byte[] bytes=byteArrayOutputStream.toByteArray();
        ObjectInputStream objectInputStream=new ObjectInputStream(new BufferedInputStream(new ByteArrayInputStream(bytes)));

        Object e=objectInputStream.readObject();
        if(e instanceof Emp){
            Emp empObj=(Emp)e;
            System.out.println(empObj);
        }


    }

}
class Emp implements Serializable {
    private String name;
    private int age;

    public Emp() {
    }

    public Emp(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Emp{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

#### 打印流
```java
public class IoPrintStream {
    public static void main(String[] args) throws FileNotFoundException {
        PrintStream ps=System.out;
        ps.println("printStream");
        ps.println(true);

        ps=new PrintStream(new BufferedOutputStream(new FileOutputStream("print.txt")),true);
        ps.println("print");
        ps.println(true);

        //设置system.out的输出位置
        System.setOut(ps);
        System.out.println("change target");

        //设置打印到控制台
        System.setOut(new PrintStream(new BufferedOutputStream(new FileOutputStream(FileDescriptor.out)),true));
        System.out.println("change  console");

        ps.close();
    }
}

```
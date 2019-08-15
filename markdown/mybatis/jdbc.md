## jdbc
``` java
public class TestJdbc {
    //1.加载驱动
    private static String DRIVER;
    private static String URL;
    private static String USER;
    private static String PASSWORD;

    //2.获取数据库连接
    static {
        try{
            DRIVER="com.mysql.jdbc.Driver";
            URL="jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=utf8";
            USER="root";
            PASSWORD="123456";
            Class.forName(DRIVER);
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    //2获取链接
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL,USER,PASSWORD);
    }

    //3.执行语句
    public static void query()throws Exception{
        String sql="select * from person";
        Connection connection=getConnection();
        connection.setAutoCommit(false);
        PreparedStatement p=connection.prepareCall(sql);

        ResultSet result=p.executeQuery();
        while (result.next()){
            String name=result.getString("name");
            System.out.println(name);
        }
        connection.commit();
        connection.clearWarnings();
    }

    public static void main(String[] args) throws Exception {
        query();
    }

}
```
#java base

### lambda
特性：允许把**函数**作为一个**方法的参数**
java 8 内部Lambda 表达式的实现方式在本质是**以匿名内部类的形式**的实现


#### 表达形式
```java
    // 1、方法体为表达式，该表达式的值作为返回值返回
    (parameters) -> expression
    (int a,int b) -> return a + b; //求和

    //2、方法体为代码块，必须用 {} 来包裹起来，且需要一个 return 返回值，但若函数式接口里面方法返回值是 void，则无需返回值。
    (parameters) -> { statements; }
    (int a) -> {System.out.println("a = " + a);} //打印，无返回值
    (int a) -> {return a * a;} //求平方
```

#### 应用场景
##### 1、使用() -> {} 替代匿名类
```java
Thread t1 = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("no use lambda");
    }
});
          
Thread t2 = new Thread(() -> System.out.println("use lambda"));
```


        

### 运算符

**来源**： https://cloud.tencent.com/developer/article/1338265

**按位与 &**
```java
private static void AND() {
        /**
        * &按位与的运算规则是将两边的数转换为二进制位，然后运算最终值.
        * 1&1=1 , 1&0=0 , 0&1=0 , 0&0=0
        * 3=11 , 5=101 , 7=111
        *   011     111
        * & 101   & 101
        *   001     101
        *     1       5
        * */
        int i=3&5;
        int y=5&7;
        System.out.println("i="+i+" y="+y);
    }
```

**逻辑与 &&**
```java
    private static void ANL() {
        /**
         * &&逻辑与也称为短路逻辑与，
         * 先运算&&左边的表达式，
         * 一旦为假，后续不管多少表达式，均不再计算，
         * 一个为真，再计算右边的表达式，两个为真才为真。
         */
        String str="";
        if ((str != null)&&(3>5)) {
            System.out.println("true");
        }else {
            System.out.println("false");
        }
    }
```
**按位或 |**
```java
    private static void bitwiseOR() {
        /**
         * |按位或和&按位与计算方式都是转换二进制再计算
         * 1|0 = 1 , 1|1 = 1 , 0|0 = 0 , 0|1 = 1
         * 2=10 5=101 6=110 7=111
         *  010     101
         * |110    |111
         *  110     111
         *    6       7
         */
        int i = 2 | 6;
        int y = 5 | 7;
        System.out.println("i=" + i + " y=" + y);
    }
```
**逻辑或 ||**
```java
  private static void ORL() {
        /**
         * 逻辑或|| 
         * 一个为真即为真，后续不再计算。
         * 一个为假再计算右边的表达式。
         */
        if(3>5||3<5){
            System.out.println(true);
        }
    }
```
**异运算 ^**
```java
  private static void XOR() {
        /**
         * 异运算
         * ^运算规则为1^0 = 1 , 1^1 = 0 , 0^1 = 1 , 0^0 = 0
         * 5=101
         *  0101
         * ^1001
         *  1100
         *    12
         */
        int i=5^9;
        System.out.println(i);
    }
```
**左移运算 <<**
```java
    private static void LSO() {
        /**
         * 左移运算
         * 5=101
         * 5<<2
         *   00101
         *   <<  2
         *   10100
         *      20
         */
        int i = 5 << 2;
        System.out.println(i);
    }
```
**右移运算 >>**
```java
 private static void RSO() {
        /**
         * 右移运算
         * 5=101
         * 5>>2
         *   00101
         *   >>  2
         *   00001
         *       1
         */
        int i = 5 >> 2;
        System.out.println(i);
    }
```
**取反运算 ~**
```java
private static void InverseOperation(){
        /**
         * 取反运算~
         * 5=101
         * 0000 0101
         * ~
         * 1111 1010
         *        -6
         */
        int i=~5;
        System.out.println(i);
    }
```
**无符号右移 >>>**
```java
 private static void UnsignedRightShiftOperator(){
        /**
         * 无符号右移 >>>
         * 负数无符号右移：
         * -6=1111 1111 1111 1111 1111 1111 1111  1111 1010
         * 1111 1111 1111 1111 1111 1111 1111  1111 1010
         *  >>>
         * 0001 1111 1111 1111 1111 1111 1111  1111 1111
         *
         */
        int i=-6>>>3;
        System.out.println("i:"+i);
    }
```


### hashMap遍历

```java
public class TestHM {
    public static void main(String[] args) {
        Map<Integer,String> map=new HashMap<>();
        String[] chars={"a","b","c","d","e"};
        for (int i = 0; i < chars.length; i++) {
            map.put(i,chars[i]);
        }

        keySetErgodic(map);
        entryIteratorErgodic(map);
        entryErgodic(map);
        valuesErgodic(map);
    }
}

```
**keySetErgodic**
```java
    private static void keySetErgodic(Map<Integer, String> map) {
        for (Integer key:map.keySet()) {
            System.out.println("key:"+key+" value:"+map.get(key));
        }
    }
```
**entryIteratorErgodic**
```java
    private static void entryIteratorErgodic(Map<Integer, String> map) {
        Iterator iterator=map.entrySet().iterator();
        while (iterator.hasNext()){
            Map.Entry<Integer,String>entry= (Map.Entry<Integer, String>) iterator.next();
            System.out.println("key:"+entry.getKey()+" value:"+entry.getValue());
        }
    }

```
**entryErgodic**
```java
    private static void entryErgodic(Map<Integer, String> map) {
        for (Map.Entry<Integer,String> entry:map.entrySet()) {
            System.out.println("key:"+entry.getKey()+" value:"+entry.getValue());
        }
    }
```
**valuesErgodic**
```java
    private static void valuesErgodic(Map<Integer, String> map) {
        for (String value:map.values()) {
            System.out.println("value:"+value);
        }
    }
```
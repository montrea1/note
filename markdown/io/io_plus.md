##io进阶

### 装饰器模式

**作用：**

-   在原有类基础上进行加强

**优点**

-   耦合性低
-   被装饰类的变化,与装饰类的变化无关

**代码**

```java
interface Coder{
    public void code();
}

/*被装饰类*/
class Student implements Coder{
    @Override
    public void code() {

    }
}

/*装饰类*/
class StudentPlus implements Coder{
  	//1.获取装饰类的引用
    private Student s;
	
  	//2.通过构造器获取被装饰对象
    public StudentPlus(Student s) {
        this.s = s;
    }
	
  	//3.装饰.对原有类进行升级
    @Override
    public void code() {
        //被装饰
        s.code();
        //装饰内容/功能
        System.out.println("ssm");
    }
}
```






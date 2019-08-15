##集合使用

#### List去重
```java
public class ArrList {

	public static void main(String[] args) throws NoSuchFieldException {
		List<Person> newList=new ArrayList<>();
		List<Person> p=new ArrayList<>();
		for (int i = 0; i < 2; i++) {
			Person person=new Person();
			person.setName("a"+i);
			person.setAge(i);
			p.add(person);
		}
		for (int i = 0; i < 3; i++) {
			Person person=new Person();
			person.setName("a"+i);
			person.setAge(i);
			p.add(person);
		}

		//list去重
		ListIterator<Person> itPerson=p.listIterator();
		while (itPerson.hasNext()){
			Person person= itPerson.next();
			if(!newList.contains(person)){
				newList.add(person);
			}
		}

		//set去重
		HashSet<Person> hashSet=new HashSet<>(p);
		Set<Person> set=hashSet;

		Iterator<Person> iteratorSet= set.iterator();
		while (iteratorSet.hasNext()){
			newList.add(iteratorSet.next());
		}



		ListIterator<Person> iterator=newList.listIterator();
		while (iterator.hasNext()){
			System.out.println(iterator.next().getName());
		}

	}
}
	
```

### 集合转数组

```java
public class ArrList {
    public static void main(String[] args) throws NoSuchFieldException {
        Integer[] arr={1,2,3,4,5,6,7};
        List arrList= Arrays.asList(arr);
    }
}
        
```

### HashSet保证唯一

1.对比hashcode

2.hashcode相同使用equals()

```java
/*实体类*/
public class Person {
    private String name;
    private Integer age;
}
```

- 重写hashcode()
```java
@Override
public int hashCode() {
  return Objects.hash(name, age);
}
```
- 重写equals()
```java
 public boolean equals(Object o) {
   if (this == o) return true;
   if (o == null || getClass() != o.getClass()) return false;
   Person person = (Person) o;
   return Objects.equals(name, person.name) &&
     Objects.equals(age, person.age);
 }
```

-   hashcode
    -   属性相同,返回值必须相同
    -   属性不同,返回值尽量不同(提高效率)
-   equals
    -   属性相同,true,不存
    -   属性不同,false,存入



### LinkedHashSet

**保证存储顺序**

```java
public class MySet {
    public static void main(String[] args) {
        LinkedHashSet set=new LinkedHashSet();
        HashSet<Integer> hashSet=new HashSet<>();
        for (int i = 7; i < 10; i++) {
            set.add(i);
            hashSet.add(i);
        }
        for (int i = 0; i < 5; i++) {
            set.add(i);
            hashSet.add(i);
        }
        System.out.println(set);
        System.out.println(hashSet);
    }
}

```

```java
[7, 8, 9, 0, 1, 2, 3, 4]	LinkedHashSet
[0, 1, 2, 3, 4, 7, 8, 9]	HashSet
```

### TreeSet实现排序

**实现Comparable接口**

```java
 public int compareTo(Object o) {
   return 0;
 }
```

```java
public int compareTo(Person o) {
  int num=this.age-o.age;
  return num==0?this.name.compareTo(o.name):num;
}
```

-   返回 0

    -   集合只有一个元素

-   返回正数

    -   集合怎么存怎么取

-   返回负数

    -   集合倒序排序,与存入顺序相反

    ​


### Map集合

map集合针对键有效,collection对元素有效



**HashMap HashTable**

-   相同
    -   hash算法、双列集合
-   不同
    -   HashMap
        -   线程不安全
        -   允许null键&值
    -   Hashtable
        -   线程安全
        -   不允许null键/值



### Collections工具类

-   排序 sort

    -   ```java
        Collections.sort(list);
        ```

-   二分查找 binarySearch

    -   需要先排序

    -   ```java
        Collections.sort(list);
        int index= Collections.binarySearch(list,4); //list & key getIndex
        ```

-   最大值 max

    -   ```java
        int max= Collections.max(list);
        ```

-   反转 reverse

    -   ```java
        Collections.reverse(list);
        ```



### 迭代器 iterator

**迭代→遍历**

**方法 (Arraylist)**

-   hasNext

    -   ```java
        public boolean hasNext() {
          return cursor != size;
        }
        ```

-   next

    -    **cursor = i + 1;**

    -   ```java
         public E next() {
           checkForComodification();
           int i = cursor;
           if (i >= size)
             throw new NoSuchElementException();
           Object[] elementData = ArrayList.this.elementData;
           if (i >= elementData.length)
             throw new ConcurrentModificationException();
           cursor = i + 1;
           return (E) elementData[lastRet = i];
         }
        ```

-   remove

    -   ```java
         public void remove() {
           if (lastRet < 0)
             throw new IllegalStateException();
           checkForComodification();

           try {
             ArrayList.this.remove(lastRet);
             cursor = lastRet;
             lastRet = -1;
             expectedModCount = modCount;
           } catch (IndexOutOfBoundsException ex) {
             throw new ConcurrentModificationException();
           }
         }
        ```

        ​

    ​










# 泛型

## 泛型的使用
泛型有三种使用方式，分别为：泛型类、泛型接口、泛型方法

### 泛型类
```java
Class 类名称 <泛型标识：可以随便写任意标识号，标识指定的泛型的类型>{
   private 泛型标识 /*（成员变量类型）*/ var; 
   .....
   }
 }
```

### 泛型接口
```java
//定义一个泛型接口
public interface Generator<T> {
     public T next();
 }
```

### 泛型方法
```java
//这才是一个真正的泛型方法。
//首先在public与返回值之间的<T>必不可少，这表明这是一个泛型方法，并且声明了一个泛型T
//这个T可以出现在这个泛型方法的任意位置.
//泛型的数量也可以为任意多个 
public <T,K> K showKeyName(Generic<T> container){
  K k=container.get();
  return k;
}
```
https://www.cnblogs.com/fnlingnzb-learner/p/7265104.html

## 通配符? 与 T 的区别
- T：作用于模板上，用于将数据类型进行参数化，不能用于实例化对象。
- ?：在实例化对象的时候，不确定泛型参数的具体类型时，可以使用通配符进行对象定义。
实例化泛型对象，我们不能够确定eList存储的数据类型是Integer还是Long，因此我们使用List<? extends Number>定义变量的类型。
```java
List<? extends Number> eList = null;
eList = new ArrayList<Integer>();
eList = new ArrayList<Long>();
```
```java
// 只能放List<Number>
public void showKeyValue1(List<Number> obj){
     Log.d("泛型测试","key value is " + obj.getKey());
}

// 可以放任何List
public void showKeyValue1(List<?> obj){
     Log.d("泛型测试","key value is " + obj.getKey());
}
```

### 上界类型通配符（? extends）
```java
List<? extends Number> eList = null;
eList = new ArrayList<Integer>();
Number numObject = eList.get(0);  //语句1，正确
Integer intObject = eList.get(0);  //语句2，错误
eList.add(new Integer(1));  //语句3，错误
```

### 下界类型通配符（? super ）
```java
List<? super Integer> sList = null;
sList = new ArrayList<Number>();
Number numObj = sList.get(0);  //语句1，错误
Integer intObj = sList.get(0);  //语句2，错误
sList.add(new Integer(1));  //语句3，正确
```
### 上界/下界类型通配符比较
```java
List<? extends Number> eList = null;
eList = new ArrayList<Integer>(); // R
eList = new ArrayList<Long>(); // R

List<? super Number> list = new ArrayList<>();
list.add(new Integer(1));  // R
list.add(new Double(2));  // R
Object obj=list.get(0);   // R 取出数据时无法指定数据类型,否则编译失败
System.out.println(list.get(1)); // R 

List<? extends Number> list2 = new ArrayList<>();
Object number1 = new Integer(1);
list2.add(number1);// W
list2.add(new Integer(1));// W

Number number2 =list2.get(1); // R
System.out.println(list2.get(1)); // R
```


### 通配符总结
- 限定通配符总是包括自己
- 上界类型通配符：add方法受限
- 下界类型通配符：get方法受限
- 如果你想从一个数据类型里获取数据，使用 ? extends 通配符
- 如果你想把对象写入一个数据结构里，使用 ? super 通配符
- 如果你既想存，又想取，那就别用通配符
- 不能同时声明泛型通配符上界和下界

## Class\<T\>类传递及泛型数组

### 使用Class\<T\>传递泛型类Class对象 

```java
// Class<T>相当于 Object.class
public static <T> List<T> parseArray(String response,Class<T> object){  
    List<T> modelList = JSON.parseArray(response, object);  
    return modelList;  
}
```

### 定义泛型数组 

遇到类似String[] list = new String[8];的需求，这里可以定义String数组，当然我们也可以定义泛型数组，泛型数组的定义方法为 T[]，与String[]是一致

```java
// 定义  
public static <T> T[] fun1(T...arg){  // 接收可变参数    
       return arg ;            // 返回泛型数组    
}    
// 使用  
public static void main(String args[]){    
       Integer i[] = fun1(1,2,3,4,5,6) ;  
       Integer[] result = fun1(i) ;  
}    
```

## 泛型实例化

类型可以通过参数来实现，例如泛型中的T t，但是我想生成T对象，那怎么实现呢？按照以往的经验，我们很容易想到这种方式.

- 由于T的具体类型我们无法获得，所以new T()是无法通过编译的

```java
public class Test<T>
{
    T t;
    public void create()
    {
        t = new T();//编译错误：Cannot instantiate the type T
    }
}  
```

- 正确方式泛型实例化

```java
public class Test<T>
{
    T t;
    public void create(Class<T> clazz)
    {
        try
        {
            t = clazz.newInstance();
        }
        catch (InstantiationException e)
        {
            e.printStackTrace();
        }
        catch (IllegalAccessException e)
        {
            e.printStackTrace();
        }
    }
}
```

## 协变，逆变
- f(⋅)是逆变（contravariant）的，当A≤B时有f(B)≤f(A)成立；
- f(⋅)是协变（covariant）的，当A≤B时有成立f(A)≤f(B)成立；

### 数组支持协变
```java
List<A> list = new ArrayList<B>();//编译失败,集合使用泛型避免了协变引入的运行时错误,在编译时就避免了问题
//数组不支持泛型,所以需要协变
Object[] array = new Integer[10];
array[0] = "s String"; //运行时错误
```
### 集合支持协变，逆变
? extends 协变
? super 逆变



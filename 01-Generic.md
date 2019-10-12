# 泛型

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

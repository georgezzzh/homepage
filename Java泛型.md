### Java泛型

* 泛型实现了类型参数化

优点

* 类型安全
* 消除了强制类型转换

#### 定义一个泛型类

用T指定类型

```java
public class Pair <T>{
    private T first;
    private T second;
    public Pair() {}
    public Pair(T first, T second){
        this.first = first;
        this.second = second;
    }
    public T getFirst(){ return this.first; }
    public T getSecond(){ return this.second; }
    // 静态方法中是不能使用类的泛型的，因为只有类经过实例化才能确定泛型的具体类型。而静态方法不需要类的实例化
    /*
    public static void printT(T a){
        System.out.print(a);
    }
    */
    public void setFirst(T newValue){ this.first = newValue; }
    public void setSecond(T newValue) { this.second = newValue; }
}
```

#### 泛型类继承

从泛型类派生子类

* 子类也是泛型类，子类和父类的泛型标识要相同

  ```java
  public class Parent<T> {}
  public class Child <E> extends  Parent<E>{}
  ```

* 子类不是泛型类

  ```java
  //明确父类的类型
  public class Child2 extends Parent<String>{}
  //不指定父类类型，默认是Object类型
  public class Child2 extends Parent{}
  ```

#### 泛型方法

定义一个泛型方法，泛型方法可以出现在普通类中，不一定是泛型类

泛型方法也支持静态方法。

```java
//方法参数类型为E
public static  <E> E getProduct(ArrayList<E> list){
    return list.get(0);
}
//调用
ArrayList<Integer> list = new ArrayList<>();
list.add(1);
Integer product = getProduct(list);
```



### 类型通配符

在访问Product泛型类时，不确定具体类型时，可以用`?` 来代替具体类型。

```java
    public  static void showProduct(Product<?> product){
        Object val = product.getVal();
        System.out.println(val);
    }
```

类型通配符指定**上限**，规定接收的参数必须是Number类或者是Number的子类。

```java
    public  static void showProduct(List<? extends Number> list){
        Number number = list.get(0);
    }
```

类型通配符指定**下限**

接收的参数只能是Number或者Number的父类, 而Object是所有类的父类。

```java
    public static void showProducts(List<? super Number> list){
        Object object = list.get(0);
    }
```



#### 类型擦除

无论何时定义一个泛型类型，都自动提供一个相应的原始类型。原始类型的名字就是删去类型参数后的泛型类型名。擦除类型变量，并替换为限定类型(无限定的变量用Object)。

泛型信息存在于代码编译阶段，在进入JVM之前，与泛型相关信息会被擦除掉，称之为 类型擦除。

* 无限制类型擦除

  ```java
  public class Eraser <T>{
      private T key;
  
      public void setKey(T key) {
          this.key = key;
      }
  
      public T getKey() {
          return key;
      }
  }
  ```

  编译生成的文件，key的类型为Object

* 生成上限类型

  ```java
  public class Eraser <T extends Number>{
      private T key;
  
      public void setKey(T key) {
          this.key = key;
      }
  
      public T getKey() {
          return key;
      }
  }
  ```

  编译生成之后，key的类型为Number类型。

  
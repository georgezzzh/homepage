---
title: Java类实现排序的两种方法
date: 2020-12-10 晚
tags:
- Java
---
### Java类实现排序的两种方法

* 实现接口**Comparable**,重载其中的`compareTo`方法
* 实现接口**Comparator**, 重载其中的`compare`方法
<!-- more -->
### Comparable

实现这个接口, 目的是本类的对象之间可以相互进行比较. 举个例子就是可以调用`Arrays.sort(DemoClass)`.

```java
public class Employee implements Comparable<Employee>{
	private int salary;
	@Override
    public int compareTo(Employee other) {
        return Integer.compare(this.salary, other.salary);
    }
    public static void main(){
        Employee[] employees = new Employee[3];
        employees[0] = new Employee("down",300);
        employees[1] = new Employee("own",500);
        employees[2] = new Employee("n",200);
        Arrays.sort(employees);
        //排序结果是 n down own, 升序
    }
}
```

### Comparator

实现Comparator类, 目的是生成一个比较器工具类, 实现某些类无法重复实现`CompareTo`方法.  例如String类的比较是按照字典顺序排序, 现在我们想让String按照字符串长短进行比较, 就可以实现一个Comparator工具类, 传递到`Arrays.sort()`就可以完成排序.

```java
public class LengthComparator implements Comparator<String>{
    @Override
    public int compare(String o1, String o2) {
        //依据字符串长短进行比较
        return Integer.compare(o1.length(),o2.length());
    }
    public static void run(boolean flag){
        if(!flag)return;
        String[] items = {"Tom","Ti","Johnson","jit"};
        //把比较器同时传入
        Arrays.sort(items,new LengthComparator());
        //顺序: Ti Tom jit Johnson
        System.out.println(Arrays.toString(items));
    }
}
```


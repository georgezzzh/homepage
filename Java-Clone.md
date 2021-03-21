---
title: Java-Clone方法
date: 2020-12-11 晚
tags:
- Java
---
`clone`方法由`Cloneable`接口提供. 这个接口指示一个类提供一个安全的clone方法.

`clone`方法是Object的一个protected方法, 因此在类外无法直接调用这个方法. 只有在Employee类内部可以克隆Employee对象. 
<!-- more -->
Object类默认的clone实现, 是将每个数据域复制一遍, 基本类型, 拷贝是没问题的, 但是如果包含对象的引用, 拷贝域会得到相同子对象的另外一个引用.

自己在类中实现`clone`方法, 需要重载为`public`的访问控制. 如此, 在类外可以直接调用. 经过自定义的`clone`方法, 应该剥离两个对象的联系. 修改其中之一, 不会影响另外一个对象的状态.

```java
public class Employee{
    private String name;
    private int salary;
    private StringBuffer buffer;
    // .... 省略构造函数及其 set get等方法
    @Override
    public Employee clone() throws CloneNotSupportedException {
        /*
        如果该类有任何一个属性,不支持clone操作, 调用super.clone()方法会抛出
         ClassNotSupportedException异常
        Employee cloneObj = (Employee) super.clone();
        */
        Employee cloneObj = new Employee(this.name,this.salary);
        //对引用的变量,进行克隆操作
        StringBuffer br = new StringBuffer(buffer);
        cloneObj.buffer = br;
        return cloneObj;
    }    
}
```

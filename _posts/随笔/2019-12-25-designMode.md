---
layout: blog
essay: true
title: "java-designMode"
background-image: http://ot1cc1u9t.bkt.clouddn.com/17-7-16/38390376.jpg
date:  2019-12-23 13:45:56
category: essay
tags:
- java
- designMode
---

## java-23种设计模式
> 设计模式是一套被反复使用的、多数人知晓的、经过分类编目的、代码设计经验的总结。  

* **设计模式六大原则**  
  1. 开闭原则：对扩展开放，对修改关闭--热插拔  
  2. 里氏代换原则：任何基类可以出现的地方，子类一定可以出现--继承、抽象化  
  3. 依赖倒转原则：针对接口编程，依赖于抽象而不依赖于具体  
  4. 接口隔离原则：降低依赖，降低耦合  
  5. 最少知道原则：一个实体应当尽量少地与其他实体之间发生相互作用  
  6. 合成复用原则：尽量使用合成/聚合的方式，而不是使用继承  

### 创建型模式  

#### 工厂模式  
* **步骤**
  1. 创建一个接口--Shape.java  

  ```java
  public interface Shape {
    void draw();
  }
  ```  

  2. 创建实现接口的实体类--Rectangle.java/Square.java/Circle.java  

  ```java
  public class Rectangle implements Shape {

     @Override
     public void draw() {
        System.out.println("Inside Rectangle::draw() method.");
     }
  }

  public class Square implements Shape {

     @Override
     public void draw() {
        System.out.println("Inside Square::draw() method.");
     }
  }

  public class Circle implements Shape {

     @Override
     public void draw() {
        System.out.println("Inside Circle::draw() method.");
     }
  }
  ```  

  3. 创建一个工厂，生成基于给定信息的实体类的对象--ShapeFactory.java  

  ```java
  public class ShapeFactory {

     //使用 getShape 方法获取形状类型的对象
     public Shape getShape(String shapeType){
        if(shapeType == null){
           return null;
        }        
        if(shapeType.equalsIgnoreCase("CIRCLE")){
           return new Circle();
        } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
           return new Rectangle();
        } else if(shapeType.equalsIgnoreCase("SQUARE")){
           return new Square();
        }
        return null;
     }
  }
  ```  

  4. 使用该工厂，通过传递类型信息来获取实体类的对象--FactoryPatternDemo.java  

  ```java
  public class FactoryPatternDemo {

     public static void main(String[] args) {
        ShapeFactory shapeFactory = new ShapeFactory();

        //获取 Circle 的对象，并调用它的 draw 方法
        Shape shape1 = shapeFactory.getShape("CIRCLE");

        //调用 Circle 的 draw 方法
        shape1.draw();

        //获取 Rectangle 的对象，并调用它的 draw 方法
        Shape shape2 = shapeFactory.getShape("RECTANGLE");

        //调用 Rectangle 的 draw 方法
        shape2.draw();

        //获取 Square 的对象，并调用它的 draw 方法
        Shape shape3 = shapeFactory.getShape("SQUARE");

        //调用 Square 的 draw 方法
        shape3.draw();
     }
  }
  ```

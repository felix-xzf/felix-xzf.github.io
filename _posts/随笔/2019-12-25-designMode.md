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

* 核心
  1. 定义一个创建对象的接口，让其子类决定实例化哪一个工厂类，创建过程在子类执行
  2. 解决接口选择的问题，不同条件下创建不同实例  

![实现](https://raw.githubusercontent.com/felix-xzf/felix-xzf.github.io/master/style/images/factoryimpl.png)

* **步骤**
    1.创建一个接口--Shape.java  

  ```java
  public interface Shape {
    void draw();
  }
  ```  

    2.创建实现接口的实体类--Rectangle.java/Square.java/Circle.java  

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

#### 抽象工厂模式
* 核心
  1. 创建一系列相互或相互依赖的接口，而无需指定它们具体的类
  2. 解决接口选择的问题，系统有多于一个产品族，一个产品族里定义多个产品

![实现](https://raw.githubusercontent.com/felix-xzf/felix-xzf.github.io/master/style/images/absfactoryimpl.png)

* 步骤
  1. 为形状创建一个接口--Shape  
```java
public interface Shape {
   void draw();
}
```
  2. 创建实现接口的实体类--Rectangle/Square/Circle  

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

  3. 为颜色创建一个接口--Color  

```java
public interface Color {
   void fill();
}
```
  4. 创建实现接口的实体类--Red/Green/Blue

```java
public class Red implements Color {

   @Override
   public void fill() {
      System.out.println("Inside Red::fill() method.");
   }
}

public class Green implements Color {

   @Override
   public void fill() {
      System.out.println("Inside Green::fill() method.");
   }
}

public class Blue implements Color {

   @Override
   public void fill() {
      System.out.println("Inside Blue::fill() method.");
   }
}
```
  5. 为 Color 和 Shape 对象创建抽象类来获取工厂--AbstractFactory
```java
public abstract class AbstractFactory {
   public abstract Color getColor(String color);
   public abstract Shape getShape(String shape) ;
}
```
  6. 创建扩展了 AbstractFactory 的工厂类，基于给定的信息生成实体类的对象--ShapeFactory/ColorFactory
```java
public class ShapeFactory extends AbstractFactory {

   @Override
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

   @Override
   public Color getColor(String color) {
      return null;
   }
}

public class ColorFactory extends AbstractFactory {

   @Override
   public Shape getShape(String shapeType){
      return null;
   }

   @Override
   public Color getColor(String color) {
      if(color == null){
         return null;
      }        
      if(color.equalsIgnoreCase("RED")){
         return new Red();
      } else if(color.equalsIgnoreCase("GREEN")){
         return new Green();
      } else if(color.equalsIgnoreCase("BLUE")){
         return new Blue();
      }
      return null;
   }
}
```
  7. 创建一个工厂创造器/生成器类，通过传递形状或颜色信息来获取工厂--FactoryProducer
```java
public class FactoryProducer {
   public static AbstractFactory getFactory(String choice){
      if(choice.equalsIgnoreCase("SHAPE")){
         return new ShapeFactory();
      } else if(choice.equalsIgnoreCase("COLOR")){
         return new ColorFactory();
      }
      return null;
   }
}
```
  8. 使用 FactoryProducer 来获取 AbstractFactory，通过传递类型信息来获取实体类的对象
```java
public class AbstractFactoryPatternDemo {
   public static void main(String[] args) {

      //获取形状工厂
      AbstractFactory shapeFactory = FactoryProducer.getFactory("SHAPE");

      //获取形状为 Circle 的对象
      Shape shape1 = shapeFactory.getShape("CIRCLE");

      //调用 Circle 的 draw 方法
      shape1.draw();

      //获取形状为 Rectangle 的对象
      Shape shape2 = shapeFactory.getShape("RECTANGLE");

      //调用 Rectangle 的 draw 方法
      shape2.draw();

      //获取形状为 Square 的对象
      Shape shape3 = shapeFactory.getShape("SQUARE");

      //调用 Square 的 draw 方法
      shape3.draw();

      //获取颜色工厂
      AbstractFactory colorFactory = FactoryProducer.getFactory("COLOR");

      //获取颜色为 Red 的对象
      Color color1 = colorFactory.getColor("RED");

      //调用 Red 的 fill 方法
      color1.fill();

      //获取颜色为 Green 的对象
      Color color2 = colorFactory.getColor("Green");

      //调用 Green 的 fill 方法
      color2.fill();

      //获取颜色为 Blue 的对象
      Color color3 = colorFactory.getColor("BLUE");

      //调用 Blue 的 fill 方法
      color3.fill();
   }
}
```

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

  3.创建一个工厂，生成基于给定信息的实体类的对象--ShapeFactory.java  

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

  4.使用该工厂，通过传递类型信息来获取实体类的对象--FactoryPatternDemo.java  

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

  1.为形状创建一个接口--Shape  

  ```java
  public interface Shape {
     void draw();
  }
  ```  

  2.创建实现接口的实体类--Rectangle/Square/Circle  

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

  3.为颜色创建一个接口--Color  

  ```java
  public interface Color {
     void fill();
  }
  ```
  4.创建实现接口的实体类--Red/Green/Blue

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

  5.为 Color 和 Shape 对象创建抽象类来获取工厂--AbstractFactory
  ```java  

  public abstract class AbstractFactory {
     public abstract Color getColor(String color);
     public abstract Shape getShape(String shape) ;
  }
  ```  

  6.创建扩展了 AbstractFactory 的工厂类，基于给定的信息生成实体类的对象--ShapeFactory/ColorFactory  

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

  7.创建一个工厂创造器/生成器类，通过传递形状或颜色信息来获取工厂--FactoryProducer  

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

  8.使用 FactoryProducer 来获取 AbstractFactory，通过传递类型信息来获取实体类的对象  

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
#### 单例模式  
* 核心  
  1. 单例类只能有一个实例  
  2. 单例类必须创建自己的唯一实例  
  3. 单例类必须给所有其他对象提供这一实例  
![实现](https://raw.githubusercontent.com/felix-xzf/felix-xzf.github.io/master/style/images/singleton.png)  
* 步骤  
  1.创建一个Singleton类  
  ```java
  public class SingleObject {

     //创建 SingleObject 的一个对象
     private static SingleObject instance = new SingleObject();

     //让构造函数为 private，这样该类就不会被实例化
     private SingleObject(){}

     //获取唯一可用的对象
     public static SingleObject getInstance(){
        return instance;
     }

     public void showMessage(){
        System.out.println("Hello World!");
     }
  }
  ```  
  2.从Singleton获取唯一的对象  
  ```java
  public class SingletonPatternDemo {
     public static void main(String[] args) {

        //不合法的构造函数
        //编译时错误：构造函数 SingleObject() 是不可见的
        //SingleObject object = new SingleObject();

        //获取唯一可用的对象
        SingleObject object = SingleObject.getInstance();

        //显示消息
        object.showMessage();
     }
  }
  ```  
#### 建造者模式  
* 核心  
  1. 使用多个简单的对象构建成一个复杂的对象  
  2. 将变与不变分离开  
![实现](https://raw.githubusercontent.com/felix-xzf/felix-xzf.github.io/master/style/images/builder.png)  
* 步骤  
  1.创建一个表示食物条目和食物包装的接口--Item/Packing  
  ```java  
  public interface Item {
     public String name();
     public Packing packing();
     public float price();    
  }
  public interface Packing {
     public String pack();
  }
  ```  
  2.创建实现 Packing 接口的实体类--Wrapper/Bottle  
  ```java
  public class Wrapper implements Packing {

     @Override
     public String pack() {
        return "Wrapper";
     }
  }
  public class Bottle implements Packing {

     @Override
     public String pack() {
        return "Bottle";
     }
  }
  ```  
  3.创建实现 Item 接口的抽象类，该类提供了默认的功能--Burger/ColeDrink  
  ```java
  public abstract class Burger implements Item {

     @Override
     public Packing packing() {
        return new Wrapper();
     }

     @Override
     public abstract float price();
  }
  public abstract class ColdDrink implements Item {

      @Override
      public Packing packing() {
         return new Bottle();
      }

      @Override
      public abstract float price();
  }
  ```  
  4.创建扩展了 Burger 和 ColdDrink 的实体类--VegBurger/ChickenBurger/Coke/Pepsi  
  ```java  
  public class VegBurger extends Burger {

     @Override
     public float price() {
        return 25.0f;
     }

     @Override
     public String name() {
        return "Veg Burger";
     }
  }
  public class ChickenBurger extends Burger {

     @Override
     public float price() {
        return 50.5f;
     }

     @Override
     public String name() {
        return "Chicken Burger";
     }
  }
  public class Coke extends ColdDrink {

     @Override
     public float price() {
        return 30.0f;
     }

     @Override
     public String name() {
        return "Coke";
     }
  }
  public class Pepsi extends ColdDrink {

     @Override
     public float price() {
        return 35.0f;
     }

     @Override
     public String name() {
        return "Pepsi";
     }
  }
  ```  
  5.创建一个 Meal 类，带有上面定义的 Item 对象  
  ```java
  import java.util.ArrayList;
  import java.util.List;

  public class Meal {
     private List<Item> items = new ArrayList<Item>();    

     public void addItem(Item item){
        items.add(item);
     }

     public float getCost(){
        float cost = 0.0f;
        for (Item item : items) {
           cost += item.price();
        }        
        return cost;
     }

     public void showItems(){
        for (Item item : items) {
           System.out.print("Item : "+item.name());
           System.out.print(", Packing : "+item.packing().pack());
           System.out.println(", Price : "+item.price());
        }        
     }    
  }
  ```  
  6.创建一个 MealBuilder 类，实际的 builder 类负责创建 Meal 对象  
  ```java  
  public class MealBuilder {

     public Meal prepareVegMeal (){
        Meal meal = new Meal();
        meal.addItem(new VegBurger());
        meal.addItem(new Coke());
        return meal;
     }   

     public Meal prepareNonVegMeal (){
        Meal meal = new Meal();
        meal.addItem(new ChickenBurger());
        meal.addItem(new Pepsi());
        return meal;
     }
  }
  ```  
  7.BuiderPatternDemo 使用 MealBuider 来演示建造者模式（Builder Pattern）  
  ```java  
  public class BuilderPatternDemo {
     public static void main(String[] args) {
        MealBuilder mealBuilder = new MealBuilder();

        Meal vegMeal = mealBuilder.prepareVegMeal();
        System.out.println("Veg Meal");
        vegMeal.showItems();
        System.out.println("Total Cost: " +vegMeal.getCost());

        Meal nonVegMeal = mealBuilder.prepareNonVegMeal();
        System.out.println("\n\nNon-Veg Meal");
        nonVegMeal.showItems();
        System.out.println("Total Cost: " +nonVegMeal.getCost());
     }
  }
  ```  
#### 原型模式  
* 核心  
  1. 利用已有的一个原型对象，快速地生成和原型对象一样的实例  
![实现](https://raw.githubusercontent.com/felix-xzf/felix-xzf.github.io/master/style/images/prototype.png)  
* 步骤  
  1.创建一个实现了 Cloneable 接口的抽象类  
  ```java  
  public abstract class Shape implements Cloneable {

     private String id;
     protected String type;

     abstract void draw();

     public String getType(){
        return type;
     }

     public String getId() {
        return id;
     }

     public void setId(String id) {
        this.id = id;
     }

     public Object clone() {
        Object clone = null;
        try {
           clone = super.clone();
        } catch (CloneNotSupportedException e) {
           e.printStackTrace();
        }
        return clone;
     }
  }
  ```  
  2.创建扩展了上面抽象类的实体类  
  ```java  
  public class Rectangle extends Shape {

     public Rectangle(){
       type = "Rectangle";
     }

     @Override
     public void draw() {
        System.out.println("Inside Rectangle::draw() method.");
     }
  }
  public class Square extends Shape {

     public Square(){
       type = "Square";
     }

     @Override
     public void draw() {
        System.out.println("Inside Square::draw() method.");
     }
  }
  public class Circle extends Shape {

     public Circle(){
       type = "Circle";
     }

     @Override
     public void draw() {
        System.out.println("Inside Circle::draw() method.");
     }
  }
  ```  
  3.创建一个类，从数据库获取实体类，并把它们存储在一个 Hashtable 中  
  ```java  
  import java.util.Hashtable;

  public class ShapeCache {

     private static Hashtable<String, Shape> shapeMap
        = new Hashtable<String, Shape>();

     public static Shape getShape(String shapeId) {
        Shape cachedShape = shapeMap.get(shapeId);
        return (Shape) cachedShape.clone();
     }

     // 对每种形状都运行数据库查询，并创建该形状
     // shapeMap.put(shapeKey, shape);
     // 例如，我们要添加三种形状
     public static void loadCache() {
        Circle circle = new Circle();
        circle.setId("1");
        shapeMap.put(circle.getId(),circle);

        Square square = new Square();
        square.setId("2");
        shapeMap.put(square.getId(),square);

        Rectangle rectangle = new Rectangle();
        rectangle.setId("3");
        shapeMap.put(rectangle.getId(),rectangle);
     }
  }
  ```  
  4.PrototypePatternDemo 使用 ShapeCache 类来获取存储在 Hashtable 中的形状的克隆  
  ```java  
  public class PrototypePatternDemo {
     public static void main(String[] args) {
        ShapeCache.loadCache();

        Shape clonedShape = (Shape) ShapeCache.getShape("1");
        System.out.println("Shape : " + clonedShape.getType());        

        Shape clonedShape2 = (Shape) ShapeCache.getShape("2");
        System.out.println("Shape : " + clonedShape2.getType());        

        Shape clonedShape3 = (Shape) ShapeCache.getShape("3");
        System.out.println("Shape : " + clonedShape3.getType());        
     }
  }
  ```  

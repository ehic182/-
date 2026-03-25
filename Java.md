# Java

## (一)面向对象

如何建一个对象,

先建一个类：表示某一类事物不是具体的一个事物

~~~java
public class dog{
    String name;
    int age;
    double weight;
    //事物的特征
    public  void eat(){
        System.out.println("狗狗在吃饭")
    }//这里是设定了一个方法，定义了狗能干什么
}
~~~

再建一个用来测试的带有main方法的类

~~~java
public class test{
    public static void main(String[] args){
        dog a1=new dog();//设定一个对象
   		a1.name="大黄";//给特定的对象赋值
		a1.age=1;
		a1.weight=13.1;
    }
}
~~~

## (二)面向对象中的安全问题

如何实现：把类中的所有属性私有化，用private（几个private几个get和set）

若构造方法私有化之后，其子类便不能再访问这个类，即无法从这个类里继承

目的：防止输入错误数据

~~~java
public class dog{
   private String name;
   private int age;
   private double weight;//事物的特征
    public void setname(String value){
        name=value;
    }//专门构造一个方法让计算机知道该怎么赋值，设了一个形参
    
    public String getname(){
        return name;
    }//同上专门构造一个方法来记录
    
    public void setage(int num){
        if(num>0&&num<15){
        name=num;
        }
        else{System.out.println"输入数不在合理范围内"}
    }
    
    public int getage(){
        return age;
    }
    public  void eat(){
        System.out.println("狗狗在吃饭")
    }//这里是设定了一个方法，定义了狗能干什么
}
~~~

测试

~~~java
public class{
     public static void main(String[] args){
         dog.setname("大黄");
         dog.setage(12);
         dog.eat;
}
}
~~~

## (三)this关键字和就近原则

在Java中变量的命名必须有自身的含义（让人能看懂是啥意思）

本质：代表所在方法调用者的内存地址

~~~java
public class dog{
   private String name;
   private int age;//成员变量
   private double weight;//事物的特征
    public void setname(String value){
        name=value;
    }
    
    public String getname(){
        return name;
    }
    
    public void setage(int age){
        if(num>0&&num<15){
        this age=age;//this关键字会调用成员变量，不会调用局部同名变量
            //后面的age遵循就近原则先找到最近的变量age
        }
        else{System.out.println"输入数不在合理范围内"}
    }
    
    public String getage(){
        return age;
    }
~~~

## (四)构造方法(初始化)

~~~java
public class student{
    private String name;
    private int age;
    private int height;
    
    public student(){
        
    }//空参构造方法
    
    public student(String name,int age,int height){
        this.name=name;
        this.age=age;
        this.height =height;
    }//全参构造法
    
}
~~~

调用

~~~java
public class test{
    public static void main(String[] args){
        student a1=new student(name:"张三"，age:19,height:56);
  }
~~~

## (五)面向对象原理

1：把类放入方法区存储（若先前已经加载过则该类已经被记录无需再加载）

2：在栈里运行main方法(临时运行区域)

3：在栈里加载等号左边的局部变量名，用来存储变量地址

4：在堆里申请一段内存和该内存地址，用来给等号右边的方法

~~~ java
 public class Memory{
     public static void main (String[] arges) {
         Student stu = new Student();
     }
 } 
~~~

## (六)static关键字

~~~java
public class StaticStudent {
    String name;
    int age;
    static String teacherName;//设置静态变量，让所有对象共用
}//先建类
~~~

~~~java
package Static;

public class TestStatic {
    public static void main(String[] args) {
        StaticStudent s1 = new StaticStudent();
        StaticStudent s2 = new StaticStudent();
        s1.name = "张三";
        s1.age = 18;
        s2.name = "李四";
        s2.age = 20;
        StaticStudent.teacherName = "王老师";
        System.out.println(s1.name + " " + s1.age + " " + StaticStudent.teacherName);
        System.out.println(s2.name + " " + s2.age + " " + StaticStudent.teacherName);
    }
}//测试运行
~~~

## (七)static方法（工具类方法）

~~~java
public class ArrayUitl {
    private ArrayUitl(){}//私有化，防止创建对象
    public static void printArray(int[] array){
        for(int i = 0; i < array.length; i++){
            System.out.print(array[i] + " ");
        }//此处必须为静态方法，否则在main方法里无法调用
        System.out.println();
    }
}
~~~

~~~java
public class Test {
    public static void main(String[] args) {
        int[] a = {1, 2, 3, 4, 5};
        ArrayUitl.printArray(a);
    }
}

~~~

注:静态只能调用静态，非静态可以调用所有，静态无this

## (八)final关键字

特点一：final 修饰常量，被final修饰后值就不能再发生改变

特点二：常量名要用大写来区分，且两个连续的单词之间要用下划线来连接

~~~java
package finaly;

public class Circle2 {
    private double radius;
    private final double PI = 3.14;//设置常量
    
    public Circle2() {}
    
    public Circle2(int radius) {
        this radius = radius;
    }
    public void setRadius(double radius) {
        this.radius = radius;
    }
    public double getRadius() {
        return radius;
    }

    public double getPI() {
        return PI;
    }//无setPI 是因为PI为常量不能修改

    public double getArea() {
        return PI*radius*radius;
    }
    public double getCircumference() {
        return 2*PI*radius;
    }
}

~~~

## (九)枚举

类

~~~ java
public enum OrderState {//class类改成enum类，即枚举类
    PAYMENT_PENDING("待支付"),
    PROCESSING("处理中"),
    SHIPPED("配送中"),
    DELIVERED("已送达"),
    CANCELLED("已取消");

    private String state;

    private OrderState(String state) {
        this.state=state;
    }//要私有化状态名，防止创建对象

    public String getState() {
        return state;
    }//无需setState，因为已经给出特定的状态
}

~~~

测试

~~~java
public class Test {
    public static void main(String[] args) {
        OrderState s1=OrderState.CANCELLED;//此处相当于创建对象
        System.out.println(s1.getState());
        switch (s1){
            case PAYMENT_PENDING->System.out.println("待支付");
            case PROCESSING->System.out.println("处理中");
            case SHIPPED->System.out.println("配送中");
            case DELIVERED->System.out.println("已送达");
            default->System.out.println("已取消");
        }
    }
}
~~~

每一个枚举项，都是该枚举类的对象，每个对象都是通过构造方法创建出来的

枚举项在底层其实就是常量，默认用public static final 来修饰

枚举类的第一行必须是所有枚举项

枚举类的构造方法必须用private来修饰，防止外部创建对象

## (十)继承

何时使用继承：当类与类之间存在共性时，且满足子类是父类的一种时

父类

~~~java
public class Father {
     String name;
     int age;
    double weight;
    String work;

    public void hobby() {
        System.out.println("运动");
    }
}

~~~

子类

~~~java
public class Son extends Father {//后面的extends Father就表示继承了父亲father的特性
    String interest;
    public void state() {
        System.out.println("上学");
    }
}

~~~

测试

~~~java
public class TryExtends {
    public static void main(String[] args) {
        Son a1=new Son();
        a1.state();
        a1.age=11;
        a1.interist="play";
    }
}

~~~

![QQ20260317-090757](E:\照片\QQ20260317-090757.png)

图中带f标签的就是从直接父类继承过来的属性，其他都是从间接父类来的

若无继承则虚拟机会自动继承object父类

继承中this在本类中开始往上找，super在父类中开始往上找



### 1.1成员方法的特点

类一

~~~java
public class FristGenerationPhone {
    public void ability() {
        System.out.println("打电话");
    }
}

~~~

类二

~~~java
public class SecondGenerationPhone extends FirstGenerationPhone{
    @Override
    public void ability() {
        System.out.println("视频通话");
    }
}//当父类的功能不能再满足子类的要求时可以在子类重写该方法 
~~~



~~~ java
public class Test {
    public static void main (String[] args){
        Phone a1 = new Phone();
        a1.name = "vivo";
        a1.price = 4999 ;
        double a2=a1.getSmartPhone();
        System.out.println(a2);
    }
 }

public class SmartFacility {
    String name;
    double price;

    public double getDiscount() {
        if (price>0&&price<1000) {
            return price;
        } else if (price>1000&&price<5000) {
            return price*0.9;
        } else if (price>5000&&price<10000) {
            return price*0.8;
        } else if (price>10000) {
            return price*0.7;
        }
        else {return 0;}
    }
}

public class Phone extends SmartFacility {
   public double getSmartPhone() {
       return super.getDiscount()*0.9;//super优先调用父类
   }
}//直接调用父类里的方法

public class PaiCmputer extends SmartFacility{}

~~~

注意：重写方法的名称，形参列表必须与父类一致

  	 final为最终类里面的所有方法都不能被重写

​	   private私有化，static静态方法，final最终方案不能被改写

### 1.2继承里的构造方法

~~~java
public class Test {
    public static void main(String [] args){
        Student a1 =new  Student(13,3,"张三");
        System.out.println(a1.age+a1.garden+a1.name);
    }
}

public class Person {//在teacher类和student类里找到共同点组成的新类，减少代码量
    int age;
    String name;

    public Person(){};

    public  Person(int age,String name) {
        this.age = age;
        this.name = name;
    }
}


public class teacher extends Person {//teacher作为子类继承父类变量
    String subject;

    public teacher(){}

    public teacher(int age,String subject,String name){
        super(age, name);//用super调用父类里的构造方法
        this.subject=subject;
    }
}

public class Student extends Person {
     int garden;

    public Student(){};

    public Student(int age,int garden,String name) {
        super(age, name);
        this.garden = garden;
    }
}
~~~

注：子类不能继承父类的构造方法，但可以用super调用

### 1.3继承里的this和super

![Screenshot_20260320_102210](D:\qq\存储/Screenshot_20260320_102210.jpg)

注意：如果子类中有多个构造方法时，不能用this()互相调用

​	    如果构造方法中写了this()就不能写super()



## 练习

~~~ java
Test类
public class test {
    public static void main(String[] args) {
UnderGraduateStudent stu1 = new UnderGraduateStudent("张三",18,"本科");//在括号里按ctrl+p查看形参
        stu1.study();
        stu1.eatFood();
    }
}

UnderGraduateStudent类
public class UnderGraduateStudent extends  Student{
    public UnderGraduateStudent () {}

    public UnderGraduateStudent(String name, int age, String educationBackGround) {
        super(name, age, educationBackGround);
    }//在保证numlock关上的情况下，按下alt+insert,选择constructor即可快捷构造方法

    @Override
    public void study() {
        System.out.println("攻读学士学位");
    }
}

GraduateStudent类
public class GraduateStudent extends Student{
    public GraduateStudent() {
    }

    public GraduateStudent(String name, int age, String educationBackGround) {
        super(name, age, educationBackGround);
    }
    @Override
    public void study() {
        System.out.println("攻读硕士学位");
    }
    @Override
    public void sleep() {
        System.out.print("在豪华宿舍睡觉");
    }
}

GeneralEducationTeacher类
public class GeneralEducationTeacher extends  Teacher{
    public GeneralEducationTeacher() {}

    public GeneralEducationTeacher(String name, int age, String subject) {
        super(name, age, subject);
    }//是个类都要有构造方法，包括工具类，但工具类要私有化构造方法

    @Override
    public void teach(String subject) {
        System.out.println("正在上的课是"+ subject);
    }
}
    
MajorCourseTeacher类
public class MajorCourseTeacher extends  Teacher{
    public MajorCourseTeacher() {
    }

    public MajorCourseTeacher(String name, int age, String subject) {
        super(name, age, subject);
    }

    @Override
    public void teach(String subject) {
        System.out.println("正在教学"+subject);
    }
}

Student类
public class Student extends Person {
     int garden;

    public Student(){};

    public Student(int age,int garden,String name) {
        super(age, name);
        this.garden = garden;
    }
}

Person类
public class Person {
    private String name;
    private int age;

    public Person() {}

    public Person(String name,int age) {
        this.age=age;
        this.name=name;
    }
   public void eatFood() {
       System.out.println("正在吃饭");
   }

   public void sleep() {
       System.out.println("正在睡觉");
   }

    public int getAge() {
        return age;
    }

    public String getName() {
        return name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }
}
~~~

1.所有类都要有明确的构造方法，全参构造和空参构造，包括特殊的类。如工具类(不过工具类一般为静态方法，为了防止构造对 象，所以要私有化构造方法，同时用final来阻止继承)

2.继承可以多层继承但不能多个继承，就像你只能有一个爸爸还有一个爷爷

3.构造方法必须没有类型，并且构造方法的类型名必须与类名相同，但方法必须有类型

4.子类的构造方法必须用super调用父类构造方法，且super必须是第一行

5.在保证numlock关上的情况下，按下alt+insert,选择constructor即可快捷构造方法

6.括号里按ctrl+p查看形参

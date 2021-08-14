## 单例模式Singleton

### 1.含义
单例模式：即一个类只能创建一个实例。       
- 只有一个实例 --> 不可以从类外new对象 --> 构造器私有化private --> 从类里创建实例；     
- 这个对象供外部使用 --> 该实例的get方法 --> 没有对象，随着类加载 --> 方法声明为static --> 静态只能调静态 --> 该实例也声明为static；

### 2.实现
#### 2.1 饿汉式
```
public class Singleton{
    
    //1.私有化构造器；
    private Singleton();
    //2.创建实例；静态；
    private static Singleton singleton = new Singleton();
    //3.获取实例的公共方法；静态；
    public static Singleton getSinggleton(){
        return singleton;
    }
}
```
**特点**       
多线程安全：是；      
lazy初始化：否；       
类加载就创建实例，浪费内存；
#### 2.2 懒汉式，线程不安全
```
public class Singgleton{
    
    private Singleton();
    private static Singleton singleton; //延迟创建对象；
    public static Singleton getSingleton(){
        if(singleton == null){ //调用方法的时候没有了再创建；
            singleton = new Singleton(); 
        }
        return singleton;
    }
}
```
**特点**  
多线程安全：否 (当有多个线程操作时，可能会有多个线程都经过if判断语句，就会创建多个实例，造成线程不安全)；    
lazy初始化：是；
#### 2.3 懒汉式，线程安全
```
public class Singleton{
    
    private Singleton();
    private static Singleton singleton;
    //给方法加锁；
    public static synchronized Singleton getSingleton(){
        if(singleton == null){
            singleton = new Singleton();
        }
        return singleton;
    }
}
```
**特点**  
多线程安全：是       
lazy初始化：是      
效率：低(每次都只是一个线程拿到锁，调用方法，多个线程无法同时调用方法)
#### 2.4 双重检验锁
```
public class Singleton{
    
    private Singleton();
    //使用volatile声明：
    //1.禁止指令重排；
    //2.保证变量修改后对所有线程可见，指示JVM，这个变量是共享且不稳定的，每次用要到主存中去读取；
    private volatile static Singleton singleton;
    public static Singleton getSingleton(){
        if(singleton == null){  //判断有没有实例化过，没有再进入加锁模式，这样绝大多数的都不会再进入了；
            synchronized(Singleton.class){
                if(singleton == null){ //再判断一次，因为可能有多个线程都经过了前面的if，if不判断就会创建多个对象了；
                    singleton == new Singleton();
                }
            }
        }
        return singleton;
    }
}
```
多线程安全：是        
lazy初始化：是      
效率：高       
- 第一个检验是为了效率；
- 第二个检验为了线程安全；

直接加锁再判断 --> 效率低；      
先判断再加锁   --> 效率高；

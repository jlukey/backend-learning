## 1. IOC原理
IOC（Inversion of Control）:控制反转，是Spring Core最核心部分。

在了解IOC之前，需要先了解依赖注入DI。



#### 什么是依赖注入 DI
<font style="color:#F5222D;background-color:#FADB14;">依赖注入DI</font>（Dependency Inversion）：**<font style="color:#F5222D;background-color:#FADB14;">把底层类作为参数传递给上层类，实现上层对下层的“控制”</font>**。



> <font style="color:#BFBFBF;">IOC的另外一种实现方式：依赖查找DL（Dependency Lookup），Apache Avalon和EJB就是利用DL实现的IOC。DL相对DI而言是更为主动的方法，在需要的时候通过调用框架提供的方法来获取对象。获取时需要通过提供相关的配置文件路径、key等信息来确定获取对象的状态。DL已经被抛弃，因为它需要用户自己去使用API查找资源和组装对象，有侵入性。DI是当今IOC的主流实现方式。</font>
>

****

**举个例子理解依赖注入DI思想：**

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1617961271662-1843d3ab-5db8-4ad0-add4-712f6a434409.png)

设计行李箱用代码实现如下：

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1617961399342-d0ef8ebe-c069-433d-a029-da23b3e9c884.png)

如果需要修改底层轮子的尺寸，就需要修改所有部位的代码：

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1617961799134-c80675b0-8ba9-4c96-9055-f23913017327.png)



因此，我们换一种思路，根据行李箱大小设计箱体，根据箱体设计底盘，根据底盘设计轮子：

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1617961587499-493556d1-6f98-4038-bf91-fd42e98d99bc.png)

用代码实现如下：

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1617961678021-f20d16b1-9385-4321-8c80-7c21c369cdfa.png)

当需要修改轮子尺寸的时候，如下，只需修改Tire，其他部位无需调整：

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1617961717021-e79a9e02-2c45-47c4-8f48-b3fdb75a0292.png)



#### 依赖注入的方式
+ Set注入：实现特定属性的public setter（）方法来让IOC容器调用注入所依赖类型的对象。
+ 接口注入：实现特定的接口来让IOC容器注入所依赖类型的对象。
+ 构造函数注入：实现特定参数的构造函数，在创建对象时让IOC注入所依赖类型的对象。
+ 注解注入：通过Java注解机制，让IOC容器注入所依赖类型的对象。例如Spring框架里的Autowire标签都是能够实现注解功能的。



#### IOC和DI的关系
![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1617962067118-a16dd54c-0ab9-4510-a06d-dbddc98f7bc4.png)

![](https://cdn.nlark.com/yuque/0/2021/webp/12493416/1617963225194-1c59b10a-6920-4919-ac83-666681f03687.webp)



#### 控制反转容器(IOC Container)
其实上面的例子中，对行李箱的类进行初始化的那段代码发生的地方，就是控制反转容器。

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1617963695973-9e2ea979-0b80-4746-8c0b-3e878aa7dfb3.png)

因为采用了依赖注入，在初始化的过程中就不可避免的会写大量的new。这里IOC容器就解决了这个问题。这个容器可以自动对你的代码进行初始化，你只需要维护一个Configuration（可以是xml可以是一段代码），而不用每次初始化一个行李箱都要亲手去写那一大段初始化的代码。这是引入IOC Container的第一个好处。IOC Container的第二个好处是：我们在创建实例的时候不需要了解其中的细节。在上面的例子中，我们自己手动创建一个行李箱实例的时候，是从底层往上层new的

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1617963905563-c4e72e36-4b6a-4e38-a11c-312b67263ff8.png)

这个过程中，我们需要了解整个Luggage/Framework/Bottom/Tire类构造函数是怎么定义的，才能一步一步new注入。而IOC Container在进行这个工作的时候是反过来的，它先从最上层开始往下找依赖关系，到达最底层之后再往上一步一步new（有点像深度优先遍历）：

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1617963948433-d583a078-ba9e-4db2-99ff-d6efba77fa99.png)

而IOC容器直接隐藏了具体的创建实例的细节，例如蓝色部分被容器隐藏，就像一个工厂，我们不用管这个实例是怎么一步一步创建出来的。

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1617963978664-8074c840-e596-4ceb-9147-b869af8f4371.png)



## 2. IOC
![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618206324263-39ccfcde-9c66-4f5b-9b02-08b7d4c1badd.png)

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618206335465-6b63f2f7-db16-45f3-bcff-5e3a00ff3bf2.png)

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618206349076-90a397ab-5caf-4181-b7ab-9b05a2b0e60c.png)![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618206368604-eebd8ce2-dabf-4a51-87ad-5ed0c0fd85e8.png)

**<font style="color:#F5222D;background-color:#FCFCCA;"></font>**

**<font style="color:#F5222D;background-color:#FCFCCA;">SpringIOC 就干两件事情，一个是实例化所有的bean，另一个是把bean与bean的依赖关系注入，底层的技术其实就是反射技术。自己对类去创建对象，最大的作用就是对代码进行解耦！</font>**



**举例**:  在controller中注入某一个TestService service，如果对他的实现类进行修改（比如使用其他实现类），只需要修改实现类的注解。spring会自动实例化新的实现类。

#### Spring IOC / DI底层实现原理
1. 使用工厂设计模式 + 配置文件 + 反射

> <font style="color:#FA8C16;">使用工厂设计模式，在工厂中，根据配置文件中标签的class属性，反射创建出对象。</font>
>
> <font style="color:#FA8C16;">再根据标签的子标签标签的name属性，反射找到对应的set方法，反射调用set方法将标签的value属性的值 赋给属性。</font>
>



## 3. AOP
![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618208915263-bc3dfd08-18de-4ad2-9789-87e172adb770.png)![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618208946992-33a1dafa-85a9-4ff0-9ef1-c8750b4ca8d1.png)

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618208970542-b1290838-d51a-4e9b-9581-92c5d891d7a6.png)

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618208988369-cbf63167-15ad-40eb-87ce-52f026a02e2d.png)





**举例**：系统程序中，有几十个service类都需要实现事务，这个时候aop就出马了。

定义一个代理类，注入具体的实现类然后在代理类中实现事务流程。在controller中注入了代理类进行调用。



**<font style="color:#121212;">代理模式</font>**<font style="color:#121212;">：</font>

> **<font style="color:#595959;">当前对象不愿意干的，没法干的东西委托给别的对象来做</font>**<font style="color:#595959;">，我只要做好本分的东西就好了！</font>
>

****

**Spring AOP and AspectJ AOP 有什么区别？AOP 有哪些实现方式？**

> AOP实现的关键在于 代理模式，AOP代理主要分为静态代理和动态代理。**静态代理的代表为AspectJ**；**动态代理则以Spring AOP为代表**。
>
> （1）AspectJ是静态代理的增强，所谓静态代理，就是AOP框架会在编译阶段生成AOP代理类，因此也称为编译时增强，他会在编译阶段将AspectJ(切面)织入到Java字节码中，运行的时候就是增强之后的AOP对象。
>
> （2）<font style="color:#F5222D;">Spring AOP使用的动态代理，所谓的动态代理就是说</font>**<font style="color:#F5222D;">AOP框架不会去修改字节码，而是每次运行时在内存中临时为方法生成一个AOP对象</font>**<font style="color:#F5222D;">，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。</font>
>

****

**动态代理**：

> <font style="color:#FA541C;">其实就是动态的创建一个代理类出来，创建这个代理类的实例对象，然后引用你真正自己写的类，所有方法的调用，都是先走代理类的对象，他负责做一些代码上的增强，再去调用你写的类。</font>
>

动态代理有几种技术：**cglib动态代理**，**jdk动态代理**

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618210868985-5e43339d-0f76-48f5-913b-96f25e2b13eb.png)

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618210891347-0d634449-7a7a-489b-b8d7-68a1ec5b9f9f.png)

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618210944240-131e0621-e216-405e-b487-c1499578beed.png)

> <font style="color:#FA541C;">在java中，动态代理类包含InvocationHandler(增强器)和Proxy(调度器)</font>
>
> 
>
> <font style="color:#FA541C;">在java中，就是通过实现InvocationHandler，重写覆盖invoke方法进行增强业务，该方法中的method.invoke(真实对象，方法参数)就是真实对象的方法调用本身，</font>
>
> <font style="color:#FA541C;">而代理对象的生成是由Proxy.newProxyInstance()方法</font>
>



**扩展知识**：（cglib动态代理和JDK动态代理的区别）

> JDK动态代理是利用反射机制生成一个实现代理接口的匿名类，在调用具体方法前调用InvokeHandler来处理。
>
> 而cglib动态代理是利用asm开源包，对代理对象类的class文件加载进来，通过修改其字节码生成子类来处理。
>
> 1、如果目标对象实现了接口，默认情况下会采用JDK的动态代理实现AOP 
>
> 2、如果目标对象实现了接口，可以强制使用CGLIB实现AOP 
>
> 3、如果目标对象没有实现了接口，必须采用CGLIB库，spring会自动在JDK动态代理和CGLIB之间转换
>
> 如何强制使用CGLIB实现AOP？
>
>  （1）添加CGLIB库，SPRING_HOME/cglib/*.jar
>
>  （2）在spring配置文件中加入<aop:aspectj-autoproxy proxy-target-class="true"/>
>
> 
>
> JDK动态代理和CGLIB字节码生成的区别？
>
>  （1）JDK动态代理只能对实现了接口的类生成代理，而不能针对类
>
>  （2）CGLIB是针对类实现代理，主要是对指定的类生成一个子类，覆盖其中的方法
>
>    因为是继承，所以该类或方法最好不要声明成final 
>



AOP代码实现参考资料：[https://www.cnblogs.com/hublogs/p/12035415.html](https://www.cnblogs.com/hublogs/p/12035415.html)

## 4. Spring中的bean是线程安全的吗？
  需要先讲一下spring的bean的作用域，

  作用域有下面几种：

 ![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618213908219-46ccba00-d211-4c41-b843-1b1e96a617cb.png)

  默认，每个容器中只有一个bean的实例，工作中其他作用域几乎都不用。

 

<font style="color:#F5222D;">S</font><font style="color:#F5222D;">pring中的bean绝对不可能是线程安全的！</font>

但是线程不安全也没有什么影响，一般很少有数据定义到service的实例变量中的，大部分情况都是多个线程并发的去访问数据库。 

> 实际上大部分时候 spring bean 无状态的（比如 dao 类），所以某种程度上来说 bean 也是安全的，但如果 bean 有状态的话（比如 view model 对象），那就要开发者自己去保证线程安全了，最简单的就是改变 bean 的作用域，把“singleton”变更为“prototype”，这样请求 bean 相当于 new Bean()了， 所以就可以保证线程安全了。 
>
> 
>
> - [x] 有状态就是有数据存储功能。 
> - [x] 无状态就是不会保存数据。
>





## 5. Spring事务
### 5.1 事务的实现原理
如果你在方法上加了@Teansactional注解，此时Spring会使用AOP的思想，**<font style="color:#F5222D;background-color:#FCFCCA;">在你的方法执行之前，先去开启事务。执行完毕之后根据你的方法是否报错，来决定回滚还是提交这个事务。</font>**

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618214942777-d14ac6b6-d7e7-4400-bde9-1972c5ac4dc6.png)

### 5.2 事务的传播机制
+ **<font style="color:#F5222D;">required</font>**<font style="color:#F5222D;">：</font>若当前存在事务，则加入该事务，若不存在事务，则新建一个事务。<font style="color:#F5222D;background-color:#FADB14;">Spring默认此级别！</font>
+ **<font style="color:#F5222D;">requires_new</font>**：<font style="color:#4D4D4D;">若当前没有事务，则新建一个事务。若当前存在事务，则新建一个事务，新老事务相互独立。</font><font style="color:#F5222D;">外部事务回滚不会影响内部事务的正常提交</font><font style="color:#4D4D4D;">。</font>
+ **<font style="color:#F5222D;">nested</font>**：<font style="color:#4D4D4D;">如果当前存在事务，则嵌套在当前事务中执行。如果当前没有事务，则新建一个事务。</font><font style="color:#F5222D;">外部事务回滚会带着内部事务一起回滚。</font>
+ **supports**：<font style="color:#4D4D4D;">支持当前事务，若当前不存在事务，以非事务的方式执行。</font>
+ **supported**：<font style="color:#4D4D4D;">以非事务的方式执行，若当前存在事务，则把当前事务挂起。</font>
+ **mandatory**：<font style="color:#4D4D4D;">强制事务执行，若当前不存在事务，则抛出异常。</font>
+ **never**：<font style="color:#4D4D4D;">以非事务的方式执行，如果当前存在事务，则抛出异常。</font>

> 事务传播机制参考：[https://blog.csdn.net/qq_17085835/article/details/84837253](https://blog.csdn.net/qq_17085835/article/details/84837253)
>

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618216231925-30a1abdb-03a0-4c5b-8e9a-4a3372ac57b9.png)

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618216267177-d37b5c22-53c9-4d9b-a8e1-36fd74890b16.png)



### 5.3 事务隔离级别
spring 有五大隔离级别，默认值为 ISOLATION_DEFAULT（使用数据库的设置），其他四个隔离级 别和数据库的隔离级别一致。

+ **ISOLATION_DEFAULT**：用底层数据库的设置隔离级别，数据库设置的是什么我就用什么。
+ **ISOLATIONREADUNCOMMITTED**：未提交读，最低隔离级别、事务未提交前，就可被其他事务读取（会出现幻 读、脏读、不可重复读）；
+ **ISOLATIONREADCOMMITTED**：提交读，一个事务提交后才能被其他事务读取到（会造成幻读、不可重复 读），SQL server 的默认级别； 
+ **ISOLATIONREPEATABLEREAD**：可重复读，保证多次读取同一个数据时，其值都和事务开始时候的内容是一 致，禁止读取到别的事务未提交的数据（会造成幻读），MySQL 的默认级别； 
+ **ISOLATION_SERIALIZABLE**：序列化，代价最高最可靠的隔离级别，该隔离级别能防止脏读、不可重复读、 幻读。



### 5.4 事务在什么场景下失效

![img](https://cdn.jsdelivr.net/gh/jlukey/image@main/img/71531fbb14102c7cc6d9ec1d8b0c24ac.png)

**1. 访问权限问题**

> 所谓的访问权限问题也就是开发中再熟悉不过的`private`，`default`，`protected`，`public`，它们的访问权限从左到右，依次变大。如果我们在开发过程中国呢，把某些事务方法定义了错误的访问权限，就会导致事务功能出现问题，甚至失效。例如：

代码语言：javascript

复制

```java
@Service
public class UserService {
 @Transactionsl
 private void add(User user){
  saveUser(user);
  updateUser(user);
 }
}
```

上面代码中我们可以看到对于方法`add`的访问修饰符被定义成了`private`，这样会导致事务失效，原因是：`Spring 要求被代理的方法必须是 **public** 的`。

> 如果我们自定义的事务方法(即目标方法)它的访问权限不是`public`，而是`private`，`defalut`，`protected`修饰符的话，Spring都不会提供事务。



**2. 方法使用final修饰**

在某些场景我们可能需要使用 `final` 修饰方法，为了不让子类重写等原因，针对普通方法而言这是没有任何问题的，但是针对需要加事务的方法则会导致事务失效。如下代码：

```java
@Service
public class UserService {
 @Transactionsl
 private final void add(User user){
  saveUser(user);
  updateUser(user);
 }
}
```

上述代码中的`add`方法被`final`修饰了，从而导致了事务失效。具体原因是什么的。这就要从Spring的源码开始说起了。相比应该是都知道Spring事务的底层其实是使用了AOP，也就是通过`jdk动态代理`或者`cglib`，帮我们生成了代理类，在代理类中实现的事务功能。

但是某个方法被`final`修饰了，那么在它的代理类中，就无法重写该方法，而添加事务功能.

> 注意：如果某个方法是 `static` 修饰的，同样无法通过动态代理，变成事务方法。



**3. 方法内部调用**

在某些场景下，我们需要在某个 service 类中的某个方法中调用另外一个事务方法。比如：

```java
@Service
public class UserService {
 @Autowired
 private UserMapper usermapper;

 public void add(User user){
  userMappper.insertUser(user);
  updateStatus(user);
 }
}

@Transactional
public void updateStatus(User user){
 doSomeThing();
}
```

从上面的方法中我们可以看到 add() 方法中直接调用了事务方法 updateStatus()；因为 updateStatus() 方法拥有事务的能力是因为 Spring AOP 生成了代理对象，但是这种方法直接调用了 this 对象的方法，所以 updateStatus() 方法不会生成事务。

由此可见，在同一类中的方法直接调用，会导致事务失效。

那么我们如何解决在同一方法中调用自己类中的另外一个方法呢？方案如下：

1. 新加一个Service方法, 将同一类中调用与被调用的两个方法拆分为两个Service。

   ```java
   @Service
   public class ServiceA {
    @Autowired
    private ServiceB serviceB;
   
    public void save(User user){
     queryData1();
     queryData2();
     serviceB.doSave(user);
    }
   }
   ```

   ```java
   @Service
   public class ServiceB{
    @Transactional(rollbackFor = Exception.class)
    public void doSave(User user) {
     addData1(user);
     updateData2(user):
    }
   }
   ```

2. 在该类中注入自己也可解决事务失效的问题。（这样不会导致循环依赖哦）。

   ```java
   @Service
   public class ServiceA(){
   
    @Autowired
    private ServiceA serviceA;
   
    public void save(User user){
     queryData1();
     queryData2();
     serviceA.doSave(user);
    }
   
    @Transactional(rollbackFor = Excetion.class)
    public void doSave(User user){
     addData1();
     updateData2(user);
    }
   }
   ```

3. 通过AopContent类，在该Service类中使用`AopContent.currentProxy()`获取对象。

   ```java
   @Service
   public class ServiceA{
   
    public void save(User user){
     queryData1();
     queryData2();
     ( (ServiceA)AopContent.currentProxy() ).doSave(user);
    }
   
   
    @Transactional(rollbackFor = Excetion.class)
    public void doSave(User user){
     addData1(user);
     updateData2(user);
    }
   }
   ```



**4. 未被 spring 管理**

如果需要使用 Spring 事务，是有一个前置条件，那就是对象需要交给 Spring 进行管理，需要创建 bean 实例。通常情况下，我们通过`@Contrlller` 、`@Service`、 `@Component`、 `@Repository` 等注解，实现将 bean 实例化和依赖注入的功能。假设某个时候你再 Service 上没有添加 `@Service` 注解，比如:

```java
public class UserService{

 @Transactional
 public void add(User user){
  saveData(user);
  updateData(user);
 }
}
```

上述代码的 UserService 类没有添加 `@Service` 注解，那么该类就不会交给 Spring 进行统一管理，同时它的 add() 方法也不会生成事务。



**5.多线程调用**

在实际的应用开发中。我们通常使用多线程的场景还是很多的。如果Spring事务用在多线程场景下。同样会有问题。代码如下：

```java
@Slf4j
@Service
public class UserService{
 @Autowired
 private UserMapper userMapper;
 @Autowired
 private RoleService roleService;

 @Transactional
 public void add(User user) throws Exception{
  userMapper.insertUser(user);
  new Thread( ()-> {
   roleService.doOtherThing();
  }).start();
 }
}


@Service
public class RoleService{

 @Transactional
 public void doOtherThing(){
  System.out.println("保存roles数据");
 }

}
```

从上面的例子，我们可以看到事务方法 add() 中，调用了事务方法 doOtherThing() ，但是事务方法 doOtherThing() 是在另外一个线程中调用的。这样会导致两个方法不在一个线程中。获取的数据库连接也就不一致，从而是两个不同的事务。如果 doOtherThing() 方法中抛出了异常， add() 方法是不可能回滚的。

> Spring的事务是通过数据库的连接来实现的。当前线程中保存了一个map，key是数据源，value是数据库连接。
>
> `private static final ThreadLocal<Map<Object,Object>> resources = new NamedThreadLocal<>("Transactional resources");`
>
> 我们说的同一个事务，其实指同一个数据库连接，只有拥有同一个数据库连接才能同时提交和回滚。如果在不同的线程中，拿到的数据库连接肯定是不一样的。所以事务也是不同的。



**6. 表不支持事务**

众所周知，在 MySQL 5.x 之前，默认的数据库引擎是 `MYISAM`。它有一个致命的缺点，那就是不支持事务。在MySQL 5.x之后， `MYISAM` 已经逐渐的退出了历史舞台，取而代之的是引擎 `InnoDB` 。所以在实际的开发中如果事务没有生效，有可能就是因为你的表的引擎不支持事务。



**7. 未开启事务**

有些时候，事务没有生效的根本原因是没有开启事务。

如果你使用的是 Spring Boot 项目，已经通过 `DataSoureTransactionManagerAutoConfiguration` 类，默认开启了事务，你只需要配置 `spring.datasource` 的相关参数即可。

如果是 Spring 项目则需要手动配置事务。



**8. 错误的传播特性**

说到事务的传播特性，首先应该知道事务的传播特性有哪些：

| 事务的传播行为类型        | 说明                                                         |
| :------------------------ | :----------------------------------------------------------- |
| PROPAGATION_REQUIRED      | 如果当前没有事务，就新建一个事务，如果已经存在一个事务，加入到这个事务中，这是最常见的选择。 |
| PROPAGATION_SUPPORTS      | 支持当前事务，如果当前没有事务，则以非事务的方式运行         |
| PROPAGATION_MANDATORY     | 使用当前事务，如果当前没有事务，就抛出异常                   |
| PROPAGATION_REQUIRED_NEW  | 新建事务，如果当前存在事务，把当前事务挂起                   |
| PROPAGATION_NOT_SUPPORTED | 以非事务的方式执行操作，如果当前存在事务，则把事务挂起       |
| PROPAGATION_NEVER         | 以非事务的方式执行，如果当前存在事务，则抛出异常             |
| PROPAGATION_NESTED        | 如果当前存在事务，则在嵌套事务内执行；如果当前没有事务，执行与PROPAGATION_REQUIRED类似的操作 |

如果在编写代码时将事务的传播特性编写出错。比如：

```java
@Service
public class UserService {

 @Transactional(propagation = Propagation.NEVER)
 public void doSave(User user){
  saveData(user);
  updateData(user);
 }
}
```

我们可以看到add方法的事务传播特性定义成了Propagation.NEVER，这种类型的传播特性不支持事务，如果有事务则会抛异常。

>  目前只有三种事务传播特性才会新建事务 `REQUIRED` 、`REQUIRED_NEW`、 `NESTED`。



**9. 自己吞了异常**

在开发过程中，有可能我们在事务中使用了`try{} catch()`了异常。

```java
@Slf4j
@Service
public class UserService {

 @Transactional
 public void add(User user){
  try{
   saveData(user);
   updateData(user);
  } catch (Exception e){
   log.error(e.getMessage(),e);
  }
 }

}
```

按照上述代码编写的话，Spring 事务是不会回滚的，因为开发者自己捕获了异常，同时没有手动抛出。如果想要Spring能够正常回滚，则必须要抛出它能够处理的异常，如果没有抛出异常，Spring则会认为程序是正常的。



**10. 手动抛出了别的异常**

如果出现的异常不正确，Spring事务也不会回滚。

```java
@Slf4j
@Service
public class UserService{

 @Transactional
 public void add(User user) throws Exception {
  try {
   saveData(user);
   updateData(user);
  } catch(Exception e) {
   log.error(e.getMessage(), e);
   throw new Exception(e);
  }
 }

}
```

例如上述代码中，开发人员自己捕获了异常，又手动抛出了异常：Exception，事务同样不会回滚。因为 Spring 事务，默认情况下只会回滚 `RunTimeException` ，和 `Error` (错误)，对于普通的 Exception (非运行时异常)，它是不会回滚的。



**11. 自定义了回滚异常**

在使用@Transactional注解声明事务时，有时我们想自定义回滚的异常，spring也是支持的。可以通过设置`rollbackFor`参数，来完成这个功能。但如果这个参数的值设置错了，就会引出一些莫名其妙的问题，例如：

```java
@Slf4j
@Service
public class UserService {
    
    @Transactional(rollbackFor = BusinessException.class)
    public void add(User user) throws Exception {
       saveData(user);
       updateData(user);
    }
}
```

如果在执行上面这段代码，保存和更新数据时，程序报错了，抛了SqlException、DuplicateKeyException等异常。而BusinessException是我们自定义的异常，报错的异常不属于BusinessException，所以事务也不会回滚。

即使 rollbackFor 有默认值，但阿里巴巴开发者规范中，还是要求开发者重新指定该参数。

> `rollbackFor `默认值为 UncheckedException ，包括了 RuntimeException 和 Error . 当我们直接使用 `@Transactional `不指定 `rollbackFor` 时， Exception 及其子类都不会触发回滚。

所以，建议一般情况下，将该参数设置成：Exception或Throwable。



**12.  嵌套事务回滚多了**

```java
public class UserService {

    @Autowired
    private UserMapper userMapper;

    @Autowired
    private RoleService roleService;

    @Transactional
    public void add(UserModel user) throws Exception {
        userMapper.insertUser(user);
        roleService.doOtherThing();
    }
}

@Service
public class RoleService {

    @Transactional(propagation = Propagation.NESTED)
    public void doOtherThing() {
        System.out.println("保存role表数据");
    }
}
```

这种情况使用了嵌套的内部事务，原本是希望调用roleService.doOtherThing()方法时，如果出现了异常，只回滚doOtherThing方法里的内容，不回滚 userMapper.insertUser里的内容，即回滚保存点。但事实是，insertUser也回滚了。

这是为什么呢？

> 因为doOtherThing()方法出现了异常，没有手动捕获，会继续往上抛，到外层add方法的代理方法中捕获了异常。所以，这种情况是直接回滚了整个事务，不只回滚单个保存点。

如何才能只回滚保存点呢？

```sql
@Slf4j
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    @Autowired
    private RoleService roleService;

    @Transactional
    public void add(User user) throws Exception {

        userMapper.insertUser(userModel);
        try {
            roleService.doOtherThing();
        } catch (Exception e) {
            log.error(e.getMessage(), e);
        }
    }
}
```

可以将内部嵌套事务放在try/catch中，并且不继续往上抛异常。这样就能保证，如果内部嵌套事务中出现异常，只回滚内部事务，而不影响外部事务。




## 6. Spring核心框架
Spring容器根据配置创建bean对象实例，管理bean与bean之间依赖关系(也即依赖注入).

![](https://cdn.nlark.com/yuque/0/2021/png/12493416/1618218572902-8b55ae61-9dc4-4e84-b522-2c19c624f76a.png)

## 7. Spring bean的生命周期
![](https://cdn.nlark.com/yuque/0/2021/jpeg/12493416/1618223882601-49b052d1-8b2a-422f-bd3d-b45901493f3c.jpeg)

> 1. <font style="color:#4D4D4D;">实例化一个Bean。</font>
> 2. <font style="color:#4D4D4D;">按照Spring上下文对实例化的Bean进行配置，也就是IOC注入</font>
> 3. <font style="color:#4D4D4D;">如果这个Bean已经实现了BeanNameAware接口，会调用它实现的setBeanName(String)方法，传递的参数就是Spring配置文件中Bean的id值。</font>
> 4. <font style="color:#4D4D4D;">如果这个Bean已经实现了BeanFactoryAware接口，会调用它实现的setBeanFactory(BeanFactory)，传递的是Spring工厂自身。</font>
> 5. <font style="color:#4D4D4D;">如果这个Bean已经实现了ApplicationContextAware接口，会调用setApplicationContext(ApplicationContext)方法，传入Spring上下文。</font>
> 6. <font style="color:#4D4D4D;">如果这个Bean关联了BeanPostProcessor接口，将会调用postProcessBeforeInitialization(Object obj, String s)方法。BeanPostProcessor经常被用作是Bean内容的更改，并且由于这个是在Bean初始化结束时调用那个的方法，也可以被应用于内存或缓存技术。</font>
> 7. <font style="color:#4D4D4D;">如果Bean在Spring配置文件中配置了init-method属性会自动调用其配置的初始化方法。</font>
> 8. <font style="color:#4D4D4D;">如果这个Bean关联了BeanPostProcessor接口，将会调用postProcessAfterInitialization(Object obj, String s)方法。</font>
> 9. <font style="color:#4D4D4D;">当Bean不再需要时，会经过清理阶段，如果Bean实现了DisposableBean这个接口，会调用那个其实现的destroy()方法。</font>
> 10. <font style="color:#4D4D4D;">最后，如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法。</font>
>



**总结一下**：

Spring Bean的生命周期分为`四个阶段`和`多个扩展点`。扩展点又可以分为`影响多个Bean`和`影响单个Bean`。整理如下：

**四个阶段：**

> + <font style="color:#F5222D;">实例化 Instantiation</font>
> + <font style="color:#F5222D;">属性赋值 Populate</font>
> + <font style="color:#F5222D;">初始化 Initialization</font>
> + <font style="color:#F5222D;">销毁 Destruction</font>
>

多个扩展点：

> + 影响多个Bean
>     - BeanPostProcessor
>     - InstantiationAwareBeanPostProcessor
> + 影响单个Bean
>     - Aware
>         * Aware Group1
>             + BeanNameAware
>             + BeanClassLoaderAware
>             + BeanFactoryAware
>         * Aware Group2
>             + EnvironmentAware
>             + EmbeddedValueResolverAware
>             + ApplicationContextAware(ResourceLoaderAware\ApplicationEventPublisherAware\MessageSourceAware)
>     - 生命周期
>         * InitializingBean
>         * DisposableBean
>



## 8. Spring循环依赖
spring为解决单例的循环依赖问题，使用了三级缓存。bean的获取过程：一级缓存 -> 二级缓存 -> 三级缓存。

+ 一级：用于存放完全初始化好的bean
+ 二级：存放原始的bean对象（尚未填充属性）,用于解决循环依赖
+ 三级：存放bean的工厂对象，解决循环依赖

> **具体过程**：
>
> A创建过程中需要B，于是A将自己放入三级缓存里，去实例化B；
>
> B实例化的时候发现需要A，于是B先查一级缓存 没有，查二级缓存 没有，查三级缓存 有。
>
> 然后把三级缓存里的A放到二级缓存，并删除三级缓存里的A
>
> B顺利初始化完自己后，将自己放到一级缓存（此时B中的A状态依然是创建中）
>
> 然后返回继续创建A，此时B已经结束，直接从一级缓存中拿到B，完成创建，并把自己放入一级缓存。
>



答案：三级缓存

循环依赖场景：

<font style="color:#4D4D4D;">①：</font><font style="color:#4D4D4D;">构造器</font><font style="color:#4D4D4D;">的循环依赖。【这个</font><font style="color:#4D4D4D;background-color:#00CCCC;">Spring解决不了</font><font style="color:#4D4D4D;">】</font>

<font style="color:#4D4D4D;">②：</font><font style="color:#4D4D4D;background-color:#00CCCC;">setter循环依赖，</font><font style="color:#4D4D4D;">field属性的循环依赖</font>



<font style="color:#4D4D4D;">Spring的</font><font style="color:#4D4D4D;">单例对象</font><font style="color:#4D4D4D;">的</font><font style="color:#4D4D4D;">初始化</font><font style="color:#4D4D4D;">主要分为三步：</font>

①：<font style="background-color:#FFFB8F;">createBeanInstance</font>：实例化，其实也就是 <font style="background-color:#FFFB8F;">调用对象的构造方法实例化对象</font>

②：<font style="background-color:#FFFB8F;">populateBean</font>：<font style="background-color:#FFFB8F;">填充属性</font>，这一步主要是多bean的<font style="background-color:#FFFB8F;">依赖属性</font>进行填充

③：<font style="background-color:#FFFB8F;">initializeBean</font>：<font style="background-color:#FFFB8F;">调用spring xml</font>中的<font style="background-color:#FFFB8F;">init()</font><font style="background-color:#FFFB8F;"> </font>方法。

从上面讲述的单例bean初始化步骤我们可以知道：循环依赖主要发生在第一、第二步。也就是<font style="background-color:#FFFB8F;">构造器循环依赖</font><font style="">和</font><font style="background-color:#FFFB8F;">field循环依赖。</font>

<font style="color:#8C8C8C;background-color:#FFCC33;"></font>

<font style="background-color:#FFFB8F;">调整配置文件</font><font style="color:#4D4D4D;background-color:#FFFB8F;">，将</font><font style="background-color:#FFFB8F;">构造函数注入</font><font style="background-color:#FFFB8F;">方式改为 </font><font style="background-color:#FFFB8F;">属性注入</font><font style="color:#4D4D4D;background-color:#FFFB8F;">方式 即可。</font>

<font style="color:#000000;"></font>

A的某个 field 或者 setter 依赖了 B 的实例对象，同时 B 的某个 field 或者 setter 依赖了 A 的实例对象”这种循环依赖的情况。A 首先完成了初始化的第一步，并且将自己提前曝光到 singletonFactories 中，此时进行初始化的第二步，发现自己依赖对象 B ，此时就尝试去 get(B) ，发现 B 还没有被 create ，所以走 create 流程， B 在初始化第一步的时候发现自己依赖了对象 A ，于是尝试 get(A) ，尝试一级缓存 singletonObjects 肯定没有，因为 A 还没初始化完全)，尝试二级缓存 earlySingletonObjects（也没有），尝试三级缓存 singletonFactories ，由于 A 通过 ObjectFactory 将自己提前曝光了，所以 B 能够通过 ObjectFactory.getObject 拿到 A 对象(虽然 A 还没有初始化完全，但是总比没有好呀)， B 拿到 A 对象后顺利完成了初始化阶段 1、2、3，完全初始化之后将自己放入到一级缓存 singletonObjects 中。此时返回 A 中， A 此时能拿到 B 的对象顺利完成自己的初始化阶段 2、3，最终 A 也完成了初始化，进去了一级缓存 singletonObjects 中，而且更加幸运的是，由于 B 拿到了 A 的对象引用，所以 B 现在 hold 住的 A 对象完成了初始化。



<font style="color:#FA541C;background-color:transparent;">知道了这个原理时候，肯定就知道为啥 Spring 不能解决“ A 的构造方法中依赖了B 的实例对象，同时 B 的构造方法中依赖了 A 的实例对象”这类问题了！因为加入 singletonFactories 三级缓存的前提是执行了构造器，所以构造器的循环依赖没法解决</font>



## 9. Spring中都使用了哪些设计模式？
> + **<font style="color:#000000;">工厂设计模式</font>**<font style="color:#000000;"> : Spring使用工厂模式通过 </font><font style="color:#000000;">BeanFactory、</font><font style="color:#000000;">ApplicationContext</font><font style="color:#000000;">创建 bean 对象。</font>
> + **<font style="color:#000000;">代理设计模式</font>**<font style="color:#000000;"> : Spring AOP 功能的实现。</font>
> + **<font style="color:#000000;">单例设计模式</font>**<font style="color:#000000;"> : Spring 中的 Bean 默认都是单例的。</font>
> + **<font style="color:#000000;">模板方法模式</font>**<font style="color:#000000;"> : Spring 中 </font><font style="color:#000000;">jdbcTemplate</font><font style="color:#000000;">、</font>`<font style="color:#000000;">hibernateTemplate</font><font style="color:#000000;"> 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。</font>
> + **<font style="color:#000000;">包装器设计模式</font>**<font style="color:#000000;"> : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。</font>
> + **<font style="color:#000000;">观察者模式:</font>**<font style="color:#000000;"> Spring 事件驱动模型就是观察者模式很经典的一个应用。</font>
> + **<font style="color:#000000;">适配器模式</font>**<font style="color:#000000;"> :Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配</font><font style="color:#000000;">Controller</font><font style="color:#000000;">。</font>
> + <font style="color:#000000;">……</font>
>



## 10.Spring MVC工作流程
> 1. **<font style="color:#FA541C;">DispatcherServlet</font>**<font style="color:#FA541C;">：前端控制器。用户请求到达前端控制器，它就相当于mvc模式中的c，dispatcherServlet是整个流程控制的中心，由它调用其它组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性,系统扩展性提高。由框架实现</font>
> 2. **<font style="color:#FA541C;">HandlerMapping</font>**<font style="color:#FA541C;">：处理器映射器。HandlerMapping负责根据用户请求的url找到Handler即处理器，springmvc提供了不同的映射器实现不同的映射方式，根据一定的规则去查找,例如：xml配置方式，实现接口方式，注解方式等。由框架实现</font>
> 3. **<font style="color:#FA541C;">Handler</font>**<font style="color:#FA541C;">：处理器。Handler 是继DispatcherServlet前端控制器的后端控制器，在DispatcherServlet的控制下Handler对具体的用户请求进行处理。由于Handler涉及到具体的用户业务请求，所以一般情况需要程序员根据业务需求开发Handler。</font>
> 4. **<font style="color:#FA541C;">HandlAdapter</font>**<font style="color:#FA541C;">：处理器适配器。通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。由框架实现。</font>
> 5. **<font style="color:#FA541C;">ModelAndView：</font>**<font style="color:#FA541C;">是springmvc的封装对象，将model和view封装在一起。</font>
> 6. **<font style="color:#FA541C;">ViewResolver</font>**<font style="color:#FA541C;">：视图解析器。ViewResolver负责将处理结果生成View视图，ViewResolver首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成View视图对象，最后对View进行渲染将处理结果通过页面展示给用户。</font>
> 7. **<font style="color:#FA541C;">View</font>**<font style="color:#FA541C;">:  </font><font style="color:#FA541C;">是springmvc的封装对象，是一个接口, springmvc框架提供了很多的View视图类型，包括：jspview，pdfview,jstlView、freemarkerView、pdfView等。一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面。</font>
>

![](https://cdn.nlark.com/yuque/0/2021/webp/12493416/1618284707674-d6febc14-352e-4c70-ad6f-e95437734814.webp)

简化版回答：

1. <font style="color:#F5222D;">spring mvc 先将请求发送给 DispatcherServlet。 </font>
2. <font style="color:#F5222D;">DispatcherServlet 查询一个或多个 HandlerMapping，找到处理请求的 Controller。 </font>
3. <font style="color:#F5222D;">DispatcherServlet 再把请求提交到对应的 Controller。 </font>
4. <font style="color:#F5222D;">Controller 进行业务逻辑处理后，会返回一个 ModelAndView。 </font>
5. <font style="color:#F5222D;">Dispathcher 查询一个或多个 ViewResolver 视图解析器，找到 ModelAndView 对象指定的视图对象。 </font>
6. <font style="color:#F5222D;">视图对象负责渲染返回给客户端</font><font style="color:#F5222D;">。</font>

<font style="color:#F5222D;"></font>

## 11. spring mvc 有哪些组件？ 
+ 前置控制器 DispatcherServlet。 
+ 映射控制器 HandlerMapping。 
+ 处理器 Controller。 
+ 模型和视图 ModelAndView。 
+ 视图解析器 ViewResolver。

## 12. @RequestMapping 的作用是什么？ 
将 http 请求映射到相应的类/方法上。




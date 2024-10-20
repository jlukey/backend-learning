### 1. spring boot 核心配置文件是什么？
spring boot 核心的两个配置文件：

- [x] **bootstrap** (.yml或者.properties)：boostrap由父ApplicationContext加载的，比applicaton优先加载，且 boostrap 里面的属性不能被覆盖； 
- [x] **application**(.yml或者.properties)：用于 spring boot 项目的自动化配置。



### 2. SpringBoot启动类注解?它是由哪些注解组成？
@SpringBootApplication 

+ **@SpringBootConfiguration**:  组合了 @Configuration 注解，实现配置文件的功能。 
+ **<font style="color:#F5222D;">@EnableAutoConfiguration</font>**:  打开自动配置的功能，也可以关闭某个自动配置的选项。 
+ **@SpringBootApplication**(exclude = { DataSourceAutoConfiguration.class }) 
+ **@ComponentScan**:  Spring组件扫描



### 3. spring boot 如何实现热部署？
+ <font style="color:#4D4D4D;">使用springloaded配置pom.xml文件，使用 </font>`<font style="color:#4D4D4D;">mvn spring-boot:run</font>` <font style="color:#4D4D4D;">启动。</font>
+ <font style="color:#4D4D4D;">使用springloaded本地加载启动，配置jvm参数： </font>`<font style="color:#4D4D4D;">-javaagent:<jar包地址> -noverify</font>`
+ 使用 <font style="color:rgba(0, 0, 0, 0.75);">devtools工具包</font>热部署。

### 4. `@Conditional` 条件注解
<font style="color:#333333;">    主要作用就是判断条件是否满足，从而决定是否初始化并向容器注册Bean，</font>



> <font style="color:#333333;">通过</font>`@Conditional`<font style="color:#333333;">注解配合</font>`Condition`<font style="color:#333333;">接口，来决定给一个bean是否创建和注册到Spring容器中，从而实现有选择的加载bean。</font>
>



这样做的目的是什么呢？

+ 当有多个同名bean时，怎么抉择的问题
+ 解决某些bean的创建有其他依赖条件的情况






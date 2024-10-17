## 1. 什么是微服务架构
<font style="color:#333333;">微服务架构就是将单体的应用程序分成多个应用程序，这多个应用程序就成为微服务，每个微服务</font>

<font style="color:#333333;">运行在自己的进程中，并使用轻量级的机制通信。这些服务围绕业务能力来划分，并通过自动化部</font>

<font style="color:#333333;">署机制来独立部署。这些服务可以使用不同的编程语言，不同数据库，以保证最低限度的集中式管</font>

<font style="color:#333333;">理。</font>

### <font style="color:#333333;">2. 为什么需要学习Spring Cloud</font>
<font style="color:#333333;">首先springcloud基于spingboot的优雅简洁，可还记得我们被无数xml支配的恐惧？可还记得</font>

<font style="color:#333333;">springmvc，mybatis错综复杂的配置，有了spingboot，这些东西都不需要了，spingboot好处不</font>

<font style="color:#333333;">再赘诉，springcloud就基于SpringBoot把市场上优秀的服务框架组合起来，通过Spring Boot风</font>

<font style="color:#333333;">格进行再封装屏蔽掉了复杂的配置和实现原理</font>

<font style="color:#333333;">什么叫做开箱即用？即使是当年的黄金搭档dubbo+zookeeper下载配置起来也是颇费心神的！而</font>

<font style="color:#333333;">springcloud完成这些只需要一个jar的依赖就可以了！</font>

<font style="color:#333333;">springcloud大多数子模块都是直击痛点，像zuul解决的跨域，fegin解决的负载均衡，hystrix的熔</font>

<font style="color:#333333;">断机制等等等等</font>

### <font style="color:#333333;">3. Spring Cloud  是什么</font>
<font style="color:#333333;">Spring Cloud是一系列框架的有序集合。它利用Spring Boot的开发便利性巧妙地简化了分布式系</font>

<font style="color:#333333;">统基础设施的开发，如服务发现注册、配置中心、智能路由、消息总线、负载均衡、断路器、数据</font>

<font style="color:#333333;">监控等，都可以用Spring Boot的开发风格做到一键启动和部署。</font>

<font style="color:#333333;">Spring Cloud并没有重复制造轮子，它只是将各家公司开发的比较成熟、经得起实际考验的服务框</font>

<font style="color:#333333;">架组合起来，通过Spring Boot风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留</font>

<font style="color:#333333;">出了一套简单易懂、易部署和易维护的分布式系统开发工具包。</font>

<font style="color:#FF0000;"> </font>

### <font style="color:#333333;">4. SpringCloud的优缺点</font>
<font style="color:#333333;">优点：</font>

<font style="color:#333333;">  1.耦合度比较低。不会影响其他模块的开发。</font>

<font style="color:#333333;">  2.减轻团队的成本，可以并行开发，不用关注其他人怎么开发，先关注自己的开发。</font>

<font style="color:#333333;">  3.配置比较简单，基本用注解就能实现，不用使用过多的配置文件。</font>

<font style="color:#333333;">  4.微服务跨平台的，可以用任何一种语言开发。</font>

<font style="color:#333333;">  5.每个微服务可以有自己的独立的数据库也有用公共的数据库。</font>

<font style="color:#333333;">  6.直接写后端的代码，不用关注前端怎么开发，直接写自己的后端代码即可，然后暴露接口，通过组件进行</font>

<font style="color:#333333;">服务通信。</font>

<font style="color:#333333;">缺点：</font>

<font style="color:#333333;"> 1.部署比较麻烦，给运维工程师带来一定的麻烦。</font>

<font style="color:#333333;">  2.针对数据的管理比麻烦，因为微服务可以每个微服务使用一个数据库。</font>

<font style="color:#333333;">  3.系统集成测试比较麻烦</font>

<font style="color:#333333;">  4.性能的监控比较麻烦。【最好开发一个大屏监控系统】</font>

<font style="color:#333333;">总的来说优点大过于缺点，目前看来Spring Cloud是一套非常完善的分布式框架，目前很多企业开</font>

<font style="color:#333333;">始用微服务、Spring Cloud的优势是显而易见的。因此对于想研究微服务架构的同学来说，学习</font>

<font style="color:#333333;">Spring Cloud是一个不错的选择。</font>

### <font style="color:#333333;">5. SpringBoot和SpringCloud的区别？</font>
<font style="color:#333333;">SpringBoot专注于快速方便的开发单个个体微服务。</font>

<font style="color:#333333;">SpringCloud是关注全局的微服务协调整理治理框架，它将SpringBoot开发的一个个单体微服务整</font>

<font style="color:#333333;">合并管理起来，</font>

<font style="color:#333333;">为各个微服务之间提供，配置管理、服务发现、断路器、路由、微代理、事件总线、全局锁、决策</font>

<font style="color:#333333;">竞选、分布式会话等等集成服务</font>

<font style="color:#333333;">SpringBoot可以离开SpringCloud独立使用开发项目， 但是SpringCloud离不开SpringBoot ，属</font>

<font style="color:#333333;">于依赖的关系</font>

<font style="color:#333333;">SpringBoot专注于快速、方便的开发单个微服务个体，SpringCloud关注全局的服务治理框架。</font>

### <font style="color:#333333;">6. Spring Cloud和SpringBoot版本对应关系</font>
<font style="color:#FF0000;"> </font>

**<font style="color:#777777;">Spring Cloud Version</font>**

<font style="color:#777777;">Hoxton</font>

**<font style="color:#777777;">SpringBoot Version</font>**

<font style="color:#777777;">2.2.x</font>

<font style="color:#777777;">2.1.x</font>

<font style="color:#777777;">2.0.x</font>

<font style="color:#777777;">1.5.x</font>

<font style="color:#777777;">1.5.x</font>

<font style="color:#777777;">Greenwich</font>

<font style="color:#777777;">Finchley</font>

<font style="color:#777777;">Edgware</font>

<font style="color:#777777;">Dalston</font>

### <font style="color:#333333;">7. SpringCloud由什么组成</font>
<font style="color:#333333;">这就有很多了，我讲几个开发中最重要的</font>

<font style="color:#333333;">Spring Cloud Eureka：服务注册与发现</font>

<font style="color:#333333;">Spring Cloud Zuul：服务网关</font>

<font style="color:#333333;">Spring Cloud Ribbon：客户端负载均衡</font>

<font style="color:#333333;">Spring Cloud Feign：声明性的Web服务客户端</font>

<font style="color:#333333;">Spring Cloud Hystrix：断路器</font>

<font style="color:#333333;">Spring Cloud Conﬁg：分布式统一配置管理</font>

<font style="color:#333333;">等20几个框架，开源一直在更新</font>

### <font style="color:#333333;">8. 使用 Spring Boot  开发分布式微服务时，我们面临什么</font>
**<font style="color:#333333;">问题</font>**

<font style="color:#333333;">（1）与分布式系统相关的复杂性-这种开销包括网络问题，延迟开销，带宽问题，安全问题。</font>

<font style="color:#333333;">（2）服务发现-服务发现工具管理群集中的流程和服务如何查找和互相交谈。它涉及一个服务目</font>

<font style="color:#333333;">录，在该目录中注册服务，然后能够查找并连接到该目录中的服务。</font>

<font style="color:#333333;">（3）冗余-分布式系统中的冗余问题。</font>

<font style="color:#333333;">（4）负载平衡 --负载平衡改善跨多个计算资源的工作负荷，诸如计算机，计算机集群，网络链</font>

<font style="color:#333333;">路，中央处理单元，或磁盘驱动器的分布。</font>

<font style="color:#333333;">（5）性能-问题 由于各种运营开销导致的性能问题。</font>

### <font style="color:#333333;">9. Spring Cloud 和dubbo区别?</font>
<font style="color:#333333;">（1）服务调用方式：dubbo是RPC springcloud Rest Api</font>

<font style="color:#333333;">（2）注册中心：dubbo 是zookeeper springcloud是eureka，也可以是zookeeper</font>

<font style="color:#333333;">（3）服务网关，dubbo本身没有实现，只能通过其他第三方技术整合，springcloud有Zuul路由</font>

<font style="color:#333333;">网关，作为路由服务器，进行消费者的请求分发,springcloud支持断路器，与git完美集成配置文件</font>

<font style="color:#333333;">支持版本控制，事物总线实现配置文件的更新与服务自动装配等等一系列的微服务架构要素。</font>

## Eureka<font style="color:#FF0000;"></font>
### <font style="color:#333333;">10. 服务注册和发现是什么意思？Spring Cloud  如何实现？</font>
<font style="color:#333333;">当我们开始一个项目时，我们通常在属性文件中进行所有的配置。随着越来越多的服务开发和部</font>

<font style="color:#333333;">署，添加和修改这些属性变得更加复杂。有些服务可能会下降，而某些位置可能会发生变化。手动</font>

<font style="color:#333333;">更改属性可能会产生问题。 Eureka 服务注册和发现可以在这种情况下提供帮助。由于所有服务都</font>

<font style="color:#333333;">在 Eureka 服务器上注册并通过调用 Eureka 服务器完成查找，因此无需处理服务地点的任何更改</font>

<font style="color:#333333;">和处理。</font>

### <font style="color:#333333;">11. 什么是Eureka</font>
<font style="color:#333333;">Eureka作为SpringCloud的服务注册功能服务器，他是服务注册中心，系统中的其他服务使用</font>

<font style="color:#333333;">Eureka的客户端将其连接到Eureka Service中，并且保持心跳，这样工作人员可以通过Eureka </font>

<font style="color:#333333;">Service来监控各个微服务是否运行正常。</font>

### <font style="color:#333333;">12. Eureka怎么实现高可用</font>
<font style="color:#333333;">集群吧，注册多台Eureka，然后把SpringCloud服务互相注册，客户端从Eureka获取信息时，按</font>

<font style="color:#333333;">照Eureka的顺序来访问。</font>

### <font style="color:#333333;">13. 什么是Eureka的自我保护模式，</font>
<font style="color:#333333;">默认情况下，如果Eureka Service在一定时间内没有接收到某个微服务的心跳，Eureka Service会</font>

<font style="color:#333333;">进入自我保护模式，在该模式下Eureka Service会保护服务注册表中的信息，不在删除注册表中的</font>

<font style="color:#333333;">数据，当网络故障恢复后，Eureka Servic 节点会自动退出自我保护模式</font>

### <font style="color:#333333;">14. DiscoveryClient的作用</font>
<font style="color:#333333;">可以从注册中心中根据服务别名获取注册的服务器信息。</font>

### <font style="color:#333333;">15. Eureka和ZooKeeper都可以提供服务注册与发现的功能,请说说两个的区别？</font>
<font style="color:#333333;">1. ZooKeeper中的节点服务挂了就要选举 在选举期间注册服务瘫痪,虽然服务最终会恢复,但是选举期</font>

<font style="color:#333333;">间不可用的， 选举就是改微服务做了集群，必须有一台主其他的都是从</font>

<font style="color:#333333;">2. Eureka各个节点是平等关系,服务器挂了没关系，只要有一台Eureka就可以保证服务可用，数据都</font>

<font style="color:#333333;">是最新的。 如果查询到的数据并不是最新的，就是因为Eureka的自我保护模式导致的</font>

<font style="color:#333333;">3. Eureka本质上是一个工程,而ZooKeeper只是一个进程</font>

<font style="color:#333333;">4. Eureka可以很好的应对因网络故障导致部分节点失去联系的情况,而不会像ZooKeeper 一样使得整</font>

<font style="color:#333333;">个注册系统瘫痪</font>

<font style="color:#333333;">5. ZooKeeper保证的是CP，Eureka保证的是AP</font>

<font style="color:#FF0000;"> </font>

<font style="color:#333333;">CAP： C：一致性>Consistency; 取舍：(强一致性、单调一致性、会话一致性、最终一致性、弱一</font>

<font style="color:#333333;">致性) A：可用性>Availability; P：分区容错性>Partition tolerance;</font>

## Zuul
**<font style="color:#333333;">16. 什么是网关?</font>**

<font style="color:#333333;">网关相当于一个网络服务架构的入口，所有网络请求必须通过网关转发到具体的服务。</font>

**<font style="color:#333333;">17.  网关的作用是什么</font>**

<font style="color:#333333;">统一管理微服务请求，权限控制、负载均衡、路由转发、监控、安全控制黑名单和白名单等</font>

**<font style="color:#333333;">18. 什么是Spring Cloud Zuul（服务网关）</font>**

<font style="color:#333333;">Zuul是对SpringCloud提供的成熟对的路由方案，他会根据请求的路径不同，网关会定位到指定的</font>

<font style="color:#333333;">微服务，并代理请求到不同的微服务接口，他对外隐蔽了微服务的真正接口地址。 三个重要概</font>

<font style="color:#333333;">念：动态路由表，路由定位，反向代理：</font>

<font style="color:#333333;">动态路由表：Zuul支持Eureka路由，手动配置路由，这俩种都支持自动更新</font>

<font style="color:#333333;">路由定位：根据请求路径，Zuul有自己的一套定位服务规则以及路由表达式匹配</font>

<font style="color:#333333;">反向代理：客户端请求到路由网关，网关受理之后，在对目标发送请求，拿到响应之后在 给</font>

<font style="color:#333333;">客户端</font>

<font style="color:#333333;">它可以和Eureka,Ribbon,Hystrix等组件配合使用，</font>

<font style="color:#333333;">Zuul的应用场景：</font>

<font style="color:#333333;">对外暴露，权限校验，服务聚合，日志审计等</font>

<font style="color:#FF0000;"> </font>

**<font style="color:#333333;">19.  网关与过滤器有什么区别</font>**

<font style="color:#333333;">网关是对所有服务的请求进行分析过滤，过滤器是对单个服务而言。</font>

**<font style="color:#333333;">20.  常用网关框架有那些？</font>**

<font style="color:#333333;">Nginx、Zuul、Gateway</font>

**<font style="color:#333333;">21. Zuul与Nginx有什么区别？</font>**

<font style="color:#333333;">Zuul是java语言实现的，主要为java服务提供网关服务，尤其在微服务架构中可以更加灵活的对网</font>

<font style="color:#333333;">关进行操作。Nginx是使用C语言实现，性能高于Zuul，但是实现自定义操作需要熟悉lua语言，对</font>

<font style="color:#333333;">程序员要求较高，可以使用Nginx做Zuul集群。</font>

**<font style="color:#333333;">22. 既然Nginx可以实现网关？为什么还需要使用Zuul框架</font>**

<font style="color:#333333;">Zuul是SpringCloud集成的网关，使用Java语言编写，可以对SpringCloud架构提供更灵活的服</font>

<font style="color:#333333;">务。</font>

**<font style="color:#333333;">23. 如何设计一套API接口</font>**

<font style="color:#333333;">考虑到API接口的分类可以将API接口分为开发API接口和内网API接口，内网API接口用于局域网，</font>

<font style="color:#333333;">为内部服务器提供服务。开放API接口用于对外部合作单位提供接口调用，需要遵循Oauth2.0权限</font>

<font style="color:#333333;">认证协议。同时还需要考虑安全性、幂等性等问题。</font>

**<font style="color:#333333;">24. ZuulFilter常用有那些方法</font>**

<font style="color:#333333;">Run()：过滤器的具体业务逻辑</font>

<font style="color:#333333;">shouldFilter()：判断过滤器是否有效</font>

<font style="color:#333333;">ﬁlterOrder()：过滤器执行顺序</font>

<font style="color:#333333;">ﬁlterType()：过滤器拦截位置</font>

**<font style="color:#333333;">25. 如何实现动态Zuul网关路由转发</font>**

<font style="color:#333333;">通过path配置拦截请求，通过ServiceId到配置中心获取转发的服务列表，Zuul内部使用Ribbon实</font>

<font style="color:#333333;">现本地负载均衡和转发。</font>

**<font style="color:#333333;">26. Zuul网关如何搭建集群</font>**

<font style="color:#FF0000;"> </font>

<font style="color:#333333;">使用Nginx的upstream设置Zuul服务集群，通过location拦截请求并转发到upstream，默认使用</font>

<font style="color:#333333;">轮询机制对Zuul集群发送请求。</font>

## Ribbon
**<font style="color:#333333;">27.  负载平衡的意义什么？</font>**

<font style="color:#333333;">简单来说： 先将集群，集群就是把一个的事情交给多个人去做，假如要做1000个产品给一个人做</font>

<font style="color:#333333;">要10天，我叫10个人做就是一天，这就是集群，负载均衡的话就是用来控制集群，他把做的最多</font>

<font style="color:#333333;">的人让他慢慢做休息会，把做的最少的人让他加量让他做多点。</font>

<font style="color:#333333;">在计算中，负载平衡可以改善跨计算机，计算机集群，网络链接，中央处理单元或磁盘驱动器等多</font>

<font style="color:#333333;">种计算资源的工作负载分布。负载平衡旨在优化资源使用，最大化吞吐量，最小化响应时间并避免</font>

<font style="color:#333333;">任何单一资源的过载。使用多个组件进行负载平衡而不是单个组件可能会通过冗余来提高可靠性和</font>

<font style="color:#333333;">可用性。负载平衡通常涉及专用软件或硬件，例如多层交换机或域名系统服务器进程。</font>

**<font style="color:#333333;">28. Ribbon是什么？</font>**

<font style="color:#333333;">Ribbon是Netﬂix发布的开源项目，主要功能是提供客户端的软件负载均衡算法</font>

<font style="color:#333333;">Ribbon客户端组件提供一系列完善的配置项，如连接超时，重试等。简单的说，就是在配置文件</font>

<font style="color:#333333;">中列出后面所有的机器，Ribbon会自动的帮助你基于某种规则（如简单轮询，随即连接等）去连</font>

<font style="color:#333333;">接这些机器。我们也很容易使用Ribbon实现自定义的负载均衡算法。（有点类似Nginx）</font>

**<font style="color:#333333;">29. Nginx与Ribbon的区别</font>**

<font style="color:#333333;">Nginx是反向代理同时可以实现负载均衡，nginx拦截客户端请求采用负载均衡策略根据upstream</font>

<font style="color:#333333;">配置进行转发，相当于请求通过nginx服务器进行转发。Ribbon是客户端负载均衡，从注册中心读</font>

<font style="color:#333333;">取目标服务器信息，然后客户端采用轮询策略对服务直接访问，全程在客户端操作。</font>

**<font style="color:#333333;">30. Ribbon底层实现原理</font>**

<font style="color:#333333;">Ribbon使用discoveryClient从注册中心读取目标服务信息，对同一接口请求进行计数，使用%取</font>

<font style="color:#333333;">余算法获取目标服务集群索引，返回获取到的目标服务信息。</font>

**<font style="color:#333333;">@LoadBalanced注解的作用</font>**

<font style="color:#333333;">开启客户端负载均衡。</font>

## Hystrix
**<font style="color:#333333;">31.  什么是断路器</font>**

<font style="color:#FF0000;"> </font>

<font style="color:#333333;">当一个服务调用另一个服务由于网络原因或自身原因出现问题，调用者就会等待被调用者的响应</font>

<font style="color:#333333;">当更多的服务请求到这些资源导致更多的请求等待，发生连锁效应（雪崩效应）</font>

<font style="color:#333333;">断路器有三种状态</font>

<font style="color:#333333;">打开状态：一段时间内 达到一定的次数无法调用 并且多次监测没有恢复的迹象 断路器完全</font>

<font style="color:#333333;">打开 那么下次请求就不会请求到该服务</font>

<font style="color:#333333;">半开状态：短时间内 有恢复迹象 断路器会将部分请求发给该服务，正常调用时 断路器关闭</font>

<font style="color:#333333;">关闭状态：当服务一直处于正常状态 能正常调用</font>

**<font style="color:#333333;">32. 什么是 Hystrix？</font>**

<font style="color:#333333;">在分布式系统，我们一定会依赖各种服务，那么这些个服务一定会出现失败的情况，就会导致雪</font>

<font style="color:#333333;">崩，Hystrix就是这样的一个工具，防雪崩利器，它具有服务降级，服务熔断，服务隔离，监控等</font>

<font style="color:#333333;">一些防止雪崩的技术。</font>

<font style="color:#333333;">Hystrix有四种防雪崩方式:</font>

<font style="color:#333333;">服务降级：接口调用失败就调用本地的方法返回一个空</font>

<font style="color:#333333;">服务熔断：接口调用失败就会进入调用接口提前定义好的一个熔断的方法，返回错误信息</font>

<font style="color:#333333;">服务隔离：隔离服务之间相互影响</font>

<font style="color:#333333;">服务监控：在服务发生调用时,会将每秒请求数、成功请求数等运行指标记录下来。</font>

**<font style="color:#333333;">33.  谈谈服务雪崩效应</font>**

<font style="color:#333333;">雪崩效应是在大型互联网项目中，当某个服务发生宕机时，调用这个服务的其他服务也会发生宕</font>

<font style="color:#333333;">机，大型项目的微服务之间的调用是互通的，这样就会将服务的不可用逐步扩大到各个其他服务</font>

<font style="color:#333333;">中，从而使整个项目的服务宕机崩溃.发生雪崩效应的原因有以下几点</font>

<font style="color:#333333;">单个服务的代码存在bug. 2请求访问量激增导致服务发生崩溃(如大型商城的枪红包，秒杀功能). 3.</font>

<font style="color:#333333;">服务器的硬件故障也会导致部分服务不可用.</font>

**<font style="color:#333333;">34. 在微服务中，如何保护服务?</font>**

<font style="color:#333333;">一般使用使用Hystrix框架，实现服务隔离来避免出现服务的雪崩效应，从而达到保护服务的效</font>

<font style="color:#333333;">果。当微服务中，高并发的数据库访问量导致服务线程阻塞，使单个服务宕机，服务的不可用会蔓</font>

<font style="color:#333333;">延到其他服务，引起整体服务灾难性后果，使用服务降级能有效为不同的服务分配资源,一旦服务</font>

<font style="color:#333333;">不可用则返回友好提示，不占用其他服务资源，从而避免单个服务崩溃引发整体服务的不可用.</font>

**<font style="color:#333333;">35.  服务雪崩效应产生的原因</font>**

<font style="color:#333333;">因为Tomcat默认情况下只有一个线程池来维护客户端发送的所有的请求，这时候某一接口在某一</font>

<font style="color:#333333;">时刻被大量访问就会占据tomcat线程池中的所有线程，其他请求处于等待状态，无法连接到服务</font>

<font style="color:#333333;">接口。</font>

**<font style="color:#333333;">36.  谈谈服务降级、熔断、服务隔离</font>**

<font style="color:#FF0000;"> </font>

<font style="color:#333333;">服务降级：当客户端请求服务器端的时候，防止客户端一直等待，不会处理业务逻辑代码，直接返</font>

<font style="color:#333333;">回一个友好的提示给客户端。</font>

<font style="color:#333333;">服务熔断是在服务降级的基础上更直接的一种保护方式，当在一个统计时间范围内的请求失败数量</font>

<font style="color:#333333;">达到设定值（requestVolumeThreshold）或当前的请求错误率达到设定的错误率阈值</font>

<font style="color:#333333;">（errorThresholdPercentage）时开启断路，之后的请求直接走fallback方法，在设定时间</font>

<font style="color:#333333;">（sleepWindowInMilliseconds）后尝试恢复。</font>

<font style="color:#333333;">服务隔离就是Hystrix为隔离的服务开启一个独立的线程池，这样在高并发的情况下不会影响其他</font>

<font style="color:#333333;">服务。服务隔离有线程池和信号量两种实现方式，一般使用线程池方式。</font>

**<font style="color:#333333;">37.  服务降级底层是如何实现的？</font>**

<font style="color:#333333;">Hystrix实现服务降级的功能是通过重写HystrixCommand中的getFallback()方法，当Hystrix的</font>

<font style="color:#333333;">run方法或construct执行发生错误时转而执行getFallback()方法。</font>

## Feign
**<font style="color:#333333;">38. 什么是Feign？</font>**

<font style="color:#333333;">Feign 是一个声明web服务客户端，这使得编写web服务客户端更容易</font>

<font style="color:#333333;">他将我们需要调用的服务方法定义成抽象方法保存在本地就可以了，不需要自己构建Http请求</font>

<font style="color:#333333;">了，直接调用接口就行了，不过要注意，调用方法要和本地抽象方法的签名完全一致。</font>

**<font style="color:#333333;">39. SpringCloud有几种调用接口方式</font>**

<font style="color:#333333;">Feign</font>

<font style="color:#333333;">RestTemplate</font>

**<font style="color:#333333;">40. Ribbon和Feign调用服务的区别</font>**

<font style="color:#333333;">调用方式同：Ribbon需要我们自己构建Http请求，模拟Http请求然后通过RestTemplate发给其他</font>

<font style="color:#333333;">服务，步骤相当繁琐</font>

<font style="color:#333333;">而Feign则是在Ribbon的基础上进行了一次改进，采用接口的形式，将我们需要调用的服务方法定</font>

<font style="color:#333333;">义成抽象方法保存在本地就可以了，不需要自己构建Http请求了，直接调用接口就行了，不过要</font>

<font style="color:#333333;">注意，调用方法要和本地抽象方法的签名完全一致。</font>

## Bus
**<font style="color:#333333;">41. 什么是 Spring Cloud Bus？</font>**

<font style="color:#333333;">Spring Cloud Bus就像一个分布式执行器，用于扩展的Spring Boot应用程序的配置文件，但也可</font>

<font style="color:#333333;">以用作应用程序之间的通信通道。</font>

<font style="color:#333333;">Spring Cloud Bus 不能单独完成通信，需要配合MQ支持</font>

<font style="color:#333333;">Spring Cloud Bus一般是配合Spring Cloud Conﬁg做配置中心的</font>

<font style="color:#333333;">Springcloud conﬁg实时刷新也必须采用SpringCloud Bus消息总线</font>

<font style="color:#FF0000;"> </font>

## Conﬁg
**<font style="color:#333333;">42. 什么是Spring Cloud Conﬁg?</font>**

<font style="color:#333333;">Spring Cloud Conﬁg为分布式系统中的外部配置提供服务器和客户端支持，可以方便的对微服务</font>

<font style="color:#333333;">各个环境下的配置进行集中式管理。Spring Cloud Conﬁg分为Conﬁg Server和Conﬁg Client两部</font>

<font style="color:#333333;">分。Conﬁg Server负责读取配置文件，并且暴露Http API接口，Conﬁg Client通过调用Conﬁg </font>

<font style="color:#333333;">Server的接口来读取配置文件。</font>

**<font style="color:#333333;">43.  分布式配置中心有那些框架？</font>**

<font style="color:#333333;">Apollo、zookeeper、springcloud conﬁg。</font>

**<font style="color:#333333;">44.  分布式配置中心的作用？</font>**

<font style="color:#333333;">动态变更项目配置信息而不必重新部署项目。</font>

**<font style="color:#333333;">45. SpringCloud Conﬁg  可以实现实时刷新吗？</font>**

<font style="color:#333333;">springcloud conﬁg实时刷新采用SpringCloud Bus消息总线。</font>

## Gateway
**<font style="color:#333333;">46. 什么是Spring Cloud Gateway?</font>**

<font style="color:#333333;">Spring Cloud Gateway是Spring Cloud官方推出的第二代网关框架，取代Zuul网关。网关作为流</font>

<font style="color:#333333;">量的，在微服务系统中有着非常作用，网关常见的功能有路由转发、权限校验、限流控制等作用。</font>

<font style="color:#333333;">使用了一个RouteLocatorBuilder的bean去创建路由，除了创建路由RouteLocatorBuilder可以让</font>

<font style="color:#333333;">你添加各种predicates和ﬁlters，predicates断言的意思，顾名思义就是根据具体的请求的规则，</font>

<font style="color:#333333;">由具体的route去处理，ﬁlters是各种过滤器，用来对请求做各种判断和修改。</font>

## SpringCloud主要项目
<font style="color:#333333;">Spring Cloud的子项目，大致可分成两类，一类是对现有成熟框架"Spring Boot化"的封装和抽</font>

<font style="color:#333333;">象，也是数量最多的项目；第二类是开发了一部分分布式系统的基础设施的实现，如Spring Cloud </font>

<font style="color:#333333;">Stream扮演的就是kafka, ActiveMQ这样的角色。</font>

**<font style="color:#333333;">Spring Cloud Conﬁg</font>**

<font style="color:#333333;">Conﬁg能够管理所有微服务的配置文件</font>

<font style="color:#333333;">集中配置管理工具，分布式系统中统一的外部配置管理，默认使用Git来存储配置，可以支持客户</font>

<font style="color:#333333;">端配置的刷新及加密、解密操作。</font>

<font style="color:#FF0000;"> </font>

**<font style="color:#333333;">Spring Cloud Netﬂix(重点，这些组件用的最多)</font>**

<font style="color:#333333;">Netﬂix OSS 开源组件集成，包括Eureka、Hystrix、Ribbon、Feign、Zuul等核心组件。</font>

<font style="color:#333333;">Eureka：服务治理组件，包括服务端的注册中心和客户端的服务发现机制；</font>

<font style="color:#333333;">Ribbon：负载均衡的服务调用组件，具有多种负载均衡调用策略；</font>

<font style="color:#333333;">Hystrix：服务容错组件，实现了断路器模式，为依赖服务的出错和延迟提供了容错能力；</font>

<font style="color:#333333;">Feign：基于Ribbon和Hystrix的声明式服务调用组件；</font>

<font style="color:#333333;">Zuul：API网关组件，对请求提供路由及过滤功能。</font>

<font style="color:#333333;">我觉得SpringCloud的福音是Netflix，他把人家的组件都搬来进行封装了，使开发者能快速简单安全的使用</font>

**<font style="color:#333333;">Spring Cloud Bus</font>**

<font style="color:#333333;">用于传播集群状态变化的消息总线，使用轻量级消息代理链接分布式系统中的节点，可以用来动态</font>

<font style="color:#333333;">刷新集群中的服务配置信息。</font>

<font style="color:#333333;">简单来说就是修改了配置文件，发送一次请求，所有客户端便会重新读取配置文件。</font>

<font style="color:#333333;">需要利用中间插件MQ</font>

**<font style="color:#333333;">Spring Cloud Consul</font>**

<font style="color:#333333;">Consul 是 HashiCorp 公司推出的开源工具，用于实现分布式系统的服务发现与配置。与其它分布</font>

<font style="color:#333333;">式服务注册与发现的方案，Consul 的方案更“一站式”，内置了服务注册与发现框架、分布一致性</font>

<font style="color:#333333;">协议实现、健康检查、Key/Value 存储、多数据中心方案，不再需要依赖其它工具（比如</font>

<font style="color:#333333;">ZooKeeper 等）。使用起来也较为简单。Consul 使用 Go 语言编写，因此具有天然可移植性(支持</font>

<font style="color:#333333;">Linux、windows和Mac OS X)；安装包仅包含一个可执行文件，方便部署，与 Docker 等轻量级</font>

<font style="color:#333333;">容器可无缝配合。</font>

**<font style="color:#333333;">Spring Cloud Security</font>**

<font style="color:#333333;">安全工具包，他可以对</font>

<font style="color:#333333;">对Zuul代理中的负载均衡从前端到后端服务中获取SSO令牌</font>

<font style="color:#333333;">资源服务器之间的中继令牌</font>

<font style="color:#333333;">使Feign客户端表现得像</font><font style="color:#333333;">OAuth2RestTemplate </font><font style="color:#333333;"> （获取令牌等）的拦截器</font>

<font style="color:#333333;">在Zuul代理中配置下游身份验证</font>

<font style="color:#333333;">Spring Cloud Security提供了一组原语，用于构建安全的应用程序和服务，而且操作简便。可以在</font>

<font style="color:#333333;">外部（或集中）进行大量配置的声明性模型有助于实现大型协作的远程组件系统，通常具有中央身</font>

<font style="color:#333333;">份管理服务。它也非常易于在Cloud Foundry等服务平台中使用。在Spring Boot和Spring </font>

<font style="color:#333333;">Security OAuth2的基础上，可以快速创建实现常见模式的系统，如单点登录，令牌中继和令牌交</font>

<font style="color:#333333;">换。</font>

**<font style="color:#333333;">Spring Cloud Sleuth</font>**

<font style="color:#333333;">在微服务中，通常根据业务模块分服务，项目中前端发起一个请求，后端可能跨几个服务调用才能</font>

<font style="color:#333333;">完成这个请求（如下图）。如果系统越来越庞大，服务之间的调用与被调用关系就会变得很复杂，</font>

<font style="color:#333333;">假如一个请求中需要跨几个服务调用，其中一个服务由于网络延迟等原因挂掉了，那么这时候我们</font>

<font style="color:#333333;">需要分析具体哪一个服务出问题了就会显得很困难。Spring Cloud Sleuth服务链路跟踪功能就可</font>

<font style="color:#FF0000;"> </font>

<font style="color:#333333;">以帮助我们快速的发现错误根源以及监控分析每条请求链路上的性能等等。</font>

**<font style="color:#333333;">Spring Cloud Stream</font>**

<font style="color:#333333;">轻量级事件驱动微服务框架，可以使用简单的声明式模型来发送及接收消息，主要实现为Apache </font>

<font style="color:#333333;">Kafka及RabbitMQ。</font>

**<font style="color:#333333;">Spring Cloud Task</font>**

<font style="color:#333333;">Spring Cloud Task的目标是为Spring Boot应用程序提供创建短运行期微服务的功能。在Spring </font>

<font style="color:#333333;">Cloud Task中，我们可以灵活地动态运行任何任务，按需分配资源并在任务完成后检索结果。</font>

<font style="color:#333333;">Tasks是Spring Cloud Data Flow中的一个基础项目，允许用户将几乎任何Spring Boot应用程序作</font>

<font style="color:#333333;">为一个短期任务执行。</font>

**<font style="color:#333333;">Spring Cloud Zookeeper</font>**

<font style="color:#333333;">SpringCloud支持三种注册方式Eureka， Consul(go语言编写)，zookeeper</font>

<font style="color:#333333;">Spring Cloud Zookeeper是基于Apache Zookeeper的服务治理组件。</font>

**<font style="color:#333333;">Spring Cloud Gateway</font>**

<font style="color:#333333;">Spring cloud gateway是spring官方基于Spring 5.0、Spring Boot2.0和Project Reactor等技术开</font>

<font style="color:#333333;">发的网关，Spring Cloud Gateway旨在为微服务架构提供简单、有效和统一的API路由管理方式，</font>

<font style="color:#333333;">Spring Cloud Gateway作为Spring Cloud生态系统中的网关，目标是替代Netﬂix Zuul，其不仅提</font>

<font style="color:#333333;">供统一的路由方式，并且还基于Filer链的方式提供了网关基本的功能，例如：安全、监控/埋点、</font>

<font style="color:#333333;">限流等。</font>

<font style="color:#FF0000;"> </font>

**<font style="color:#333333;">Spring Cloud OpenFeign</font>**

<font style="color:#333333;">Feign是一个声明性的Web服务客户端。它使编写Web服务客户端变得更容易。要使用Feign，我</font>

<font style="color:#333333;">们可以将调用的服务方法定义成抽象方法保存在本地添加一点点注解就可以了，不需要自己构建</font>

<font style="color:#333333;">Http请求了，直接调用接口就行了，不过要注意，调用方法要和本地抽象方法的签名完全一致。</font>**<font style="color:#333333;"></font>**


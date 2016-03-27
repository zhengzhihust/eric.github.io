---
layout: post
title:  "Spring 知识点回顾"
date:   2016-03-27 22:39:00 +0800
categories: jekyll update
group: navigation
tags: Java
---

### 一、Spring响应HTTP请求的过程

从接受请求到返回响应，Spring MVC框架的众多组件都伸胳膊挽袖子行动起来，各司其职，有条不紊地完成份内的工作。在整个框架中，DispatcherServlet处于核心的位置，它负责协调和组织不同组件，共同完成请求响应的工作。和大多数Web MVC框架一样，Spring MVC通过一个前端Servlet处理器接收所有的请求，并将具体工作委托给其它组件进行具体的处理，DispatcherServlet就是 Spring MVC的前端Servlet处理器。下面我们对Spring MVC处理请求的整体过程做一下高空俯瞰：

* 整个过程开始于客户端发送一个HTTP请求；

* DispatcherServlet接收这个请求后，并将请求的处理工作委托给具体的处理器（Handler），后者负责处理请求执行相应的业务逻辑。在这之前，DispatcherServlet必须能够凭借请求信息（URL或请求参数等）按照某种机制找到请求对应的处理器，DispatcherServlet是通过垂询HandlerMapping完成这一工作的；

* 当DispatcherServlet从HandlerMapping中得到当前请求对应的处理器后，它就将请求分派给这个处理器。处理器根据请求的信息执行相应的业务逻辑，一个设计良好的处理器应该通过调用Service层的业务对象完成业务处理，而非自己越俎代庖。

* 处理器完成业务逻辑的处理后将返回一个ModelAndView给DispatcherServlet，ModelAndView包含了视图逻辑名和渲染视图时需要用到的模型数据对象；

* 由于ModelAndView中包含的是视图逻辑名，DispatcherServlet必须知道这个逻辑名对应的真实视图对象，这项视图解析的工作通过调用ViewResolver来完成；

* 当得到真实的视图对象后，DispatcherServlet将请求分派给这个View对象，由其完成Model数据的渲染工作；

* 最终客户端得到返回的响应，这可能是一个普通的HTML页面，也可能是一个Excel电子表格、甚至是一个PDF文档等不一而足的视图形式，Spring的视图类型是异常丰富和灵活的

### 二、Spring框架

* IoC(Inversion of control): 控制反转，依赖注入（DI）

1）IoC：概念：控制权由对象本身转向容器；由容器根据配置文件去创建实例并创建各个实例之间的依赖关系

2）依赖IoC容器负责管理bean，有两种，一种是BeanFactory，另一种是ApplicationContext，但是ApplicationContext继承与BeanFactory。

核心：bean工厂；在Spring中，bean工厂创建的各个实例称作bean

BeanFactory 在解析配置文件时并不会初始化对象,只有在使用对象时(getBean())才会对该对象进行初始化 ApplicationContext 在解析配置文件时对配置文件中的所有对象都初始化了,getBean()方法只是获取对象的过程

3）DI的实现方式：接口注入；setter注入；构造器注入

* AOP(Aspect-Oriented Programming): 面向方面编程

1）代理的两种方式：

静态代理：针对每个具体类分别编写代理类；针对一个接口编写一个代理类.

动态代理：针对一个方面编写一个InvocationHandler，然后借用JDK反射包中的Proxy类为各种接口动态生成相应的代理类

2）AOP的主要原理：动态代理

实现：有两种：JDK Proxy和Cglib，Spring规定对于有接口的类用JDK Proxy，对于无接口和抽象类用Cglib，虽然Cglib均可以代理，但是Cglib复杂，效率低。但是Cglib有例外，就是代理的类中不能是final修饰的类或者类中有final方法。

### 三、Spring如何管理Bean？

Spring容器最基本的接口就是BeanFactor。BeanFactory负责配置、创建、管理Bean，他有一个子接口：ApplicationContext，因此也称之为Spring上下文。Spring容器负责管理Bean与Bean之间的依赖关系。

BeanFactory接口包含以下几个基本方法：

```java
Boolean containBean(String name):判断Spring容器是否包含id为name的Bean实例。
<T> getBean(Class<T> requiredTypr):获取Spring容器中属于requiredType类型的唯一的Bean实例。
Object getBean(String name)：返回Sprin容器中id为name的Bean实例。
Class <?> getType(String name)：返回容器中指定Bean实例的类型。
```

调用者只需使用getBean()方法即可获得指定Bean的引用，无须关心Bean的实例化过程。即Bean实例的创建过程完全透明。

在使用BeanFactory接口时，我们一般都是使用这个实现类：org.springframework.beans.factory.xml.XmlBeanFactory。然而ApplicationContext作为BeanFactory的子接口，使用它作为Spring容器会更加方便。它的实现类有：FileSystemXmlApplicationContext、ClassPathXmlApplicationContext、AnnotationConfigApplicationContext。

创建Spring容器实例时，必须提供Spring容器管理的Bean的详细配置信息。Spring的配置信息通常采用xml配置文件来设置，因此，创建BeanFactory实例时，应该提供XML配置文件作为参数。

XML配置文件通常使用Resource对象传入。Resource接口是Spring提供的资源访问接口，通过使用该接口，Spring能够以简单、透明的方式访问磁盘、类路径以及网络上的资源。

### 四、Spring Security机制

在SpringSide 3的官方文档中，说安全框架使用的是Spring Security 2.0。乍一看，吓了我一跳，以为Acegi这么快就被淘汰了呢。上搜索引擎一搜，发现原来Spring Security 2.0就是Acegi 2.0。悬着的心放下来了。虽然SpringSide 3中关于Acegi的配置文件看起来很不熟悉，但是读了Acegi 2.0的官方文档后，一切都释然了。

先来谈一谈Acegi的基础知识，Acegi的架构比较复杂，但是我希望我下面的只言片语能够把它说清楚。大家都知道，如果要对Web资源进行保护，最好的办法莫过于Filter，要想对方法调用进行保护，最好的办法莫过于AOP。Acegi对Web资源的保护，就是靠Filter实现的。如下图：

![Acegi](https://raw.githubusercontent.com/zhengzhihust/zhengzhihust.github.io/master/pics/2016-03-27-spring-knowledge-point-pic_01.png)

一般来说，我们的Filter都是配置在web.xml中，但是Acegi不一样，它在web.xml中配置的只是一个代理，而真正起作用的Filter是作为Bean配置在Spring中的。web.xml中的代理依次调用这些Bean，就实现了对Web资源的保护，同时这些Filter作为Bean被Spring管理，所以实现AOP也很简单，真的是一举两得啊。

Acegi中提供的Filter不少，有十多个，一个一个学起来比较复杂。但是对于我们Web开发者来说，常用的就那么几个，如下图中的被红圈圈标记出来的：

![Acegi Usage](https://raw.githubusercontent.com/zhengzhihust/zhengzhihust.github.io/master/pics/2016-03-27-spring-knowledge-point-pic_02.png)

从上到下，它们实现的功能依次是1、制定必须为https连接；2、从Session中提取用户的认证信息；3、退出登录；4、登录；5、记住用户；6、所有的应用必须配置这个Filter。

一般来说，我们写Web应用只需要熟悉这几个Filter就可以了，如果不需要https连接，连第一个也不用熟悉。但是有人肯定会想，这些Filter怎么和我的数据库联系起来呢？不用着急，这些Filter并不直接处理用户的认证，也不直接处理用户的授权，而是把它们交给了认证管理器和决策管理器。如下图：

![Acegi Filter](https://raw.githubusercontent.com/zhengzhihust/zhengzhihust.github.io/master/pics/2016-03-27-spring-knowledge-point-pic_03.png)

对于这两种管理器，那也是不需要我们写代码的，Acegi也提供了现成的类。那么大家又奇怪了：又是现成的，那怎么和我的数据库关联起来呢？别着急，其实这两个管理器自己也不做事，认证管理器把任务交给了Provider，而决策管理器则把任务交给了Voter，如下图：

![Acegi Voter](https://raw.githubusercontent.com/zhengzhihust/zhengzhihust.github.io/master/pics/2016-03-27-spring-knowledge-point-pic_04.png)

现在我要告诉你们，这里的Provider和Voter也是不需要我们写代码的。不要崩溃，快到目标了。Acegi提供了多个Provider的实现类，如果我们想用数据库来储存用户的认证数据，那么我们就选择DaoAuthenticationProvider。对于Voter，我们一般选择RoleVoter就够用了，它会根据我们配置文件中的设置来决定是否允许某一个用户访问制定的Web资源。而DaoAuthenticationProvider也是不直接操作数据库的，它把任务委托给了UserDetailService，如下图：

![Acegi UserDetailService](https://raw.githubusercontent.com/zhengzhihust/zhengzhihust.github.io/master/pics/2016-03-27-spring-knowledge-point-pic_05.png)

而我们要做的，就是实现这个UserDetailService。图画得不好，大家不要见笑，但是说了这么多总算是引出了我们开发中的关键，那就是我们要实现自己的UserDetailService，它就是连接我们的数据库和Acegi的桥梁。UserDetailService的要求也很简单，只需要一个返回org.springframework.security.userdetails.User对象的loadUserByUsername(String userName)方法。因此，怎么设计数据库都可以，不管我们是用一个表还是两个表还是三个表，也不管我们是用户-授权，还是用户-角色-授权，还是用户-用户组-角色-授权，这些具体的东西Acegi统统不关心，它只关心返回的那个User对象，至于怎么从数据库中读取数据，那就是我们自己的事了。

反过来再看看上面的过程，我们发现，即使我们要做的只是实现自己的UserDetailService类，但是我们不得不在Spring中配置那一大堆的Bean，包括几个Filter，几个Manager，几个Provider和Voter，而这些配置往往都是重复的无谓的。好在Acegi 2.0也认识到了这个问题，所以，它设计了一个<http>标签，让Acegi的配置得到了简化。下面是SpringSide 3中的配置的截图，大家可以看看：

![Acegi SpringSide3](https://raw.githubusercontent.com/zhengzhihust/zhengzhihust.github.io/master/pics/2016-03-27-spring-knowledge-point-pic_06.png)

下图是官方文章中的传统Filter设置和<http>元素之间的对应关系：

![Acegi http](https://raw.githubusercontent.com/zhengzhihust/zhengzhihust.github.io/master/pics/2016-03-27-spring-knowledge-point-pic_07.png)

下面的代码是SpringSide 3中实现UserDetailService的范例，在SpringSide 3的范例中，白衣使用了三个表User、Role、Authority。但是Acegi不关心你用了几个表，它只关心UserDetails对象。而决定用户能否访问指定Web资源的，是RoleVoter类，无需任何修改它可以工作得很好，唯一的缺点是它只认ROLE_前缀，所以搞得白衣的Authority看起来都象角色，不伦不类。

```java
import java.util.ArrayList;
import java.util.List;

import org.springframework.beans.factory.annotation.Required;
import org.springframework.dao.DataAccessException;
import org.springframework.security.GrantedAuthority;
import org.springframework.security.GrantedAuthorityImpl;
import org.springframework.security.userdetails.UserDetails;
import org.springframework.security.userdetails.UserDetailsService;
import org.springframework.security.userdetails.UsernameNotFoundException;
import personal.youxia.entity.user.Authority;
import personal.youxia.entity.user.Role;
import personal.youxia.entity.user.User;
import personal.youxia.service.user.UserManager;

/**
 * 实现SpringSecurity的UserDetailsService接口,获取用户Detail信息.
 *
 * @author calvin
 */
public class UserDetailServiceImpl implements UserDetailsService {

    private UserManager userManager;

    public UserDetails loadUserByUsername(String userName) throws UsernameNotFoundException, DataAccessException {
        User user = userManager.getUserByLoginName(userName);
        if (user == null)
            throw new UsernameNotFoundException(userName + " 不存在");

        List<GrantedAuthority> authsList = new ArrayList<GrantedAuthority>();

        for (Role role : user.getRoles()) {
            for (Authority authority : role.getAuths()) {
                authsList.add(new GrantedAuthorityImpl(authority.getName()));
            }
        }

        // 目前在MultiDatabaseExample的User类中没有enabled, accountNonExpired,credentialsNonExpired, accountNonLocked等属性
        // 暂时全部设为true,在需要时才添加这些属性.
        org.springframework.security.userdetails.User userdetail = new org.springframework.security.userdetails.User(
                user.getLoginName(), user.getPassword(), true, true, true, true, authsList
                        .toArray(new GrantedAuthority[authsList.size()]));

        return userdetail;
    }

    @Required
    public void setUserManager(UserManager userManager) {
        this.userManager = userManager;
    }
}
```

### 五、Spring DM 与OSGI

1、OSGi 的体系架构是基于插件式的软件结构，包括一个 OSGi 框架和一系列插件，在 OSGi中，插件称为 Bundle，其中，OSGi 框架规范是 OSGi 规范的核心部分，它提供了一个通用的、安全可管理的 Java 框架，通过这个框架，可以支持 Bundle 服务应用的部署和扩展。Bundle 之间可以通过 Import Package 和 Require-Bundle 来共享 Java 类，在 OSGi 服务平台中，用户通过开发 Bundle 来提供需要的功能，这些 Bundle 可以动态加载和卸载，或者根据需要远程下载和升级。OSGi 体系结构图如图 1 所示：

![osgi](https://raw.githubusercontent.com/zhengzhihust/zhengzhihust.github.io/master/pics/2016-03-27-spring-knowledge-point-pic_08.png)

其中：

Execution Environment：

Bundle 应用所倚赖运行的 Java 执行环境，如 J2SE-1.4、CDC-1.0 等都是可用的执行环境。

Modules：

模块层定义了 Bundle 应用的加载策略。OSGi 框架是一个健壮并且严格定义的类加载模型。在大多数 Java 应用中，通常只有一个单独的 ClassPath，它包含了所有的 Java 类文件和资源文件，OSGi基于Java技术，对于每个实现了 BundleActivator 接口的 Bundle 应用，为它生成一个单独的 ClassLoader，使得 Bundle 应用的组织更加模块化。

Life Cycle：

生命周期层可以动态地对 Bundle 进行安装、启动、停止、升级和卸载等操作。该层基于模块层，提供了一组 API 来控制 Bundle 应用的运行时操作。

Service Registry 和 Services：

OSGi 服务层定义了一个集成在生命周期层中的动态协作模型，是一个发布、动态寻找、绑定的服务模型。一个服务通常是一个 Java 对象实现了特定的服务接口，并且通过服务注册，被绑定到 OSGi 的运行环境中。Bundle 应用可以注册发布服务，动态绑定服务，并且在服务注册状态改变时，可以接受到事件消息等。

Security：

OSGi 的安全管理是基于 Java2 安全体系的，贯穿在 OSGi 平台的所有层中，它能够对部署在 OSGi 运行环境中的 Bundle 应用进行详细的管理控制。

2、Bundle 生命周期的状态分析

在一个动态扩展的 OSGi 环境中，OSGi 框架管理 Bundle 的安装和更新，同时也管理 Bundle 和服务之间的依赖关系。一个 Bundle 可能处于以下六个状态，如图 2 所示：

![Bundle](https://raw.githubusercontent.com/zhengzhihust/zhengzhihust.github.io/master/pics/2016-03-27-spring-knowledge-point-pic_09.png)

INSTALLED：安装完成，本地资源成功加载。

RESOLVED：依赖关系满足，这个状态意味该Bundle要么已经准备好运行，要么是被停止了。

STARTING：Bundle正在被启动，BundleActivator的start()方法已经被调用但是还没有返回。

STOPPING：Bundle正在被停止，BundleActivator的stop()方法已经被调用但是还没有返回。

ACTIVE：Bundle 被成功启动并且在运行。

UNINSTALLED：bundle被卸载并且无法进入其他状态。

Bundle接口定义了getState()方法来返回Bundle的状态。

### 五、Spring单例与线程安全

Spring框架里的bean，或者说组件，获取实例的时候都是默认的单例模式，这是在多线程开发的时候要尤其注意的地方。

单例模式的意思就是只有一个实例。单例模式确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。这个类称为单例类。

当多用户同时请求一个服务时，容器会给每一个请求分配一个线程，这是多个线程会并发执行该请求多对应的业务逻辑（成员方法），此时就要注意了，如果该处理逻辑中有对该单列状态的修改（体现为该单列的成员属性），则必须考虑线程同步问题

* 同步机制的比较　　

ThreadLocal和线程同步机制相比有什么优势呢？ThreadLocal和线程同步机制都是为了解决多线程中相同变量的访问冲突问题。

在同步机制中，通过对象的锁机制保证同一时间只有一个线程访问变量。这时该变量是多个线程共享的，使用同步机制要求程序慎密地分析什么时候对变量进行读写，什么时候需要锁定某个对象，什么时候释放对象锁等繁杂的问题，程序设计和编写难度相对较大。

而ThreadLocal则从另一个角度来解决多线程的并发访问。ThreadLocal会为每一个线程提供一个独立的变量副本，从而隔离了多个线程对数据的访问冲突。因为每一个线程都拥有自己的变量副本，从而也就没有必要对该变量进行同步了。ThreadLocal提供了线程安全的共享对象，在编写多线程代码时，可以把不安全的变量封装进ThreadLocal。

由于ThreadLocal中可以持有任何类型的对象，低版本JDK所提供的get()返回的是Object对象，需要强制类型转换。但JDK 5.0通过泛型很好的解决了这个问题，在一定程度地简化ThreadLocal的使用。

概括起来说，对于多线程资源共享的问题，同步机制采用了“以时间换空间”的方式，而ThreadLocal采用了“以空间换时间”的方式。前者仅提供一份变量，让不同的线程排队访问，而后者为每一个线程都提供了一份变量，因此可以同时访问而互不影响。

* Spring使用ThreadLocal解决线程安全问题

我们知道在一般情况下，只有无状态的Bean才可以在多线程环境下共享，在Spring中，绝大部分Bean都可以声明为singleton作用域。就是因为Spring对一些Bean（如RequestContextHolder、TransactionSynchronizationManager、LocaleContextHolder等）中非线程安全状态采用ThreadLocal进行处理，让它们也成为线程安全的状态，因为有状态的Bean就可以在多线程中共享了。

一般的Web应用划分为展现层、服务层和持久层三个层次，在不同的层中编写对应的逻辑，下层通过接口向上层开放功能调用。在一般情况下，从接收请求到返回响应所经过的所有程序调用都同属于一个线程

ThreadLocal是解决线程安全问题一个很好的思路，它通过为每个线程提供一个独立的变量副本解决了变量并发访问的冲突问题。在很多情况下，ThreadLocal比直接使用synchronized同步机制解决线程安全问题更简单，更方便，且结果程序拥有更高的并发性。

如果你的代码所在的进程中有多个线程在同时运行，而这些线程可能会同时运行这段代码。如果每次运行结果和单线程运行的结果是一样的，而且其他的变量的值也和预期的是一样的，就是线程安全的。 或者说:一个类或者程序所提供的接口对于线程来说是原子操作或者多个线程之间的切换不会导致该接口的执行结果存在二义性,也就是说我们不用考虑同步的问题。 　线程安全问题都是由全局变量及静态变量引起的。

若每个线程中对全局变量、静态变量只有读操作，而无写操作，一般来说，这个全局变量是线程安全的；若有多个线程同时执行写操作，一般都需要考虑线程同步，否则就可能影响线程安全。

1） 常量始终是线程安全的，因为只存在读操作。

2）每次调用方法前都新建一个实例是线程安全的，因为不会访问共享的资源。

3）局部变量是线程安全的。因为每执行一个方法，都会在独立的空间创建局部变量，它不是共享的资源。局部变量包括方法的参数变量和方法内变量。

有状态就是有数据存储功能。有状态对象(Stateful Bean)，就是有实例变量的对象  ，可以保存数据，是非线程安全的。在不同方法调用间不保留任何状态。

无状态就是一次操作，不能保存数据。无状态对象(Stateless Bean)，就是没有实例变量的对象  .不能保存数据，是不变类，是线程安全的。

有状态对象:

无状态的Bean适合用不变模式，技术就是单例模式，这样可以共享实例，提高性能。有状态的Bean，多线程环境下不安全，那么适合用Prototype原型模式。Prototype: 每次对bean的请求都会创建一个新的bean实例。

Struts2默认的实现是Prototype模式。也就是每个请求都新生成一个Action实例，所以不存在线程安全问题。需要注意的是，如果由Spring管理action的生命周期， scope要配成prototype作用域。

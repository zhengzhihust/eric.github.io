---
layout: post
title:  "Git-flow 使用笔记"
date:   2016-03-27 22:39:00 +0800
categories: jekyll update
group: navigation
tags: Java
---

#### 实现配置文件读取

> PropertyPlaceholderConfigurer类的主要的用法是将BeanFactory里定义的内容放在一个.properties的文件中.
> PropertyPlaceholderConfigurer是个bean工厂后置处理器的实现，也就是BeanFactoryPostProcessor接口的一个实现.
> PropertyPlaceholderConfigurer可以将上下文（配置文件）中的属性值放在另一个单独的标准java Properties文件中去. 这样的话，我只需要对properties文件进行修改，而不用对xml配置文件进行修改.

```java
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.ConfigurableListableBeanFactory;
import org.springframework.beans.factory.config.PropertyPlaceholderConfigurer;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.Properties;

/**
 * 配置读取工具
 * Created by zhengzhihust on 15/9/23.
 */
public class NotifyPropertyConfigure extends PropertyPlaceholderConfigurer {

    @Override
    protected void processProperties(ConfigurableListableBeanFactory beanFactory, Properties props) throws BeansException {
        super.processProperties(beanFactory, props);
        List<String> topics = new ArrayList<>();
        for (Map.Entry<Object, Object> entry : props.entrySet()) {
          //读取配置文件中的信息
        }
    }
}

```

当然，spring.xml配置文件还需要引用一下资源文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.mogujie.com/schema/tesla http://www.mogujie.com/schema/tesla/tesla.xsd">

    <bean id="id" class="全限定类名">
        <property name="locations">
            <list>
                <value>classpath:conf/example.properties</value>
            </list>
        </property>
    </bean>

</beans>

```

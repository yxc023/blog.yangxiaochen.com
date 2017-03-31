+++
date = "2017-03-30T19:19:32+08:00"
draft = false
title = "spring & spring mvc 初始化介绍"
categories = [ "spring","spring mvc","config","context" ]
type="java"

+++

# spring & spring mvc 初始化介绍

## web项目的结构

### java servlet 技术

请看实例简单复习 servlet, listner, filter

### 非 servlet 规范的 java web 项目

使用netty作为服务器lib, 自行完成处理 http 请求读写

### spring 的的封装

SpringContextListener 与 DispatcherServlet 结合

## spring context 结构

### 继承树

![context继承.png](http://img.yangxiaochen.com/image/blog/context继承.png)

### 重要组件

* `BeanFactory` 通用组件, 负责 bean 注册和初始化
* `Environment` 通用组件 , 包含系统参数, 启动参数等. 还有对启动参数解析后的属性, 比如 profile
* `ApplicationListener`s 通用组件, 保存注册进来的context事件listener
* 一些 context 状态标志, parent
* `ServletContext`, `ServletConfig` WebApplicationContext类里特有的


* 一些特定的参数. 在特定context类型中, 比如

```java
/** Default config location for the root context */
public static final String DEFAULT_CONFIG_LOCATION = "/WEB-INF/applicationContext.xml";

/** Default prefix for building a config location for a namespace */
public static final String DEFAULT_CONFIG_LOCATION_PREFIX = "/WEB-INF/";

/** Default suffix for building a config location for a namespace */
public static final String DEFAULT_CONFIG_LOCATION_SUFFIX = ".xml";	
```





## spring 初始化过程概览(非 spring boot 的传统war包)

### ContextLoaderListener

`ContextLoaderListener` 配置在 web.xml 中, 在容器启动时初始化.

![ContextLoaderListener.png](http://img.yangxiaochen.com/image/blog/ContextLoaderListener.png)



部分较为重要步骤解释:

>3) 决定spring web app context类型. 不指定的情况下默认策略创建 `XmlWebApplicationContext`

>16) 以 bean 的 configuration 文件, 就是一般我们说的xml文件 为入口, 加载 `BeanDefinition` 到 bean factory. 注意仅仅是加载 bean 的描述, 而没有实例化这些 bean

> 17) 实例化执行所有的 `BeanFactoryPostProcessor`, 从 `BeanDefinition` 中和 context 的 `beanFactoryPostProcessors` 字段中查找.
> `BeanFactoryPostProcessor`是意图在 bean factory 加载了所有定义的 bean 定义之后, 且在这些 bean 实例化之前, 做一些操作.
> 这一步通常在调用各个 processor 时, 产生新的 bean 定义到 bean factory

> 18) 注册 `BeanPostProcessor` , 从 `BeanDefinition` 中查找.
> `BeanPostProcessor` 会在之后每个 bean 实例化之后调用, 用来对 bean 做一些其他操作, 比如放入一些参数: 像 `AutowiredAnnotationBeanPostProcessor` 的作用就是注入 `@Autowired` 字段.
> 生成动态代理对象也是通过  `BeanPostProcessor` 实现的.

> 21) `onRefresh ` 是用来初始化其他的特殊的 bean, 这部分逻辑通常在特殊的 context 子类实现
> 比如在 spring boot 中使用的 `AnnotationConfigEmbeddedWebApplicationContext` 中, 会在这里初始化并启动内嵌服务器 

> 22) 将在  `BeanDefinition` 中的, 还有之前设置到context属性中的  `ApplicationListener` 加到广播列表中

> 23) 将  `BeanDefinition`  中其他的非懒加载的 bean 实例化.

> 24) 实例化并调用 `LifecycleProcessor` , 然后广播 `ContextRefreshedEvent`

> 26) 将 context 放到 `ServletContext` 的 attribute 属性里, 之后 `DispatcherServlet` 初始化会用到

> > *以上说的 bean 均为 singleton 的 scope


### DispatcherServlet

`DispatcherServlet` 配置在 web.xml 中, 在第一次访问时初始化.

![DispatcherServlet.png](http://img.yangxiaochen.com/image/blog/DispatcherServlet.png)

部分较为重要步骤解释:

> 5) 以 `ContextLoaderListener`创建的 context 为 parent, 创建新的 `XmlWebApplicationContext`

> 8) 创建一个 `ContextRefreshListener` , 加入到 context 中, 监听 `ContextRefreshedEvent`

> 9) refresh 过程跟`ContextLoaderListener` 一样, 不同的是在查找 bean 时, 能够查到 parent context 的 bean, 供新的 context 初始化使用.

> 10) 触发  `ContextRefreshedEvent` , 初始化 spring mvc 的组件, 添加到 `DispatcherServlet` 中.

## 一些特殊类型Bean的初始化时机

TODO

## Bean 的初始化过程详解

TODO
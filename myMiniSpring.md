# introduction

mini Spring的项目

一些组件 接口 的记录 

开发日志

# simple-bean-container

![截屏2024-06-30 16.31.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-30%2016.31.20.png)

项目初始化后的第一个分支 simple-bean-container

模拟最简单的bean的管理容器 beanfactory

容器只有register和get两个功能 模拟bean按照name的注册和获取





# bean-definition-and-bean-definition-registry

BeanDefinition，顾名思义，用于定义bean信息的类，包含bean的class类型、构造参数、属性值等信息，**每个bean对应一个BeanDefinition的实例**。简化BeanDefinition仅包含bean的class类型



BeanDefinitionRegistry，BeanDefinition注册表接口，定义注册BeanDefinition的方法。



bean容器作为BeanDefinitionRegistry和SingletonBeanRegistry的实现类，具备两者的能力。向bean容器中注册BeanDefinition后，使用bean时才会实例化。

![截屏2024-06-30 17.46.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-06-30%2017.46.42.png)

bean的实例对象存储在DefaultSingletonBeanRegistry中的singletonObjects（map）中

SingletonBeanRegistry及其实现类DefaultSingletonBeanRegistry，**定义添加和获取单例bean的方法**



beanFactory中定义**getBean**的接口

abstractBeanFactory实现BeanFactory的**获取bean的方法，**获取单例bean不存在，**就用Beandefinitioin注册一个bean**

注册过程：

​	defaultListableBeanFactory实现 BeanDefinitionRegister中的注册bean的方法 可以向beanDefinitionMap中注册beanDefinition  **实现对beanDefinitionMap的增加和获取beanDefinition**

​	AbstractAutowireCapableBeanfactory中**会解析beanDefinition信息**，将bean初始化，自动装配，如果可以成功配置bean(传入参数中的beanDefinition合法)，那么就把**bean加入singletonObjects**，并返回bean

​	

#  instantiation-strategy

在之前的实现中，bean的实例化是通过 abstractAutowireCapableBeanFactory 中的beanClass.newInstance来实例化，这样子只能用于bean的无参构造方法

![截屏2024-07-08 09.35.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2009.35.44.png)

![截屏2024-07-08 09.42.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-08%2009.42.45.png)

CGlib是另一种依赖其他库的动态代理方式，通过创建目标的子类来实现

simple中，直接使用反射获得class的constructor来newInstance





# populate-bean-with-property-values

在BeanDefinition中增加和bean属性对应的PropertyValues，实例化bean之后，为bean填充属性(AbstractAutowireCapableBeanFactory）

先是创建**PropertyValues** 实体类，存放bean的属性的name和value

在abstractAutowireCapableBeanFactory中先是通过反射set的方式

**通过name获得bean中其对应的属性的类型，**

**拼接set方法(set+字段首字母大写) 得到functionName**

**由于函数可能存在重构，需要functionName和类型（变量数组，刚才得到了）共同确定出mthod 然后method直接invoke就可以模拟调用set方法进行bean的属性填充了**

# populate-bean-with-bean

这里是解决 bean的成员变量也是一个bean的情况

**对bean中的bean封装beanReferenece**

在做属性填充的时候，发现这个value是beanReference类型的，就对这个value做getBean，获得其Object 然后再做属性填充

# resource-and-resource-loader

**资源和资源加载器**

![截屏2024-07-11 16.39.30](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-11%2016.39.30.png)

Resource是资源顶级接口 

**分别实现有关file，classpath和url的三个实现类**

都是通过路径获取 InputStream



**ResourceLoader是资源加载器接口，实现类为defaultResourceLoader，作用为输入一个地址，判断属于Resource三个实现类中的哪一个，并作出输入流的返回，加载资源**

# xml-file-define-bean

**定义读取bean配置信息的接口 - BeanDefinitionReader - 要有资源加载器功能和beanDefinition功能**



**在XmlBeanDefinition中**

**利用w3c工具包将 xml文件解析成文档数结构 文档对象模型**

**document(整个文档)**

**element(一个元素)**

**node(节点)**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<catalog>
    <book id="bk101">
        <author>Gambardella, Matthew</author>
        <title>XML Developer's Guide</title>
        <genre>Computer</genre>
        <price>44.95</price>
        <publish_date>2000-10-01</publish_date>
        <description>An in-depth look at creating applications with XML.</description>
    </book>
    <book id="bk102">
        <author>Ralls, Kim</author>
        <title>Midnight Rain</title>
        <genre>Fantasy</genre>
        <price>5.95</price>
        <publish_date>2000-12-16</publish_date>
        <description>A former architect battles corporate zombies, an evil sorceress, and her own childhood to become queen of the world.</description>
    </book>
</catalog>

```



```
Document
  |
  |-- catalog
       |
       |-- book (id="bk101")
       |     |
       |     |-- author: "Gambardella, Matthew"
       |     |-- title: "XML Developer's Guide"
       |     |-- genre: "Computer"
       |     |-- price: "44.95"
       |     |-- publish_date: "2000-10-01"
       |     |-- description: "An in-depth look at creating applications with XML."
       |
       |-- book (id="bk102")
             |
             |-- author: "Ralls, Kim"
             |-- title: "Midnight Rain"
             |-- genre: "Fantasy"
             |-- price: "5.95"
             |-- publish_date: "2000-12-16"
             |-- description: "A former architect battles corporate zombies, an evil sorceress, and her own childhood to become queen of the world."

```

**通过 遍历一个elememt 先解析其本身元素**  

![截屏2024-07-11 20.53.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-11%2020.53.28.png)

**然后循环遍历节点**

![截屏2024-07-11 20.54.10](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-11%2020.54.10.png)

核心代码

```java
protected void doLoadBeanDefinitions(InputStream inputStream) {
        Document document = XmlUtil.readXML(inputStream); // 将xml解析成document结构
        Element root = document.getDocumentElement();//这代表最大的bean标签
        NodeList childNodes = root.getChildNodes(); // 每一个元素代表这个标签下的子标签
        for (int i = 0 ; i < childNodes.getLength() ; i ++){
            if (childNodes.item(i) instanceof Element){ // 子标签还是一个element
                if (BEAN_ELEMENT.equals(((Element) childNodes.item(i)).getNodeName())){ // bean标签
                    //解析bean标签
                    Element bean = (Element) childNodes.item(i);
                    String id = bean.getAttribute(ID_ATTRIBUTE);
                    String name = bean.getAttribute(NAME_ATTRIBUTE);
                    String className = bean.getAttribute(CLASS_ATTRIBUTE);

                    String initMethodName = bean.getAttribute(INIT_METHOD_ATTRIBUTE);
                    String destroyMethodName = bean.getAttribute(DESTROY_METHOD_ATTRIBUTE);
                    Class<?> clazz = null; //将名字转换为class类
                    try{
                        clazz = Class.forName(className);
                    }catch (ClassNotFoundException e){
                        throw new BeansException("Cannot find class - " + className);
                    }
                    //id的优先级大于name
                    String beanName = StrUtil.isNotEmpty(id) ? id : name;
                    if (StrUtil.isEmpty(beanName)){
                        //这种情况是 id 和 name都为空的情况
                        beanName = StrUtil.lowerFirst(clazz.getSimpleName());//取类名的第一个字母转为小写后的结果
                    }
                    BeanDefinition beanDefinition = new BeanDefinition(clazz); //创建bean定义信息
                    beanDefinition.setInitMethodName(initMethodName);
                    beanDefinition.setDestroyMethodName(destroyMethodName);

                    //填充xml中bean定义的属性
                    for (int j = 0; j < bean.getChildNodes().getLength() ;j ++){
                        if (bean.getChildNodes().item(j) instanceof Element){
                            //代表是一个property标签
                            if (PROPERTY_ELEMENT.equals(((Element) bean.getChildNodes().item(j)).getNodeName())){
                                //解析property标签
                                Element property = (Element) bean.getChildNodes().item(j);
                                String  nameAttribute = property.getAttribute(NAME_ATTRIBUTE);
                                String valueAttribute = property.getAttribute(VALUE_ATTRIBUTE);
                                String refAttribute = property.getAttribute(REF_ATTRIBUTE);
                                if (StrUtil.isEmpty(nameAttribute)){
                                    throw new BeansException("the name attribute cannot be null or empty");
                                }

                                Object value = valueAttribute;
                                if (StrUtil.isNotEmpty(refAttribute)){
                                    value = new BeanReference(refAttribute);
                                }
                                PropertyValue propertyValue = new PropertyValue(nameAttribute,value);

                                beanDefinition.getPropertyValues().addPropertyValue(propertyValue);

                            }
                        }
                    }

                    if (getRegistry().containsBeanDefinition(beanName)){
                        throw new BeansException("Duplicate beanName " + beanName + "is not allowed");
                    }

                    getRegistry().registerBeanDefinition(beanName,beanDefinition);

                }
            }
        }
    }
```

![截屏2024-07-12 18.25.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-12%2018.25.28.png)

#  bean-factory-post-processor-and-bean-post-processor

beanFactoryPostProcessor 是在创建bean之前 对beanDefinition进行修改，

通过创建BeanFactoryPostProcessor的实现类，其有修改BeanDefinition容器的功能



BeanPostProcessor是在bean实例化后进行 分为bean初始化方法前后执行befor和after方法

使用时 通过实现BeanPostProcessor的实现类 增加规则，并且要将这个实现类加入到 AbstractBeanFactory的beanPostProcessors的容器中，底层会在bean进行初始化方法前后，利用循环将beanPostProcessors中的after和before方法分别执行

# application-context

比BeanFactory更高级的IOC容器

**applicationContext支持特殊bean(beanFactoryPostProcessor,beanPostProcessor)的自动识别，资源加载，容器事件，和监听器，国际化支持，单例bean的自动初始化**

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-14%2022.53.54.png" alt="截屏2024-07-14 22.53.54" style="zoom: 25%;" />

configurableApplicationContext 继承于 ApplicationContext 接口，为了体现configurable 有refesh方法

**刷新  =  创建新的BeanFactory （**跟新维护在 AbstractRefreshableApplicationContext 中的BeanFactory - 继承自AbstractApplicationContext） 加载BeanDefinition 调用 invokeBeanFactoryPostProcessor(调用BeanFactoryPostProcessor的方法，通过getBeanOfType **通过获取类型的方式，**把容器中所有BeanFactoryPostProcessor 读取出来)

然后调用 registerBeanPostProcessor 用类型获取bean的方式，把所有的BeanPostProcessor加入到容器中



**这个config的原理就是自动在一切获取bean之前，修改好beanDefinition(beanFactoryPostProcessor) 然后**

**完善好BeanPostProcessors   敷用getBean的逻辑 （before + 初始化 + after）**

# init-and-destroy-method

![截屏2024-07-16 21.38.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-16%2021.38.44.png)

<img src="https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-16%2021.39.37.png" alt="截屏2024-07-16 21.39.37" style="zoom:25%;" />

在BeanDefinition中增加initMethodName 和 destroyMethodName 代表bean的初始化方法和销毁方法

设计DisposableBean和InitializingBean两个接口，里面有初始化和销毁方法



在xmlBeanDefinitionReader中将initMethodName和destroyMethodName信息读取到BeanDefintion中去

在getBean，创建bean加入singletonObjects中的时候，会在执行完BeanPostProcessor的前置处理后执行初始化操作，首先判断这个bean是不是InitializingBean的实现类，若是，直接调用bean对这个接口的实现

如不是且 initMethodName非空，且这个bean不是Initializating的子类或者这个initMethodName和接口中的默认方法名字不同(这两点都是为了避免自己定义的初始化方法和初始化bean接口中的方法名字一样而使得其被调用两次的情况) 

在BeanPostProcessor之后，若bean存在销毁方法 或者 bean是DisposableBean的实现类，将bean封装成DisposableBean的实现类DisposableBeanAdapter 并将这个实例对象加入到DefaultSingletonBeanRegistry维护的disposableBeans(map) 中

当执行销毁方法的时候，将map中的对象一个个取出来执行destory方法，过程和初始化类似

```java
protected void invokeInitMethod(String beanName, Object bean, BeanDefinition beanDefinition) throws Throwable{
   if (bean instanceof InitializingBean){
       ((InitializingBean)bean).afterPropertiesSet();
   }
   String initMethodName = beanDefinition.getInitMethodName();
   if (StrUtil.isNotEmpty(initMethodName) && !(bean instanceof InitializingBean && "afterPropertiesSet".equals(initMethodName))){
       Method initMethod = ClassUtil.getPublicMethod(beanDefinition.getBeanClass(),initMethodName);
       if (initMethod == null){
           throw new BeansException("Could not find an init method named " + initMethodName + "on bean with name" + beanName);
       }
       initMethod.invoke(bean);
   }
}
```



**为了确保销毁方法在虚拟机关闭之前执行，向虚拟机中注册一个钩子方法，查看AbstractApplicationContext#registerShutdownHook（非web应用需要手动调用该方法）。当然也可以手动调用ApplicationContext#close方法关闭容器。**

```java
public void registerShutdownHook(){
    Thread shutdownHook = new Thread(){
        public void run(){
            doClose();
        }
    };
    Runtime.getRuntime().addShutdownHook(shutdownHook);
}
```

# aware-interface

感知接口

ApplicationContextAware和BeanFactoryAware是Aware接口的继承接口

实现了这两个接口的bean，分别可以感知到ApplicationContext和BeanFactory的存在

**具体来说** 

**在getBean的时候，是在BeanFactory的实现类中实现的，所以getbean，bean初始化的时候就可以进行BeanFactoryAware的操作，当bean实现了这个接口，在初始化时，将容器复制到bean中**（bean实例化之后,初始化的最开始）



**在applicationContext执行的时候 向容器中注册一个用来感知容器的BeanPostProcessor 当bean实现其接口，赋值ApplicationContext容器**

![截屏2024-07-17 22.53.28](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-17%2022.53.28.png)

# prototype-bean

**对于在beanDefinition中scope为prototype的bean**

**在getBean的时候不把bean加入到singletonObjects中去，每次get都是一个新的对象**

**因为不在容器中管理，不注册销毁方法**

![截屏2024-07-18 21.50.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-18%2021.50.04.png)

# factory-bean

**factoryBean中的getbean方法可以实现更加复杂的有关bean的操作**

当一个bean为工厂bean模式

在getBean的时候，会根据其是否为单例，选择从缓存中读取(单例，第一次创建加入),或者直接重新getBean

```java
protected Object getObjectForBeanInstance(Object beanInstance,String beanName){
    Object object = beanInstance;
    if (beanInstance instanceof FactoryBean){
        FactoryBean factoryBean = (FactoryBean) beanInstance;
        try{
            if (factoryBean.isSingleton()){
                //singleton作用域bean从缓存中获取
                object = this.factoryBeanObjectCache.get(beanName);
                if (object == null){
                    object = factoryBean.getObject();
                    this.factoryBeanObjectCache.put(beanName,object);
                }
            }else{
                //prototype - 新创建bean
                object = factoryBean.getObject();
            }
        }catch (Exception e){
            throw new BeansException("FactoryBean threw exception on object " + beanName,e);
        }
    }
    return object;
 }
```

![截屏2024-07-18 22.45.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-07-18%2022.45.44.png)

直接一个名字是无法判断出接口的，所以都要产生出一个初始对象后，才开始进入方法





**允许开发者以更高度的定制性创建和管理 bean 实例，适用于需要动态创建或者复杂初始化逻辑的场景**

# event-and-event-listener

**允许应用程序在特定事件发生时进行通信**

首先spring的事件**ApplicationEvent**继承自**Java**的**EventObject**类

对于专属于**ApplicationContext**的事件有**ApplicaionContextEvent**继承自**ApplicationEvent**专门处理有关应用上下文的事件

(**Spring**容器本身也是一个事物发布者， 在生命周期的不同阶段会发布相应事件，比如开始/刷新 - **ContextRefeshedEvent** 和 销毁事件 - **ContextClosedEvent**)



事件监听器 - **ApplicationListener<?>** 泛型为要监听的**ApplicationEvent**的儿子

当监听到 泛型的对象，调用内部的**onApplicationEvent**方法



对于一整个事件的管理，监听器的管理由统一的multicaster

**ApplicationEventMulticaster** -> **AbstractApplicationEventMulticaster** -> **SimpleApplicationEventMulticaster**

其中可以增减监听器，和执行监听具体事件

```java
//监听器是否对该事件感兴趣
protected boolean supportsEvent(ApplicationListener<ApplicationEvent> applicationListener, ApplicationEvent event) {
    Type type = applicationListener.getClass().getGenericInterfaces()[0]; //获取第一个泛型接口的类型信息
    //ParameterizedType表示带有泛型参数的类型
    Type actualTypeArgument = ((ParameterizedType) type).getActualTypeArguments()[0];//第一个实际类型参数
    String className = actualTypeArgument.getTypeName();
    Class<?> eventClassName;
    try {
        eventClassName = Class.forName(className);
    }catch (ClassNotFoundException e){
        throw new BeansException("wrong event class name : " + className );
    }
    return eventClassName.isAssignableFrom(event.getClass());
}
```

发布事件 - **ApplicationEventPublisher** 发布事件 - 就是调用**SimpleApplicationEventMulticaster**的执行监听具体事件方法

```java
@Override
public void publishEvent(ApplicationEvent event) {
    applicationEventMulticaster.multicasterEvent(event);
}
```





**每次初始化容器，先会 把所有的listener由统一容器管理，每次发布一个事件，就是让这个容器中的listener去响应这个事件(监听到了，调用内部方法onApplicationEvent)，达到了监听的效果**

# pointcut-expression

**JoinPoint** - 织入点 - 切面-执行代理的某个类的某个方法

**Pointcut** - 切面表达式 ， 用来表述**joinpoint**

AspectJ 是常用的切点表达式 - **需要匹配类，定义ClassFilter接口；匹配方法，定义MethodMatcher接口。PointCut需要同时匹配类和方法，包含ClassFilter和MethodMatcher,其需要外界可以匹配，提供 获取匹配器的接口**



```java
private final PointcutExpression pointcutExpression;

private static final Set<PointcutPrimitive> SUPPORTED_PRIMITIVES = new HashSet<>();//存储切点的基本元素
static{
    SUPPORTED_PRIMITIVES.add(PointcutPrimitive.EXECUTION);//当前的切点表达式支持 execution 这一基本切点原语
}
public AspectJExpressionPointcut(String expression){
    //创建一个切点解析器 
    PointcutParser pointcutParser = PointcutParser.getPointcutParserSupportingSpecifiedPrimitivesAndUsingSpecifiedClassLoaderForResolution(SUPPORTED_PRIMITIVES,this.getClass().getClassLoader());
    pointcutExpression = pointcutParser.parsePointcutExpression(expression);
}
```

```java
/**
 * AspectJ是一种切点表达式，就是切点(继承Pointcut) 用来，描述织入点，那么其基本作用
 * 就是以aspectJ的方式去解析这一切点匹配规则(这里是简单利用execution语句)
 * 作为一个匹配原则，其暴露出去的就是方法和类的匹配器，用来让外界去比匹配切点规则
 * 实现两个接口，可以做class和method的匹配
 */
```

# jdk-dynamic-proxy

这种动态代理方式 代理的类 需要实现**接口**

**TargetSource** - 封装代理对象

**AdvisedSupport**(管理操作Adviser列表) 有 **targetSource**(目标)   **methodInteceptor**(方法拦截，定义如何执行切点增强方法)   **methodMatcher(匹配方法)**三个成员变量

将三个成员(目标的封装，方法拦截器继承的实现，**aspectJ**切点表达式中获取**matcher**)封装成**asvisedSupport**对象

通过**JdkDynamaicAopProxy**获取代理后的对象

```java
public void testJdkDynamicProxy() throws Exception{
    WorldService worldService = new WorldServiceImpl();

    AdvisedSupport advisedSupport = new AdvisedSupport();
    TargetSource targetSource = new TargetSource(worldService); // 封装代理对象
  
  //MethodInteceptor的继承 其中复写了增强后的方法的逻辑
    WorldServiceInterceptor methodInterceptor = new WorldServiceInterceptor();
    MethodMatcher methodMatcher = new AspectJExpressionPointcut("execution(* org.springframework.test.service.WorldService.explode(..))").getMethodMatcher(); //aspectJ的切点表达式
    
 	  advisedSupport.setTargetSource(targetSource);
    advisedSupport.setMethodInterceptor(methodInterceptor);
    advisedSupport.setMethodMatcher(methodMatcher);

    WorldService proxy = (WorldService) new JdkDynamicAopProxy(advisedSupport).getProxy();
    proxy.explode();
}
```

```java
public static Object newProxyInstance(ClassLoader loader, //指定一个类的加载器
                                          Class<?>[] interfaces, //代理对象的接口
                                          InvocationHandler h//用来指定生成的代理要做什么事情)
```

这是jdk动态代理的核心代码，类加载器最好选择目标代理对象的类加载器

```java
return Proxy.newProxyInstance(getClass().getClassLoader() , advised.getTargetSource().getTargetClass() , this);
```

获取代理对象的三个参数，第三个为实现InvocationHandler接口的实现类

```java
public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    if (advised.getMethodMatcher().matches(method,advised.getTargetSource().getTarget().getClass())){
        //代理方法
        MethodInterceptor methodInterceptor = advised.getMethodInterceptor();
        return methodInterceptor.invoke(new ReflectiveMethodInvocation(advised.getTargetSource().getTarget(),method,args));
    }
    return method.invoke(advised.getTargetSource().getTarget(),args);
}
```

这里重写的**invoke**方法，使用**MethodInteceptor**的原因是，这是jdk动态代理的统一格式，原本是在这个invoke中去书写aop的逻辑

但是这样就做不到每个方法的改变，所以统一去获取**advice**中的**MethodInteceptor**(方法拦截器),这样子就可以通过对advice对象方法拦截器的

方法拦截器的参数为**MethodInvocation**（方法调用）接口的实现类，这个就是用来调用函数的，所以实现类维护**object** **method**和args就可以调用函数了 封装成**ReflectiveMethodInvocation**

# cglib-dynamic-proxy

**与基于JDK的动态代理在运行期间为接口生成对象的代理对象不同，基于CGLIB的动态代理能在运行期间动态构建字节码的class文件，为类生成子类，因此被代理类不需要继承自任何接口**

# proxy-factory

代理工厂，规范jdk和cglib的动态代理，统一格式

在AdviceSupport中增加一个字段，标记使用什么模式的动态代理

获取代理对象时统一使用工厂获取

```java
public void testProxyFactory() throws Exception{
    //jdk
    advisedSupport.setProxyTargetClass(false);
    WorldService proxy = (WorldService) new ProxyFactory(advisedSupport).getProxy();
    proxy.explode();

    advisedSupport.setProxyTargetClass(true);
    proxy = (WorldService) new ProxyFactory(advisedSupport).getProxy();
    proxy.explode();
}
```

# common-advice

将spring的各种类型的Advice通过扩展MethodInterceptor来实现

创建统一的接口GenericInterceptor，将其他advice注册成成员，统一格式访问

```java
GenericInterceptor genericInterceptor = new GenericInterceptor();
genericInterceptor.setBeforeAdvice(new WorldServiceBeforeAdvice());
genericInterceptor.setAfterAdvice(new WorldServiceAfterAdvice());
genericInterceptor.setThrowsAdvice(new WorldServiceThrowsAdvice());
```

```java
public class GenericInterceptor implements MethodInterceptor {
    private BeforeAdvice beforeAdvice;
    private AfterAdvice  afterAdvice;
    private AfterReturningAdvice afterReturningAdvice;
    private ThrowsAdvice throwsAdvice;
    @Override
    public Object invoke(MethodInvocation invocation) throws Throwable {
        Object result = null;
        try{
            if (beforeAdvice != null){
                beforeAdvice.before(invocation.getMethod(),invocation.getArguments(),invocation.getThis());
            }
            result = invocation.proceed();
        }catch (Exception throwable){
            if (throwsAdvice != null){
                throwsAdvice.throwsHandle(throwable, invocation.getMethod(), invocation.getArguments(), invocation.getThis());
            }
        }finally {
            if (afterAdvice != null){
                afterAdvice.after(invocation.getMethod(), invocation.getArguments(), invocation.getThis());
            }
        }
        if (afterReturningAdvice != null){
            afterReturningAdvice.afterReturning(result, invocation.getMethod(), invocation.getArguments(), invocation.getThis());
        }
        return result;
    }

    public void setBeforeAdvice(BeforeAdvice beforeAdvice) {
        this.beforeAdvice = beforeAdvice;
    }

    public void setAfterAdvice(AfterAdvice afterAdvice) {
        this.afterAdvice = afterAdvice;
    }

    public void setAfterReturningAdvice(AfterReturningAdvice afterReturningAdvice) {
        this.afterReturningAdvice = afterReturningAdvice;
    }

    public void setThrowsAdvice(ThrowsAdvice throwsAdvice) {
        this.throwsAdvice = throwsAdvice;
    }
}
```

# pointcut-advisor

**Advisor**是包含一个**Pointcut**和一个**Advice**的组合，**Pointcut**用于捕获**JoinPoint**，**Advice**决定在**JoinPoint**执行某种操作。实现了一个支持**aspectj**表达式的**AspectJExpressionPointcutAdvisor**

**MethodInterceptor**就是**Advice**的继承 描述增强的具体方式

# auto-proxy

**BeanPostProcessor处理阶段可以修改和替换bean，正好可以在此阶段返回代理对象替换原对象。不过我们引入一种特殊的BeanPostProcessor——InstantiationAwareBeanPostProcessor，如果InstantiationAwareBeanPostProcessor处理阶段返回代理对象，会导致短路，不会继续走原来的创建bean的流程**

判断出有代理存在(遍历advisors 这个bean方法可以别匹配)

利用BeanFactoryAware的感知，获取beanDefinitnion的实例化策略，获取到加强前到bean，作为动态代理的原始对象，进行封装(TargetSource等)获取到代理对象

**代理后的对象还要参与BeanPostProcessor的后置方法调用**

# property-placeholder-configurer

在xml中定义bean的时候可以使用${}占位符，解析配置文件中的内容



解决思路是利用**BeanFactoryPostProcessor**

自定义的**ResourceLoader**解析配置文件获取输入流，通过工具类转换成**properties**对象存储配置变量



注册一个配置信息替换占位符的**BeanFactoryPostProcessor**

解析出配置文件后，遍历**BeanDifinition**替换其中的占位符(若存在)

# package-scan

扫描制定目录下的包，将有**component**注解的类变成bean

思路

在**xml**读取文件中，找到**component**-**scan**对应的目录，利用工具类 将目录下的类注册成**beanDefinition**并加入到集合当中

随后利用相似的bean产生规则，初始化参数(**Scope**) 确定**beanName**，利用**registry**将**beanDefinition**注册进**BeanDefinitionMap**中去

# value-annotation

为bean字段 填充@Value内容

首先存在StringValueResolver接口，其实现类是解决对字符串的解析问题

在PropertyPlaceholderConfigurer中会注册一个字符串处理方法，实现StringValueResolver接口，加入到abstractBeanFactory中维护的embeddedValueResolves集合中，解析字符串时也是调用这个类中的resolveEmbeddedValue方法



核心操作是创建继承于InstantiationAwareBeanPostProcessor的beanPostProcessor

在bean实例化之后设置属性之前执行 

其拦截方法获取类的带有value注解的字段，并通过 上述的字符串处理逻辑 修改类中的字符值

```java
bean = createBeanInstance(beanDefinition);

//允许BeanPostProcessor修改属性
applyBeanPostProcessorsBeforeApplyingPropertyValues(beanName,bean,beanDefinition);

//为bean填充信息
applyPropertyValues(beanName,bean,beanDefinition);
```

在实例化之后，属性填充之前，解析class，调用原来定义(从BeanProcessor集合中找)的InstantiationAwareBeanPostProcessor中的处理方法

并且这个接口的实现类会在doScan(存在包扫描进行的方法函数中)中被注册成为BeanDefinition

# autowired-annotation

自动注入注解

实现方法和@Value类似

实现简单

# populate-proxy-bean-with-property-values

**InstantiationAwareBeanPostProcessor**

其实现的**BeanPostProcessor**接口 拓展方法为bean实例化前后的两个方法

**postProcessBeforeInstantiation** 和 **postProcessAfterInstantiation**

一开始 自动代理的织入操作在**postProcessBeforeInstantiation** 在bean的实例化和填充属性之前

那么结束代理后 **bean**的属性无法填充

所以将织入操作放到**BeanPostProcessor**的**postProcessAfterInitialization**中做加强操作

(对象被代理后，仅仅增强方法，其成员变量什么的不会变的)

# type-conversion-first-part

**类型转换的第一阶段**



普通的**Converter**接口 实现S 到 T 的普通直接转换

**ConverterFactory** 转换工厂  一对多的类型转换，可以将一种类型转换为另一种类型及其子类 可以获取任意子类的转换器

先构造实现**ConverterFactory**的实现类，其中要指定好<T,S> S为大类  后调用函数获取大类子类的特有转换器

**GenericConverter** 通用转换器 

其内部类**ConvertiblePair** 维护 **sourceType**和**targetType**两个变量 是用来描述转换器可以转换的类型

并且定义转换的标准接口 **convert**



维护一个转换器的**Service**

主要有以下功能

**判断是否可以转换**

**添加转换器**

**转换**

类中维护一个**map** 管理转换器 **key**为**ConvertiblePair**(转换信息) **value** 为转换器(**GenericConverter**)

在判断两个**class**是否存在转换器的时候，尝试获取转换器，这个过程要对**sourceType**和**targetType**的**superClass**做一个双层的遍历循环，一一组合判断是否存在转换器，因为一个类继承实现某个类或接口，就可以把这个类看作是其**superClass**，因为superClass可以干的事情你都可以干，这里注意不是只对**source**和**target**封装成**ConvertiblePair**并在**map**中尝试获取



获取转换器的过程是把**source**和**target**封装成key 在 map中尝试获取 但是每个类 new 出来，肯定是不一样的

但是我们获取转换器应该只有**source**和**target**为依据，所以要重写**ConvertiblePair**的**hashCode**和**equals**方法，是的key通用型增强



对于**Converter**和**ConverterFactory** 默认对其创建两个类实现**GenericConcerter**接口，**ConverterAdapter** 和 **ConverterFactoryAdapter**

分别用其自己方式实现接口



在注册转换器的时候，分**Converter**和**ConverterFactory** 两个情况 先通过反射获取出类的ConvertiblePair 再封装成**ConverterAdapter** 和 **ConverterFactoryAdapter** 最后加入到map中去

# type-conversion-second-part

这一**branch**是把上一**branch**实现的 转换器 融入到**spring**生命周期 如各类参数从xml 或 注解 中读取 后的一个类型转换

首先在**ConfigurableBeanFactory**中增加**set**和**getConversionService**的方法

主要是在属性注入(**applyPropertyValues**) 和 解析注解内容 上 做了**object -> ?** 的类型转换 通过反射得到那个字段的**class** 作为**targetType**



这里的主要操作是 创建**ConversionServiceFactory** 这一个**FactoryBean**

他实现**InitializingBean** 可以进行初始化操作 ----- 注册**DefaultConversionService** 并向其中加载 **converters**

并在**getObject**的时候 将**ConversionService**加入到**spring**容器



在**applicationContext**的执行流程中 回去判断是否初始化好了这个**ConversionService** 若有 则会 注入到**BeanFactory** 完成正常功能



在使用这一**service**的时候 自己构建一个**converter**的**set**集合 注册一个**ConversionServiceFactory**的**bean** 将**set**注入

这里的**set**可以通过**FactoryBean<Set<?>> getObject**实现

```xml
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactory">
    <property name="converters" ref="converters"/>
</bean>

<bean id="converters" class="org.springframework.test.common.ConverterFactoryBean"/>
```

# circular-reference-without-proxy-bean

初步解决循环依赖问题

先引入二级缓存**earlySingletonObjects**

在**bean**实例化之后，判断是否单例，是则先加入到**earlySingleton**中去，获取**bean**的时候先从一级缓存**singletonObjects**中获取，如果没有的话，再尝试从二级缓存中获取，不然还是一样地去**getBean**



在循环依赖的案例中，B在实例化时会使用A在**earlySingletonObjects**中的引用完成B的构造后，A继续执行后续操作



但是如果存在代理对象，B中存放A缓存中的引用，但是在B完成构造后，**A**在**BeanPostProcessor**中执行动态代理，获取到代理对象加入到**singletonObjects**中去，这个对象的引用和在**B**中的引用不同 就乱套了 问题没有解决

# circular-reference-with-proxy-bean

使用三级依赖解决有代理对象的循环依赖问题

**首先明确原始对象和代理对象之间的关系** 

- **代理与目标对象的关系**: 在 Java 动态代理中，代理对象和目标对象是紧密关联的。代理对象不直接持有目标对象的引用，但所有方法调用都通过 `InvocationHandler` 转发到目标对象。这意味着如果目标对象的属性发生变化，代理对象将反映这些变化，因为所有对代理对象的方法调用都最终转发到目标对象。

- **代理对象的行为**: 代理对象的行为完全取决于 `InvocationHandler` 的实现。即使目标对象的状态发生变化，代理对象的行为（如何处理方法调用）仍然是由 `InvocationHandler` 控制的。因此，虽然目标对象的状态（如属性）会影响代理对象的行为，但代理对象本身并没有直接持有目标对象的状态。

  



首先第三层依赖**singletonFactories** 单例工厂 **getObject**方法是 提前返回 目标对象的代理后状态 并在**DefaultAdvisorAutoProxyCreater**中标记**beanName**，防止后续二次代理(使用Set记录)



整个**bean**创建过程中，在实例化后统一把**bean**封装成**ObjectFactory** 内置自定义的**getObject**方法返回实例化**bean**代理后结果

如果**bean**在属性注入过程中被别的**bean**提前注入了，那个**bean**会从第三级依赖中获取到注入**bean**的**ObjectFactory**，尝试提前获取代理后的代理对象(若不需要代理，方法的默认实现就是直接返回**bean**)，利用工厂获取到的bean，加入到二级缓存，并删除三级缓存(此时二级缓存中的**bean**的状态是仅仅初始化，和可能完成一部分属性注入)

之后原始**bean**继续完成其他生命周期过程，在加入到单例池前，执行一次**getSingleton**(根据三层依赖的顺序注意判断能否取到**bean**)

因为这样如果它被别的bean注入过了，提前已经把代理bean加入到二级缓存中去了(代理bean的原始代理对象就是这个bean，所以**bean**完成一系列操作，可以看成代理**bean**也经历了) + 被提前注入了，在**DefaultAdvisorAutoProxyCreater**有所记录，在其**BeanPostProcessor**中不会再来一次代理了

如果这个**bean**没有在属性注入过程中，被别的**bean**提前引用，也会有一次**getSingleton**，此时应该只存在工厂，也会返回bean

在加入到**singletonObject**后，还会统一删除一次二级和三级缓存

# lazy-init-and-multi-advice

懒加载很好实现，**beanDefinition**中加一个懒加载的字段，后面在**preInstantation**中**if**多个判断就行



重写**aop**中

之前对于一个方法实现多个增强，是在对一个方法一个增强的基础上，还是用一个**Interceptor** 但是这个**intercepter**是**GenericInterceptor** 内部逻辑是一个集成的**Interceptor** 对各种**before**的，**after**的做了提前的逻辑判断

```java
public Object invoke(MethodInvocation invocation) throws Throwable {
    Object result = null;
    try{
        if (anotherBeforeAdvice != null){
            anotherBeforeAdvice.before(invocation.getMethod(),invocation.getArguments(),invocation.getThis());
        }
        result = invocation.proceed();
    }catch (Exception throwable){
        if (throwsAdvice != null){
            throwsAdvice.throwsHandle(throwable, invocation.getMethod(), invocation.getArguments(), invocation.getThis());
        }
    }finally {
        if (anotherAfterAdvice != null){
            anotherAfterAdvice.after(invocation.getMethod(), invocation.getArguments(), invocation.getThis());
        }
    }
    if (anotherAfterReturningAdvice != null){
        anotherAfterReturningAdvice.afterReturning(result, invocation.getMethod(), invocation.getArguments(), invocation.getThis());
    }
    return result;
}
```

这么写解决了大部分的多**advice**的问题，但是如果一个**before**的**advice**有2个，就无法解决了，因为**GenericInterceptor**的逻辑被写死了



这里开始 使用拦截器链，在**AdviceSupport**中增加一个**advisors**的**list**，记录这个切点的所有**advice** （所以可以取消**MethodInterceptor**）

**method**去匹配拦截器链   所以在**AdviceSupport** 根据**method**的**hashcode**去设置一个缓存，记录不同**method**的拦截器链情况

(根据类去匹配，一个**AdviceSupport**有很多**advice**，对每个**method**的再做细分，**advisors**列表由**DefaultAdvisorChainFactory**的**get**方法把总体的**advisors**变为单个方法的拦截器链)

**jdk**和**Cglib** 做动态代理的时候，是会去获取**method**的拦截器链，以这个为依据判断是否需要做代理

**ProxyFactory**中根据对象是否存在接口来决定使用**jdk**还是**Cglib**做代理（继承自**AdvisorSupport**）

**DefaultAdvisorsAutoProxyCreater** 工作时 ，对于每个**class**#**method** 遍历**AspectJExpressionPointcutAdvisor**把符合要求的**advisor**加入到**proxyFactory**(**AdvisorSupport**)的**advisors**中去

主要实现这个拦截器链调用的是**ReflectMethodinvocation**中的**proceed**方法

```java
public Object proceed() throws Throwable {
    //初始化index为-1 每调用一次proceed index就+1
    if (this.currentInterceptorIndex == this.interceptorsAndDynamicMethodMatchers.size() - 1){
        //调用次数 = 拦截器个数
        //触发当前method方法
        return method.invoke(this.target,this.arguments);
    }
    Object o = this.interceptorsAndDynamicMethodMatchers.get(++this.currentInterceptorIndex);
    return ((MethodInterceptor) o).invoke(this);//循环调用拦截器
}
```

**interceptors**中的元素循环调用**invoke**又会回到 这里的**proceed**   **实现循环![截屏2024-08-10 23.04.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-08-10%2023.04.39.png)**

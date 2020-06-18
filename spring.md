## Spring

1. IOC & AOP  
    - IOC: 将原本在程序中手动创建对象的控制权，交由Spring框架来管理。当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的
    - AOP: **代码增强**，为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，**降低耦合度**
        - 动态代理
            - JDK Proxy 实现某个接口
            - Cglib 没有接口 继承子类
2. Spring AOP 和 AspectJ AOP 有什么区别
    - Spring AOP 属于运行时增强，而 AspectJ 是编译时增强
    - AspectJ 功能更强大，切面很多时使用
3. Spring生命周期
    - Bean 容器找到配置文件中 Spring Bean 的定义。
    - Bean 容器利用 Java Reflection API 创建一个Bean的实例。
    - 如果涉及到一些属性值 利用 set()方法设置一些属性值。
    - 如果 Bean 实现了 BeanNameAware 接口，调用 setBeanName()方法，传入Bean的名字。
    - 如果 Bean 实现了 BeanClassLoaderAware 接口，调用 setBeanClassLoader()方法，传入 ClassLoader对象的实例。
    - 与上面的类似，如果实现了其他 *.Aware接口，就调用相应的方法。
    - 如果有和加载这个 Bean 的 Spring 容器相关的 BeanPostProcessor 对象，执行postProcessBeforeInitialization() 方法
    - 如果Bean实现了InitializingBean接口，执行afterPropertiesSet()方法。
    - 如果 Bean 在配置文件中的定义包含 init-method 属性，执行指定的方法。
    - 如果有和加载这个 Bean的 Spring 容器相关的 BeanPostProcessor 对象，执行postProcessAfterInitialization() 方法
    - 当要销毁 Bean 的时候，如果 Bean 实现了 DisposableBean 接口，执行 destroy() 方法。
    - 当要销毁 Bean 的时候，如果 Bean 
    在配置文件中的定义包含 destroy-method 属性，执行指定的方法。
    <img src="./image/spring-lifecycle.jpg" height="300" width="800"/>
4. SpringMVC 工作原理
    1. 客户端（浏览器）发送请求，直接请求到 DispatcherServlet。
    2. DispatcherServlet 根据请求信息调用 HandlerMapping，解析请求对应的 Handler。
    3. 解析到对应的 Handler（也就是我们平常说的 Controller 控制器）后，开始由 HandlerAdapter 适配器处理。
    4. HandlerAdapter 会根据 Handler 来调用真正的处理器开处理请求，并处理相应的业务逻辑。
    5. 处理器处理完业务后，会返回一个 ModelAndView 对象，Model 是返回的数据对象，View 是个逻辑上的 View。
    6. ViewResolver 会根据逻辑 View 查找实际的 View。
    7. DispaterServlet 把返回的 Model 传给 View（视图渲染）。
    把 View 返回给请求者（浏览器）
    <img src="./image/spring-mvc-life.jpg" height="300" width="800"/>
5. Spring设计模式：  

    - 工厂设计模式 : Spring使用工厂模式通过 BeanFactory、ApplicationContext 创建 bean 对象。
    - 代理设计模式 : Spring AOP 功能的实现。
    - 单例设计模式 : Spring 中的 Bean 默认都是单例的。
    - 模板方法模式 : Spring 中 jdbcTemplate、hibernateTemplate 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
    - 包装器设计模式 : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
    - 观察者模式: Spring 事件驱动模型就是观察者模式很经典的一个应用。
    - 适配器模式 :Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配Controller

6. Spring事务传播  

    支持当前事务的情况：

        - TransactionDefinition.PROPAGATION_REQUIRED： 如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。
        - TransactionDefinition.PROPAGATION_SUPPORTS： 如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
        - TransactionDefinition.PROPAGATION_MANDATORY： 如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。（mandatory：强制性）

    不支持当前事务的情况：

        - TransactionDefinition.PROPAGATION_REQUIRES_NEW： 创建一个新的事务，如果当前存在事务，则把当前事务挂起。
        - TransactionDefinition.PROPAGATION_NOT_SUPPORTED： 以非事务方式运行，如果当前存在事务，则把当前事务挂起。
        - TransactionDefinition.PROPAGATION_NEVER： 以非事务方式运行，如果当前存在事务，则抛出异常。
    其他情况：

        - TransactionDefinition.PROPAGATION_NESTED： 如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于TransactionDefinition.PROPAGATION_REQUIRED
<<<<<<< Updated upstream
    
7. BeanFactory & FactoryBean
    - BeanFactory是一个接口，它是Spring中工厂的顶层规范，是SpringIoc容器的核心接口
    具体实现DefaultListableBeanFactory, XmlBeanFactory, XmlBeanFactory
    - FactoryBean是一个Bean，但又不仅仅是一个Bean。它是一个能生产或修饰对象生成的工厂Bean，类似于设计模式中的工厂模式和装饰器模式
        - 即一个Bean A如果实现了FactoryBean接口，那么A就变成了一个工厂，根据A的名称获取到的实际上是工厂调用getObject()返回的对象，而不是A本身，如果要获取工厂A自身的实例，那么需要在名称前面加上'&'符号。
        - getObject('name')返回工厂中的实例
        - getObject('&name')返回工厂本身的实例

8. BeanFactoryPostProcessor & BeanPostProcessor
    - BeanFactoryPostProcessor 是与 BeanDefinition 打交道的，如果想要与 Bean 打交道，请使用 BeanPostProcessor
    - BeanFactoryPostProcessor 同样支持排序，一个容器可以同时拥有多个 BeanFactoryPostProcessor ，这个时候如果我们比较在乎他们的顺序的话，可以实现 Ordered 接口

9. ApplicationContext
    - 会自动识别配置文件中的 BeanFactoryPostProcessor 并且完成注册和调用，我们只需要简单的配置声明
    - 也可以自动识别BeanPostProcessor
    - 都有方法定义再refresh()中
    - ApplicationContext使用即时加载，BeanFactory使用懒加载

10. Spring事件驱动模型
    - 三个概念：事件、事件监听器、事件发布者
    - @EventListener注解代替实现ApplicationListener接口,
    - 事件：HomeWorkEvent extends ApplicationEvent
    - 事件监听者StudentListener implements ApplicationListener<HomeWorkEvent>
    - 事件发布者
        ```java

        @Component
        public class TeacherPublisher {
            @Autowired
            private ApplicationContext applicationContext;
            
            public void publishHomeWork(String content){
                HomeWorkEvent homeWorkEvent = new HomeWorkEvent(this, content);
                applicationContext.publishEvent(homeWorkEvent);
            }
        }
        ```
11. Spring Boot 全局异常处理
    - ControllerAdvice 开启了全局异常的捕获    
    ```java
    @ControllerAdvice
    public class MyExceptionHandler {

        @ExceptionHandler(value =Exception.class)
        public String exceptionHandler(Exception e){
            System.out.println("未知异常！原因是:"+e);
            return e.getMessage();
        }
    }
    ```
=======
7. Spring循环依赖问题
    - 构造器的循环依赖。【这个Spring解决不了】
    - setter循环依赖 field属性的循环依赖【setter方式 单例，默认方式-->通过递归方法找出当前Bean所依赖的Bean，然后提前缓存【会放入Cach中】起来。通过提前暴露 -->暴露一个exposedObject用于返回提前暴露的Bean
8. Spring Bean的实例化过程
9. BeanFactory & FactoryBean
10. Spring中不同的事件监听器/观察者模式
11. BeanFactory & ApplicationContext
>>>>>>> Stashed changes

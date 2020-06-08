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
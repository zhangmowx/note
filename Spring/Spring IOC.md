# IoC 容器
  IOC 容器具有依赖注入功能的容器，它可以创建对象，IOC 容器负责实例化、定位、配置应用程序中的对象及建立这些对象间的依赖。
  通常new一个实例，控制权由程序员控制，而"控制反转"是指new实例工作不由程序员来做而是交给Spring容器来做。
  在Spring中BeanFactory是IOC容器的实际代表者。

## Spring 提供了以下两种不同类型的容器
 1.	Spring BeanFactory 容器
    它是最简单的容器，给 DI 提供了基本的支持，它用 org.springframework.beans.factory.BeanFactory 接口来定义。BeanFactory 或者相关的接口，
    如 BeanFactoryAware，InitializingBean，DisposableBean，在 Spring 中仍然存在具有大量的与 Spring 整合的第三方框架的反向兼容性的目的。
 2. Spring ApplicationContext 容器
    该容器添加了更多的企业特定的功能，例如从一个属性文件中解析文本信息的能力，发布应用程序事件给感兴趣的事件监听器的能力。
    该容器是由 org.springframework.context.ApplicationContext 接口定义。
 总结：
  ApplicationContext容器包括BeanFactory容器的所有功能，所以通常建议使用ApplicationContext。
  但是BeanFactory仍然可以用于轻量级的应用程序，如移动设备或基于applet的应用程序，其中它的数据量和速度是显著。

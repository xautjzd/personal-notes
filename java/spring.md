# Spring

容器初始化流程

- AbstractApplicationContext
- SpringBootServletInitializer(类似web.xml)
- ServletContainerInitializer(实现类: ServletContainerInitializer): 加载SpringBootServletInitializer
- DispatcherServlet
- ClassPathBeanDefinitionScanner(@Service, @Controller 等注解扫描)

## Spring 扩展点

### BeanDefinition

- `ContextNamespaceHandler`

### BeanFactoryPostProcessor

场景: 从配置中心获取配置，注入Bean

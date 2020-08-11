# 网关

常用网关:

- Kong
- Spring Cloud Gateway


## Spring Cloud Gateway

网关作为流量入口，进行流量转发，核心是路由 & 路由规则配置，要保证高可用，尽量避免重启，为此需要实现动态路由配置。

动态路由涉及以下几个要点:

- 网关启动时，存量路由数据加载
- 启动后，监听动态路由配置，进行热加载

Spring Cloud Gateway 路由相关核心类:

- PropertiesRouteDefinitionLocator: 从配置文件(yaml、properties)加载路由配置
- RouteDefinitionRepository: 读取 & 写入路由配置
- DiscoveryClientRouteDefinitionLocator: 从注册中心中读取路由信息(如Nacos、Eurka、Zookeeper等)
- RouteDefinitionRouteLocator: 将 RouteDefinition 转化为 Route

> 路由变化只需要往 ApplicationEventPublisher 发送 RefreshRoutesEvent 事件，gateway 会自动监听该事件并调用 RouteDefinitionRepository->getRouteDefinitions() 更新路由信息

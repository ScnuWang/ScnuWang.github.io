---
layout: post
title: Spring Cloud Gateway官方文档笔记(一)
category: Spring Cloud Gateway
tags: [Spring Cloud Gateway, 网关]
---

[官方文档](https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.1.0.RELEASE/single/spring-cloud-gateway.html)

官方文档的内容不算很多，花点时间应该很多可以看完。

### 一、引入依赖

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```

如果引入了包，但是又不想让Gateway生效，可以通过在application.properties中添加：`spring.cloud.gateway.enabled=false`来禁用。

**注意**：`Spring Cloud Gateway`需要`Spring Boot`和`Spring Webflux`提供的Netty运行时。它不能在传统的Servlet容器中工作或构建为WAR。

### 二、关键词

**Route**（路由）：Route是网关的基础。根据路由编号，目标地址（请求转发到哪里去），一系列的断言（Predicate）和一系列的过滤器（Filter）来最终确定一个路由。如果一系列的断言都为真，那么路由就匹配了。

- [ ] **疑问：过滤器不用考虑吗？**

**Predicate**（断言）：这个是Java8 的一个新类。据我现在了解的，它的功能如下：

第一：根据输入判断是否与既定的规则匹配，要是匹配就返回True,否则返回false。

第二：对输入的规则进行操作：合并规则、规则取非、规则取并。

第三：根据规则判断两个参数是否一致。

以上表述应该不够准确，后续再来修改。

**Filter**（过滤器）：这里的过滤器都是指`Spring Framework GatewayFilter`的实例，可以在发送下游请求之前或之后修改请求和响应。

### 三、工作流程

![1560700380417](https://scnuWang.github.io/assets/images/1560700380417.png)

`Gateway Handler Mapping`根据Predicate确定要转发到哪里去，`Gateway Web Handler`让请求通过过滤器链，根据执行结果判断是否要继续下发，已经对下发之前的请求和下发之后的响应进行处理。我个人觉得到了这一步，一句话概括就是**再一次确认要不要转发，怎么转发，转发后怎么处理响应。**

**注意**：在没有端口的路由中定义的URI将分别为HTTP和HTTPS URI获取默认端口设置为80和443。

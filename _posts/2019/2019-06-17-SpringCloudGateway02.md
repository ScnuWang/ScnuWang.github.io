---
layout: post
title: Spring Cloud Gateway官方文档笔记(二)
category: Spring Cloud
tags: [Spring Cloud Gateway, 网关]
---

继续上一篇：本篇笔记主要是Predicate相关的内容

### 四、路由断言工厂

`Spring Cloud Gateway`将路由作为`Spring WebFlux HandlerMapping`基础结构的一部分进行匹配。 `Spring Cloud Gateway`包含许多内置的`Route Predicate Factory`。所有这些谓Predication都匹配HTTP请求的不同属性。多个Precation工厂可以组合并通过逻辑`and`组合。

#### 1. After Route Predicate Factory

可以构建在某个指定时间之后生效的路由。

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: after_route
        uri: http://example.org
        predicates:
        - After=2017-01-20T17:42:47.789-07:00[America/Denver]
```

#### 2. Before Route Predicate Factory

和上面的相反，可以构建在某个指定时间之前有效的路由。

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: before_route
        uri: http://example.org
        predicates:
        - Before=2017-01-20T17:42:47.789-07:00[America/Denver]
```

#### 3. Between Route Predicate Factory

可以构建在某个指定的时间段内的路由

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: between_route
        uri: http://example.org
        predicates:
        - Between=2017-01-20T17:42:47.789-07:00[America/Denver], 2017-01-21T17:42:47.789-07:00[America/Denver]
```

#### 4. Cookie Route Predicate Factory

根据Cookie里面是否包含指定的`Key--Value`构建路由，支持正则表达式

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: cookie_route
        uri: http://example.org
        predicates:
        - Cookie=chocolate, ch.p
```

#### 5. Header Route Predicate Factory

根据请求头里是否包含指定的`Key--Value`构建路由，支持正则表达式

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: header_route
        uri: http://example.org
        predicates:
        - Header=X-Request-Id, \d+
```

#### 6. Host Route Predicate Factory

根据请求头里的Host构建路由，该模式是一种Ant样式模式`.`作为分隔符，支持正则表达式，也支持URI模板变量，例如{sub} .myhost.org。 如果请求的主机头具有值`www.somehost.org`或`beta.somehost.org`或`www.anotherhost.org`，则此路由将匹配。 此Predicate将URI模板变量（如上例中定义的sub）提取为名称和值的映射，并将其放在`ServerWebExchange.getAttributes（）`中，并在`ServerWebExchangeUtils.URI_TEMPLATE_VARIABLES_ATTRIBUTE`中定义一个键。然后，这些值可供`GatewayFilter Factories`使用。

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: host_route
        uri: http://example.org
        predicates:
        - Host=**.somehost.org,**.anotherhost.org
```

#### 7. Method Route Predicate Factory

根据请求方法构建路由。

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: method_route
        uri: http://example.org
        predicates:
        - Method=GET
```

#### 8. Path Route Predicate Factory

根据请求路径构建路由。路径里面可以取模板变量，其他的工厂也可以使用。

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: host_route
        uri: http://example.org
        predicates:
        - Path=/foo/{segment},/bar/{segment}
```

此谓词将URI模板变量（如上例中定义的`segment`）提取为name和value的map，并将其放在`ServerWebExchange.getAttributes（）``中，并在ServerWebExchangeUtils.URI_TEMPLATE_VARIABLES_ATTRIBUTE中定义一个键。然后，这些值可供`GatewayFilter Factories`使用.

#### 9. Query Route Predicate Factory

如果配置一个参数，则可以构建根据请求体里面包含某个参数的路由；如果有两个参数则认为是一个键值对，即可以构建请求体中包含某个键值对的路由。

示例：请求中包含参数`baz`

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: query_route
        uri: http://example.org
        predicates:
        - Query=baz
```

示例：请求体中包含`foo`的值满足正则表达式`ba.`

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: query_route
        uri: http://example.org
        predicates:
        - Query=foo, ba.
```

#### 10. RemoteAddr Route Predicate Factory

采用CIDR表示法（IPv4或IPv6）字符串的列表（最小值为1），例如， `192.168.0.1/16`（其中`192.168.0.1`是IP地址，16是子网掩码）。

```yml
spring:
  cloud:
    gateway:
      routes:
      - id: remoteaddr_route
        uri: http://example.org
        predicates:
        - RemoteAddr=192.168.1.1/24
```

**注意**：默认情况下，`RemoteAddr Route Predicate Factory`使用传入请求中的远程地址。如果Spring Cloud Gateway位于代理层后面，则可能与实际客户端IP地址不匹配。

您可以通过设置自定义`RemoteAddressResolver`来自定义解析远程地址的方式。 `Spring Cloud Gateway`附带一个非默认远程地址解析器，它基于`X-Forwarded-For`标头`XForwardedRemoteAddressResolver`。

`XForwardedRemoteAddressResolver`有两个静态构造函数方法，它们采用不同的安全方法：

`XForwardedRemoteAddressResolver :: trustAll`返回一个`RemoteAddressResolver`，它始终采用`X-Forwarded-For`标头中找到的第一个IP地址。这种方法容易受到欺骗，因为恶意客户端可以初始化一个可以解析器接受的`X-Forwarded-For`的值。

`XForwardedRemoteAddressResolver :: maxTrustedIndex`采用与`Spring Cloud Gateway`前运行的可信基础架构数相关的索引。例如，如果只能通过HAProxy访问`Spring Cloud Gateway`，则应使用值1。如果在可访问`Spring Cloud Gateway`之前需要两跳可信基础架构，则应使用值2。

示例：

请求头的方式：`X-Forwarded-For: 0.0.0.1, 0.0.0.2, 0.0.0.3`

下面的maxTrustedIndex值将产生以下远程地址。

| `maxTrustedIndex`        | result                                                      |
| ------------------------ | ----------------------------------------------------------- |
| [`Integer.MIN_VALUE`,0]  | (invalid, `IllegalArgumentException` during initialization) |
| 1                        | 0.0.0.3                                                     |
| 2                        | 0.0.0.2                                                     |
| 3                        | 0.0.0.1                                                     |
| [4, `Integer.MAX_VALUE`] | 0.0.0.1                                                     |

使用配置类的方式：

```
RemoteAddressResolver resolver = XForwardedRemoteAddressResolver
    .maxTrustedIndex(1);

...

.route("direct-route",
    r -> r.remoteAddr("10.1.1.1", "10.10.1.1/24")
        .uri("https://downstream1")
.route("proxied-route",
    r -> r.remoteAddr(resolver,  "10.10.1.1", "10.10.1.1/24")
        .uri("https://downstream2")
)
```


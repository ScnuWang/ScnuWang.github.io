---
layout: post
title: GatewayFlowRule类解析
category: Spring Cloud Gateway
tags: [Spring Cloud Gateway, 网关]
---

### 字段属性

```java
// 资源名，即限流规则的作用对象
private String resource;
// 默认值为路由Id，也可以是自定义API的名称
private int resourceMode = SentinelGatewayConstants.RESOURCE_MODE_ROUTE_ID;
// 限流阈值类型（QPS或者并非线程数）,默认为QPS
private int grade = RuleConstant.FLOW_GRADE_QPS;
// 限流阈值
private double count;
// 统计时间窗口，单位是秒，默认是 1 秒。
private long intervalSec = 1;
// 流量控制模式，默认为直接拒绝，还有Warm Up(预热/冷启动方式)、匀速器模式
private int controlBehavior = RuleConstant.CONTROL_BEHAVIOR_DEFAULT;
// 应对突发请求时额外允许的请求数目。
private int burst;
// 匀速排队模式下的最长排队时间，单位是毫秒，仅在匀速排队模式下生效。
private int maxQueueingTimeoutMs = 500;
// 用于参数流量控制。如果未设置，网关规则将转换为正常流规则。
private GatewayParamFlowItem paramItem;
```

### 构造方法

包含一个无参构造方法和一个包含资源名称的构造方法。

### 普通方法

只有getter,setter,equal,toString等

### 综上

一个普通的类，主要是封装限流的属性，方便使用。

网关默认限流规则：默认是对以路由Id作为资源名称,通过统计QPS的阈值，采取直接拒绝的方式进行限流。

**注意**：如果需要对参数进行限流的时候，才为GatewayParamFlowItem赋值。
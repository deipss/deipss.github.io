---
layout: default
title: Java Sandbox
parent: Sandbox
nav_order: 2
---

# 包结构

## sandbox-agent

- SandboxClassLoader 类加载器，构造函数中声明了sandboxCoreJarFilePath这个核心jar包的文件
- AgentLauncher 代理的启用类

## sandbox-api

- 注解：命令注解、BootstrapClassLoader类加载注解、包含子类注解、不使用sandbox的加载类加载的注解
- 事件定义：定义了7件事件如，调用之前，调用之后，异常之后……
- 过滤器：对加载某些类时，是否匹配，是否使用BootstrapClassLoader类加载，是否包含子类 等，进行逻辑过滤
- 监听器：
    - 行为通知 Advice Attachment
    - 事件监听器 EventListener AdviceAdapterListener AdviceListener
- 资源
    - LoadedClassDataSource 已加载类数据源
    - ModuleController 模型控制接口
    - ModuleManager 模块管理
    - ModuleEventWatcher 事件观察
- SPI onJarUnLoadCompleted
- ModuleLifecycle 沙箱模块的生命周期 ModuleLifecycleAdapter
- ProcessController 流程控制 ProcessControlException

## sandbox-common-api

- 沙箱的配置 ConfigInfo 如启用方式是Agent 还是 Attach

## sandbox-core
- classloader 类加载器




## sandbox-debug-module
- 调用模块，定义了许多module

## sandbox-mgr-module

- ControlModule 沙箱控制模块
- InfoModule 沙箱信息模块
- ModuleMgrModule 沙箱管理模块 list flush reset unload frozen
- OnJarUnLoadCompleted SPI实现

## sandbox-mgr-provider

- 对ModuleLoadingChain 和 ModuleJarLoadingChain 的空实现

## sandbox-provider-api

- 模块加载链 ModuleLoadingChain
- 模块jar包文件加载链 ModuleJarLoadingChain

## sandbox-spy

- 流程扭转中间类 SpyHandler Spy

# 参考文献

- https://www.baeldung.com/java-classloaders 类加载器
- https://tech.hipac.cn/archives/aeb6e3616cf74e1984b908fc1cd98913#jacoco 多agent治理在海拍客的应用与实践
- https://arthas.aliyun.com/doc/agent.html 通常 Arthas 是以动态 attach 的方式来诊断应用，但从3.2.0版本起，Arthas 支持直接以
  java agent 的方式启动。
- https://www.cnblogs.com/rickiyang/p/11368932.html agent代码精解
- https://juejin.cn/post/7018237356532563999 代码示例
- https://developer.aliyun.com/article/854428 代码示例
- https://zhuanlan.zhihu.com/p/448871215
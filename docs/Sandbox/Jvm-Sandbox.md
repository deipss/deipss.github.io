---
layout: default
title: Java Sandbox
parent: Sandbox
nav_order: 2
---

# 包结构

## sandbox-agent

### SandboxClassLoader 类加载器，

构造函数中声明了sandboxCoreJarFilePath这个核心jar包的文件

### AgentLauncher 代理的启用类

使用两种方式来启动agent，**LAUNCH_MODE_AGENT（启动加载）**，**LAUNCH_MODE_ATTACH（动态加载）**

- 会不会两个命令同时写一个文件，产生竞争，发生ABA？

```text
private static synchronized InetSocketAddress install(final Map<String, String> featureMap, final Instrumentation inst)
premain和attach两种方式都会调用这个方法，这个方法上的static锁。

```

- 对同一个类文件增强，会有先后顺序的影响吗？会同时存在吗？

#### premain 方式

- premain方式启动时，loadOrDefineClassLoader()会使用SandboxClassLoader来加载sandbox-core这个模块的jar文件，做到与工程代码的类加载器隔离
- inst.appendToBootstrapClassLoaderSearch(new JarFile(new File(getSandboxSpyJarPath(home)  将Spy注入到BootstrapClassLoader

#### attach

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

### asm
- CodeLock 代码锁

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

```shell
Spy类的方法，其实是ASM增强后，调用的

public static void spyMethodOnCallThrows(final String throwException,
                                             final String namespace,
                                             final int listenerId) throws Throwable {
        try {
            final SpyHandler spyHandler = namespaceSpyHandlerMap.get(namespace);
            if (null != spyHandler) {
                spyHandler.handleOnCallThrows(listenerId, throwException);
            }
        } catch (Throwable cause) {
            handleException(cause);
        }
    }

    /**
     * asm method of {@link Spy#spyMethodOnCallThrows(String, String, int)}
     */
    Method ASM_METHOD_Spy$spyMethodOnCallThrows = getAsmMethod(
            Spy.class,
            "spyMethodOnCallThrows",
            String.class, String.class, int.class
    );
```


# 源代码细节

## 如何进行类隔离

- 对于同样的类，是不是会加载多次，比如LogFactory

## 同一个类被多个模块增强，字节码会是怎么样


## 同一个类被多个模块同步增强，是否会出现ABA问题，如何应对这类问题



# 参考文献

- https://www.baeldung.com/java-classloaders 类加载器
- https://tech.hipac.cn/archives/aeb6e3616cf74e1984b908fc1cd98913#jacoco 多agent治理在海拍客的应用与实践
- https://arthas.aliyun.com/doc/agent.html 通常 Arthas 是以动态 attach 的方式来诊断应用，但从3.2.0版本起，Arthas 支持直接以
  java agent 的方式启动。
- https://www.cnblogs.com/rickiyang/p/11368932.html agent代码精解
- https://juejin.cn/post/7018237356532563999 代码示例
- https://developer.aliyun.com/article/854428 代码示例
- https://zhuanlan.zhihu.com/p/448871215
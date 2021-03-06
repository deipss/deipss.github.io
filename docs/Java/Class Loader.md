---
layout: default
title: Class Loader
parent: Java
nav_order: 2
---

# 1. 类加载器

从java.lang.ClassLoader类的源代码中的注释可以知悉以下几个要点：

- 给出类的二进制名，类加载器可以加载、生成类的定义
- 第个类都一个加载它的类加载器
- 数组类不是由类加载器创建，它的类加载器和它的元素类是同一个
- 类加载通过双亲委派机制来搜索类和资源
- 每个类加载器都有对应的父类（也是一个类加载器）
- JVM中有一个 bootstrap class loader，是所有器加载器的父类
- 可以通过ClassLoader.registerAsParallelCapable来并行加载
- 双亲委派不是强制的，所以类加载需要具备并行能力，否则会出现死锁

## 1.1. 主要类加载器

### 1.1.1. Bootstrap Class Loader

主要用来加载 JDK 内部的核心类库（ %JAVA_HOME%/lib目录下的 rt.jar、resources.jar、charsets.jar等 jar 包和类）以及被
-Xbootclasspath参数指定的路径下的所有类

### 1.1.2. Extension Class Loader

加载 %JRE_HOME%/lib/ext 目录下的 jar 包和类以及被 java.ext.dirs 系统变量所指定的路径下的所有类。

### 1.1.3. System Class loader

面向我们用户的加载器，负责加载当前应用 classpath 下的所有 jar 包和类。

## 1.2. 双亲委派机制

```text
protected Class<?> loadClass(String name, boolean resolve)
        throws ClassNotFoundException
    {
    // 上锁
        synchronized (getClassLoadingLock(name)) {
    // 先查询一次
            Class<?> c = findLoadedClass(name);
            if (c == null) {
                long t0 = System.nanoTime();
                try {
                    if (parent != null) {
                        // 由父类去查找
                        c = parent.loadClass(name, false);
                    } else {
                        // 使用bootstrap去查询
                        c = findBootstrapClassOrNull(name);
                    }
                } catch (ClassNotFoundException e) {
                    // ClassNotFoundException thrown if class not found
                    // from the non-null parent class loader
                }

                if (c == null) {
                    // 使用父类和bootstrap还是没有找到，使用findClass去查找
                    long t1 = System.nanoTime();
                    c = findClass(name);

                    //记录类加载的信息
                    sun.misc.PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                    sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                    sun.misc.PerfCounter.getFindClasses().increment();
                }
            }
            if (resolve) {
                resolveClass(c);
            }
            return c;
        }
    }
```

## 1.3. 指定类加载器加载哪些类

以jvm-sandbox-repeater自定义的类加载器为例，

```text
    public JarFileLifeCycleManager(String jarFilePath, PluginClassLoader.Routing ... routingArray) {
        File file = new File(jarFilePath);
        if (!file.exists()) {
            throw new IllegalArgumentException("jar file does not exist, path=" + jarFilePath);
        }
        final URL[] urLs = getURLs(jarFilePath);
        if (urLs.length == 0) {
            throw new IllegalArgumentException("does not have any available jar in path:" + jarFilePath);
        }
        // 在new一个类加载器时，传递了具体多个jar包的URL
        this.classLoader = new PluginClassLoader(getURLs(jarFilePath), this.getClass().getClassLoader(), routingArray);
    }
```

在RepeaterModule中调用上面的方法

```text
ApplicationModel.instance().setConfig(config);
 // 特殊路由表;
 PluginClassLoader.Routing[] routingArray = PluginClassRouting.wellKnownRouting(configInfo.getMode() == Mode.AGENT, 20L);
 String pluginsPath;
 if (StringUtils.isEmpty(config.getPluginsPath())) {
     pluginsPath = PathUtils.getPluginPath();
 } else {
     pluginsPath = config.getPluginsPath();
 }
 lifecycleManager = new JarFileLifeCycleManager(pluginsPath, routingArray);
 //
```

## 1.4. 如何加载一个类

从上面的逻辑可以看到，加载的方使用loadClass(),但过程先是使用父类加载，没有加载到，再使用findClass()去查找，所以加载一个类，

- 如果要遵从委派机制，可以只重写findClass()方法，
- 如果是要破坏委派机制，是直接重写loadClass()方法。
  使用以下方法进行加载

```text
    @Override
    protected synchronized Class<?> loadClass(String name, boolean resolve) 
```

## 1.5. 判定一个类是否被加载

使用findLoadedClass方法

```text
final Class<?> loadedClass = findLoadedClass(name);
```

## 1.6. 等待一个类加载完成

参加sandbox-repeater的实现，

```text
while (isPreloading && --timeout > 0 && ClassloaderBridge.instance().findClassInstances(routing.targetClass).size() == 0) {
    try {
        Thread.sleep(100);
        LogUtil.info("{} required {} class router,waiting for class loading", routing.identity, routing.targetClass);
    } catch (InterruptedException e) {
        // ignore
    }
}
```

## 1.7. 参考文献

- https://www.baeldung.com/java-classloaders
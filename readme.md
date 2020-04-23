这是一个android 方法耗时统计打印插件，可以根据包名，类名，方法正则表达式等，指定插入代码进行统计，在做性能调优的时候非常方便。能清晰的观察每个方法的耗时情况。另外还可以自定义统计实现类，增加额外的内容。

[![License](https://img.shields.io/badge/license-Apache%202-green.svg)](https://www.apache.org/licenses/LICENSE-2.0)[![Download](https://api.bintray.com/packages/miqingtang/maven/pluginSrc/images/download.svg)](https://bintray.com/miqingtang/maven/pluginSrc)

## 效果展示：

统计项目中所有activity的方法耗时情况：


```
2020-04-23 17:38:47.178 26336-26336/? D/TimePrint:  
    ╔======================================================================================
    ║[Thread]:main
    ║[This]:com.miqt.plugindemo.MainActivity@6f59be3
    ║[Class]:com.miqt.plugindemo.MainActivity
    ║[Method]:onCreate
    ║[Return]:void
    ║[ArgsType]:[android.os.Bundle]
    ║[ArgsValue]:[null]
    ║[Time]:80 ms
    ╚======================================================================================
2020-04-23 17:38:54.899 26336-26384/com.miqt.plugindemo E/TimePrint:  
    ╔======================================================================================
    ║[Thread]:Thread-3
    ║[This]:null
    ║[Class]:com.miqt.plugindemo.Hello
    ║[Method]:getStr
    ║[Return]:java.lang.String
    ║[ArgsType]:[java.lang.String]
    ║[ArgsValue]:[hello word!]
    ║[Time]:1001 ms
    ╚======================================================================================
2020-04-23 17:38:54.900 26336-26384/com.miqt.plugindemo E/TimePrint:  
    ╔======================================================================================
    ║[Thread]:Thread-3
    ║[This]:com.miqt.plugindemo.MainActivity@6f59be3
    ║[Class]:com.miqt.plugindemo.MainActivity
    ║[Method]:mmm
    ║[Return]:void
    ║[ArgsType]:[]
    ║[ArgsValue]:[]
    ║[Time]:4003 ms
    ╚======================================================================================
```

可以看出，这样的话方法名，运行线程，和耗时情况就都一目了然啦。

也支持注解指定固定的方法 @PrintTime：

```java
@PrintTime
public static String getStr() {
    try {
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    return "hello";
}
```

## 使用方法：

项目根目录：build.gradle 添加以下代码

```groovy
dependencies {
    classpath 'me.miqt.plugin.tools:pluginSrc:0.1.1'
}
```

对应 module 中启用插件，可以是`application`也可以是`library`

```groovy
apply plugin: 'miqt.plugin.tools'

methodtimer {
    enable = true //是否启用
    //项目中的class true：全部插桩 false：只有注解插桩
    all = true

    //指定插桩那些外部引用的jar，默认空，表示只对项目中的class插桩
    jarRegexs = [".*androidx.*"]
    //指定插桩那些类文件，默认空
    classRegexs = [".*view.*"]
    //所有包含 on 的方法,所有构造方法
    methodRegexs = [".*on.*", ".*init.*"]
    
    //编译时是否打印log
    log = true
    //是否用插桩后的jar包替换项目中的jar包，慎用
    replaceJar = false
    //是否生成详细mapping文件
    mapping = true
    //自定义方法统计实现类,不指定默认使用自带实现方式
    //impl = "com.miqt.plugindemo.MyTimeP"
}
```

添加类库依赖：

```groovy
dependencies {
    implementation 'me.miqt.plugin.tools:pluginlib:0.1.1'
}
```

这个插件是借鉴了很多大佬的代码，并结合自己的想法进行了一些调整，在此感谢他们付出的努力。

> https://github.com/novoda/bintray-release  
> https://github.com/JeasonWong/CostTime  
> https://github.com/MegatronKing/StringFog  
---
title: 安卓How To
p: android/android-howto
date: 2018-02-06 12:54:17
tags: Android
---

边学边做，初学时遇到的问题记录

## 如何使用Snackbar

   Snackbar 是 Android design support library 中的一个组件，它的作用和Toast类似，显示吐司，但Snackbar的特别之处在于Snackbar显示的提示信息可以和用户交互，更好地获取用户反馈信息。

   ```,java
    Snackbar.make(view,"Hello SnackBar!",Snackbar.LENGTH_SHORT).show();
   ```
   
   *使用问题*  打开输入法时，Snackbar的显示消息会被键盘遮挡

   *相关资源* 
   - Snakbar只能在底部显示，想要在顶部或中部显示，可以使用开源项目: https://github.com/nispok/snackbar 或者 https://github.com/AndreiD/TSnackBar
   - 具有类似能力Toast(吐司)类

## ConstraintLayout 如何使用
ConstraintLayout 是一种非常灵活的布局，核心思想是按照各个组件相对关系约束组件大小，位置，间距等布局属性。

主要大小属性： 固定大小，明确指定组件大小；匹配内容,适配组件内容大小然后再依据约束条件设定位置；匹配约束：优先使用四周约束关系计算组件实际大小，很适合在组件大小需要自适应的场景。

 *相关资源*  
 - 很多博客都有详细的用法研究，可以参考： https://www.jianshu.com/p/a8b49ff64cd3 和 https://segmentfault.com/a/1190000009565417
 - 这篇更侧重具体属性的讲解 http://blog.csdn.net/u013187628/article/details/60751812


## 如何实现不同环境打包时使用不同变量或资源

关键字： 多渠道打包，动态替换常量

在gradle配置里配置 buildConfigField ， 打包时gradle会根据配置生成常量类 BuildConfig。配置示例：

```,groovy
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release
            buildConfigField("String", "URL_API_ENDPOINT", "\"http://product.yourdomain.com\"")
        }
        debug {
            signingConfig signingConfigs.debug
            buildConfigField("String", "URL_API_ENDPOINT", "\"http://devleop.yourdomain.com\"")
        }
    }
```
使用方法
```,java
  public String getUrl(String uri) 
   return BuildConfig.URL_API_ENDPOINT + uri;
  }
```

那么，debug环境下 `BuildConfig.URL_API_ENDPOINT` 的值是`http://devleop.yourdomain.com`， 而在 release 环境下`BuildConfig.URL_API_ENDPOINT` 的值是`http://product.yourdomain.com`

 *相关资源*  
 
- https://www.jianshu.com/p/533240d222d3 还提供了下列配置 
 
        不同环境，不同包名；
        不同环境，修改不同的 string.xml 资源文件；
        不同环境，修改指定的常量；
        不同环境，修改 AndroidManifest.xml 里渠道变量；
        不同环境，引用不同的 module。
 - http://blog.csdn.net/droidrzy/article/details/61200115 讲解了`buildType`常见的几个配置
 - https://www.jianshu.com/p/9df3c3b6067a 可以作为安卓gradle配置快速入门参考

- https://developer.android.google.cn/studio/build/build-variants.html#workBuildVariants 官方资源



## 出现Performing stop of activity that is not resumed 异常

在Activtity的`onCreate`方法中启动了另一个Actitvity, 同时导致前一个Activity生命周期未正确完成。例如`oncreate->onstart->onstop`.
解决方案是使用`Handler`的分发处理消息

kotlin代码
```，kotlin
      // 使用handler防止出现异常 Performing stop of activity that is not resumed
      rootHandler.post {
        this.startActivity(
                Intent(applicationContext, LoginActivity::class.java)
                        .setFlags(
                                Intent.FLAG_ACTIVITY_NEW_TASK
                                        .or(Intent.FLAG_ACTIVITY_CLEAR_TASK)
                        )
        )
      }
```
*相关资源*
- https://www.cnblogs.com/JohnTsai/p/5259869.html   Handler讲解

## 出现异常  Calling startActivity() from outside of an Activity

异常消息

    Caused by: android.util.AndroidRuntimeException: Calling startActivity() from outside of an Activity  context requires the FLAG_ACTIVITY_NEW_TASK flag. Is this really what you want?

Context中有一个startActivity方法，Activity继承自Context，重载了startActivity方法。如果使用Activity的startActivity方法，不会有任何限制，而如果使用Context的startActivity方法的话，就需要开启一个新的task，遇到上面那个异常的，都是因为使用了Context的startActivity方法。解决办法是，加一个flag `Intent.FLAG_ACTIVITY_NEW_TASK`。

我的场景是试图在Application中直接启动一个登录界面Activity
```,java
intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);  
```

## 如何实现清空Activity栈

方法1: 使用 `Intent.FLAG_ACTIVITY_NEW_TASK` 和 `Intent.FLAG_ACTIVITY_CLEAR_TASK`

```，kotlin
        this.startActivity(
                Intent(applicationContext, LoginActivity::class.java)
                        .setFlags(
                                Intent.FLAG_ACTIVITY_NEW_TASK
                                        .or(Intent.FLAG_ACTIVITY_CLEAR_TASK)
                        )
        )
```
更多方法参照: http://blog.csdn.net/swjtuxu/article/details/26163737



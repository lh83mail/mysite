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



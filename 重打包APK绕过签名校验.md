---
title: 重打包APK绕过签名校验
tags:
  - RE
  - Android
abbrlink: 792658f8
date: 2021-09-07 19:43:57
---

### 这里先提一种针对性校强但简单好理解的办法，纯Java实现，代码大概也就50行不到吧。

### 还有更强的并且能过各种保护（反调试反HOOK反内存修改等等）的万能方法，不过较复杂，长篇大论的，等有空整理出来再提

## 本文适用场景：

### 1.需要重打包APK给普通用户，没有root权限也没有HOOK框架

### 2.so很难脱壳修复分析修改，或者逆向时间会很长很长。

### 3.so中调用了this.getPackageManager().getPackageInfo来获取签名进行校验

故事开始，某天，你暗恋的女神需要某个app的VIP，怎奈你兜兜空空如也，根本买不起VIP，然后你发现这个app的VIP是本地判断的，于是……
经过一番折腾，很快就改好了APK，重打包安装运行。提示非法操作？
又一番折腾，发现程序主要逻辑在so中，so被混淆的面目全非还进行了加固，最后发现在so中进行了APK签名校验，于是用hook过了签名校验，正常运行，VIP到手。
可是这样怎么发给女神呢？女神的手机没有root也没有hook框架。分析so？实在太复杂了，一时半会根本来不及，要让女神等你三五十天？恐怕到时候是备胎都当不成了吧。
怎么办？

## 总体思路：

既然修改so不成，也无法使用hook框架，那么就从别处入手。

### 本文用本人曾经悬赏过的加固APK（悬赏期内没人破解成功，加固强度还可以）进行分析：

由于so非常复杂，时间关系直接放弃分析。通过HOOK发现so的中签名校验是通过this.getPackageManager().getPackageInfo来获取的，于是想法就来了——
这个this是MainActivity的实例，所以这个this是一个很大的突破口：
思路活跃的同学可能已经发现了，只要覆盖MainActivity的getPackageManager就行了。

## 具体过程如下：

### 1.写一个HookPackageManager，专门用来HOOK各种PM相关方法：

```Java
public class HookPackageManager extends PackageManager {
 
    private PackageManager mBase; //用来做适配，返回其他信息
 
    public HookPackageManager(PackageManager base) {
        mBase = base;
    }
 
    @Override
    public PackageInfo getPackageInfo(String packageName, int flags)
            throws NameNotFoundException {
        if (!"ywt.android.test6".equals(packageName)) {
            //如果不是目标APK，则返回原始数据
            return mBase.getPackageInfo(packageName, flags);
        }
        PackageInfo pkgInfo = new PackageInfo();
        pkgInfo.signatures = new Signature[] {
            new Signature(new byte[] {
                //这是填写原始APK中读到的签名，用来欺骗so，很长就不贴了，后面会给出APK，反编译看即可。
            })
        };
        return pkgInfo;
    }
 
    //下面还有很多需要implements的方法，太多了，就不写了，后面会给出APK，反编译看即可，只要用mBase一一调用返回即可。
}
```

### 2.写一个HookMainActivity，代码如下：

```java
public class HookMainActivity extends Activity {
 
    @Override
    public PackageManager getPackageManager() {
        //由于调用的是this.getPackageManager()，所以只要重写getPackageManager，返回一个伪造的实例，就可以实现欺骗
        PackageManager pm = new HookPackageManager(super.getPackageManager());
        return pm;
    }
}
```

### 3.将写好的Java代码反编译为smali，将crackme.apk也反编译为smali，然后将crackme中的MainActivity.smali中的父类修改为继承HookMainActivity

### 4.重新打包APK，就可以绕过APK的签名校验啦。

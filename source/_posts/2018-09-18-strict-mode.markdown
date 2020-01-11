---
layout: post
title: 严格模式
date: 2018-09-18T07:33:54+00:00
categories: android
lang: zh
---

昔日凭借**单元测试，严格模式，崩溃日志分析**三大法宝，
我同其他同事，一起把360手机卫士的错误率，降到了万分之三以下。
时至今日仍然未见到比较好的，使用StrictMode的代码工具类或者教程，
今天拿出来分享一下，给有志提高代码质量的猿。

### 为什么需要严格模式

有些问题不能被立刻发现，容易造成很多莫名其妙的现象。

或者短期没有异常现象，影响到了长期的稳定。

例如：内测泄露，文件资源不释放，数据库不关闭等。

为了能够即时发现这些错误，安卓引入了严格模式这个概念。


### 严格模式的含义
严格模式是一个开关，开启之后一些严格检查的代码会生效。
检查到问题后会主动崩溃报错。

#### 严格模式的精神

尽快暴露错误，而不是隐藏和绕过错误。

#### 严格模式有两个特点：

1. 什么时候出错，什么时候就会报错；
2. 哪里出错，就会在哪里报错。

### 怎样开启

严格模式是默认关闭等，这样用户体验是稳定的，不受影响。

测试过程中，通过开关软件开启严格模式，这样能发现更多问题。

工具类
```java
package ovo.top.app;

import android.os.Build;
import android.os.Environment;
import android.os.StrictMode;
import android.util.Log;

import java.io.File;

/**
 * 参考http://www.jianshu.com/p/98f348fc7688
 * Created by 曹建峰(windcao@hotmail.com) on 2017/2/22.
 * 单独控制activity开关
 */
public class StrictModeManager {
    private static final String STRICT_MODE_CFG = "ovo.top.stictmode.cfg/enable";
    private static final String ACTIVITY_LEAKS_DETECT_CFG = "ovo.top.stictmode.cfg/enable/activity_leaks_detect";
    private static final boolean DEBUG = false;
    private static boolean sIsInStrictMode = false;
    private static boolean sEnableActivityLeaksDetect = false;
    private static boolean sIsFlgChecked = false;

    public static void init() {
        checkStrictModeCfg();
        if (sIsInStrictMode) {
            StrictMode.VmPolicy.Builder builder = new StrictMode.VmPolicy.Builder();
            if (sEnableActivityLeaksDetect) {
                builder.detectAll();
            } else {
                if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.HONEYCOMB) {
                    builder.detectLeakedClosableObjects();
                    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN) {
                        builder.detectLeakedRegistrationObjects();
                        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR2) {
                            builder.detectFileUriExposure();
                        }
                    }
                }
                builder.detectLeakedSqlLiteObjects();
            }

            builder.penaltyLog().penaltyDropBox().penaltyDeath();
            StrictMode.setVmPolicy(builder.build());

            StrictMode.setThreadPolicy(new StrictMode.ThreadPolicy.Builder()
                    .detectNetwork()
                    .penaltyLog()
                    .penaltyDropBox()
                    .penaltyDialog()
                    .build());
        }
    }

    public static boolean isInStrictMode() {
        checkStrictModeCfg();
        return sIsInStrictMode;
    }

    /**
     * 有些情况下我们希望立刻发现错误，有任何异常都曝光出来，而不是带着错误继续执行。这样可以尽快发现错误。
     * 这种情况下将刚捕获的异常放到handleException就可以了。
     * 严格模式下，这些被处理的Exception 仍会立刻抛出。而非严格的模式下，会写日志。
     * @param e
     * @param msg
     */
    public static void handleException(Exception e, String msg) {
        if (isInStrictMode()) {
            throw new RuntimeException(msg, e);
        } else if (DEBUG) {
            Log.e("Exception", msg, e);
        }
    }

    private static void checkStrictModeCfg() {
        if (sIsFlgChecked == false) {
            File strictModeCfgFile = new File(Environment.getExternalStorageDirectory(), STRICT_MODE_CFG);
            sIsInStrictMode = strictModeCfgFile.exists();

            File activityLeaksCfgFile = new File(Environment.getExternalStorageDirectory(), ACTIVITY_LEAKS_DETECT_CFG);
            sEnableActivityLeaksDetect = activityLeaksCfgFile.exists();

            sIsFlgChecked = true;
        }
    }
}

```

使用方法：
```
在Application的onCreate中添加一行代码
  public void onCreate() {
        StrictModeManager.init();
        super.onCreate();
 }
```
开启开关：
```
caojianfeng$ adb shell mkdir /sdcard/ovo.top.stictmode.cfg/
caojianfeng$ adb shell mkdir /sdcard/ovo.top.stictmode.cfg/enable
caojianfeng$ adb shell mkdir /sdcard/ovo.top.stictmode.cfg/enable/activity_leaks_detect

```

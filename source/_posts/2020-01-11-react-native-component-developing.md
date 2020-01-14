---
title: react-native component developing
date: 2020-01-11 12:30:07
categories: react-native
tags:
---

# React-Native 中的组件化开发实战

## 背景
假设你同时维护多了个app，
并且有很多可以相似的业务逻辑，
每次修改你都需要逐个修改工程的。
好烦啊

```
┌─────────────┐ ┌─────────────┐ ┌─────────────┐
│     appA    │ │     appB    │ │     appC    │
│ ┌─────────┐ │ │ ┌─────────┐ │ │ ┌─────────┐ │
│ │ partOne │ │ │ │ partOne │ │ │ │ partOne │ │
│ └─────────┘ │ │ └─────────┘ │ │ └─────────┘ │
└─────────────┘ └─────────────┘ └─────────────┘
```

## 解决：组件化

使用组件维护相似功能代码。
```txt
┌─────────────┐ ┌─────────────┐ ┌─────────────┐
│     appA    │ │     appB    │ │     appC    │
│ ┌ ┈ ┈ ┈ ┈ ┐ │ │ ┌ ┈ ┈ ┈ ┈ ┐ │ │ ┌ ┈ ┈ ┈ ┈ ┐ │
│ ┊         ┊ │ │ ┊         ┊ │ │ ┊         ┊ │
│ └┈ ┈ ┬ ┈ ┈┘ │ │ └┈ ┈ ┬ ┈ ┈┘ │ │ └┈ ┈ ┬ ┈ ┈┘ │
└──────┼──────┘ └──────┼──────┘ └──────┼──────┘
       │               │               │
       ╰───────────────┼───────────────╯
                       │
                  ┌────┴────┐
                  │ partOne │
                  └─────────┘
```

## 🌰举个例子：
我们要在每个应用中显示隐私政策。
我们可以将这个页面放到公共组件里面，让每个应用都引用它。

考虑每个应用的隐私政策都不相同，
我们可以将文本内容留在App里面，作为参数传递给组件。

## 怎样做

step 1. 创建组件工程

step 2. 创建app工程

step 3. 在app中调试组件

step 4. 发布组件

### step 1. 创建组件工程

FB为我们准备了很好的创建组件的工具react-native-create-library

#### 1.1 安装：
命令行输入：
```
npm install -g react-native-create-library
或者
yarn global add react-native-create-library
```
输出：
```
/usr/local/bin/react-native-create-library -> /usr/local/lib/node_modules/react-native-create-library/cli.js
+ react-native-create-library@3.1.2
added 90 packages from 38 contributors in 77.753s
```
命令行输入：
```
react-native-create-library init protocal-view
```
输出：
```
While `RN` is the default prefix,
  it is recommended to customize the prefix.

📚  Created library init in `./init`.
🕘  It took 10ms.
➡️  To get started type `cd ./init`
```

#### 1.2 使用：
通过命令行输入：react-native-create-library YOUR_COMPONENT_NAME
这里我们用protocal-view作为组件名
```
react-native-create-library protocal-view
```
命令行输出：
```
soukenbons-iMac:npms caojianfeng$ react-native-create-library protocal-view
While `RN` is the default prefix,
  it is recommended to customize the prefix.

📚  Created library protocal-view in `./protocal-view`.
🕘  It took 33ms.
➡️  To get started type `cd ./protocal-view`
```
新创建的工程目录如下：
```
├── README.md
├── android
│   ├── build.gradle
│   └── src
│       └── main
│           ├── AndroidManifest.xml
│           └── java
│               └── com
│                   └── reactlibrary
│                       ├── RNProtocalViewModule.java
│                       └── RNProtocalViewPackage.java
├── index.js
├── ios
│   ├── RNProtocalView.h
│   ├── RNProtocalView.m
│   ├── RNProtocalView.podspec
│   ├── RNProtocalView.xcodeproj
│   │   └── project.pbxproj
│   └── RNProtocalView.xcworkspace
│       └── contents.xcworkspacedata
├── package.json
└── windows
    ├── RNProtocalView
    │   ├── Properties
    │   │   ├── AssemblyInfo.cs
    │   │   └── RNProtocalView.rd.xml
    │   ├── RNProtocalView.csproj
    │   ├── RNProtocalViewModule.cs
    │   ├── RNProtocalViewPackage.cs
    │   └── project.json
    └── RNProtocalView.sln
```

然后你可以在你的组件工程中实现你需要的功能了。


#### 1.3 改造刚创建的工程

由于目前我们要实现的这个功能比较简单不需要用到原生的桥接，
因此我们可以将原生目录（ios/、android/、windows/）删掉。

完成修改后的目录：
```
├── README.md
├── index.js
└── package.json
```



### step 2. 创建app工程


#### 创建App，命名为protocal_view_demo

命令行输入：
```
react-native init protocal_view_demo
```

得到的工程目录如下面的protocal_view_demo：
```
.
├── protocal_view
│   ├── README.md
│   ├── dlg.js
│   ├── html_dlg_view
│   ├── index.js
│   ├── package.json
│   └── protocal_dlg_view
└── protocal_view_demo
    ├── App.js
    ├── __tests__
    ├── android
    ├── app.json
    ├── babel.config.js
    ├── index.js
    ├── ios
    ├── metro.config.js
    ├── node_modules
    ├── package.json
    ├── utils.js
    └── yarn.lock

```


### step 3. 在app中调试组件

对于一个已经发布的npm组件，我们只需要 yarn add 就可以安装组件了。
怎样调试一个尚未发布的组件呢？


最简单的办法是将组件的工程复制一份到protocal_view_demo/node_modules中
但是这样每次修改都需要拷贝一次。

另一个可行的办法是，将现有的组件工程复制一份到App的node_modules下面。
```
.
└── protocal_view_demo
    ├── App.js
    ├── __tests__
    ├── android
    ├── app.json
    ├── babel.config.js
    ├── index.js
    ├── ios
    ├── metro.config.js
    ├── node_modules
    │   └── protocal_view
    │       ├── README.md
    │       ├── dlg.js
    │       ├── html_dlg_view
    │       ├── index.js
    │       ├── package.json
    │       └── protocal_dlg_view
    ├── package.json
    ├── utils.js
    └── yarn.lock

```

但这样做有两个缺点：
1. git 管理比较混乱，通常我们的app工程和组件工程都需要放到git中。当组件放到protocal_view_demo/node_modules中
2. 有时候调试的npm server更新不正确，需要清空watch 和 缓存重新刷新，这时候还需要备份protocal_view_demo/node_modules/protocal_view

如果是做过js前端开发的人很容易想到npm link,或者yarn link

参考：[How to test your new NPM module without publishing it every 5 minutes](https://medium.com/@the1mills/how-to-test-your-npm-module-without-publishing-it-every-5-minutes-1c4cb4b369be)

```
cd <组件工程根目录>
yarn link
cd <调用组件的工程的根目录>
yarn link <组件名> 
```
但是不幸的是这样做，并不能支持HotReload。

经过搜寻和探索发现，我们可以用wml来实现热更新。

### 最佳实践
使用wml动态更新RN库代码修改，实现组件的hotReload调试。

#### 参考：
https://www.bram.us/2018/03/10/working-with-symlinked-packages-in-react-native/

#### step3.1 安装wml:
```
npm install -g wml
yarn global add wml
```

### step3.2 添加监控变化目录
在protocal_view和protocal_view_demo的父目录中输入命令：
```bash
wml add protocal_view/ protocal_view_demo/node_modules/react-native-protocal-view
```

输出如下：
```bash
? Source folder is a git repo, add `.git` to ignored folders? Yes
? Source folder is an npm package, add `node_modules` to ignored folders? Yes
Added link: (0) /Volumes/user/cjf/w/npms/protocal_view -> /Volumes/user/cjf/w/npms/protocal_view_demo/node_modules/react-native-protocal-view
```

输出中会提示添加忽略.git目录和node_modules目录，我们直接选择yes就可以了。
这个配置会保存在protocal_view_demo/.watchmanconfig文件中
```json
{
  "ignore_dirs": [
    ".git",
    "node_modules"
  ]
}

```


### step3.3 接下来我们启动wml服务监控

一共需要打开3个终端窗口。
在第一个终端窗口中输入：
```bash
wml start
```

输出:
```bash
[watch] /Volumes/user/cjf/w/npms/protocal_view
[watch-config] { ignore_dirs: [ '.git', 'node_modules' ] }
[subscribe] /Volumes/user/cjf/w/npms/protocal_view
[copy] /Volumes/user/cjf/w/npms/protocal_view/protocal_dlg_view/styles.js -> /Volumes/user/cjf
...

```

### step3.4 愉快的开发组件

添加组件代码：
```
├── protocal_view
│   ├── README.md
│   ├── dlg.js
│   ├── html_dlg_view
│   ├── index.js
│   ├── package.json
│   └── protocal_dlg_view
```

组件开发过程中用了两个rn第三方库，将其添加到protocal_view/package.json文件中。

与日常的依赖不同的是npm组件的依赖需要添加到peerDependencies下面。
```json
{
  "name": "@zhike-private/rn-protocal-view",
  "version": "1.0.1",
  "description": "Show protocal to customers",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "react-native"
  ],
  "author": "JeffreyCao",
  "license": "MIT",
  "peerDependencies": {
    "react-native": "^0.41.2",
    "react-native-windows": "0.41.0-rc.1",
    "react-native-modalbox": "^1.6.0",
    "react-native-exit-app": "^1.1.0"
  }
}
```
### 3.5 启动app并调试

#### 3.5.1 在app的文件中添加依赖

```
yarn add react-native-modalbox react-native-exit-app
react-native link react-native-exit-app
cd ios
pod install
cd ..
```
#### 3.5.2 在app的package.json中手动添加开发中的组件的依赖
添加前
```json
{
  "...":"...",
  "dependencies": {
    "react": "16.9.0",
    "react-native": "0.61.5",
    "react-native-exit-app": "^1.1.0",
    "react-native-modalbox": "^2.0.0"
  },
  "devDependencies": {
    "...":"..."
  },
  "...":"..."
  
}
```
添加后
```json
{
  "...":"...",
  "dependencies": {
    "react": "16.9.0",
    "react-native": "0.61.5",
    "react-native-exit-app": "^1.1.0",
    "react-native-modalbox": "^2.0.0",
    "react-native-protocal-view": "1.0.0"
  },
  "devDependencies": {
    "...":"..."
  },
  "...":"..."
  
}

```

在引用组件的文件中导入组件：
![](adding_component.png)
完成相应的工作。

然后在第二个终端里面启动npm server 
```
npm start --reset-cache
```

在第三个终端里面启动app
```
react-native run-ios
```

像往常一样编译和启动我们的app吧
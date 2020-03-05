---
title: flutter_create
date: 2020-03-03 22:21:08
tags:
---
# 玩转 flutter create


我们随时可以很方便的差官方帮助文档：

终端中输入命令
```bash
$ flutter create --help
```

输出如下：

```txt

Create a new Flutter project.

If run on a project that already exists, this will repair the project, recreating any files that are missing.

Usage: flutter create <output directory>
-h, --help                     Print this usage information.
    --[no-]pub                 Whether to run "flutter pub get" after the project has been created.
                               (defaults to on)

    --[no-]offline             When "flutter pub get" is run by the create command, this indicates whether to run it in offline mode or not. In offline mode, it will need to have all dependencies already available in the pub cache to succeed.
    --[no-]with-driver-test    Also add a flutter_driver dependency and generate a sample 'flutter drive' test.
-t, --template=<type>          Specify the type of project to create.

          [app]                (default) Generate a Flutter application.
          [module]             Generate a project to add a Flutter module to an existing Android or iOS application.
          [package]            Generate a shareable Flutter project containing modular Dart code.
          [plugin]             Generate a shareable Flutter project containing an API in Dart code with a platform-specific implementation for Android, for iOS code, or for both.

-s, --sample=<id>              Specifies the Flutter code sample to use as the main.dart for an application. Implies --template=app. The value should be the sample ID of the desired sample from the API documentation website
                               (http://docs.flutter.dev). An example can be found at https://master-api.flutter.dev/flutter/widgets/SingleChildScrollView-class.html

    --list-samples=<path>      Specifies a JSON output file for a listing of Flutter code samples that can created with --sample.
    --[no-]overwrite           When performing operations, overwrite existing files.
    --description              The description to use for your new Flutter project. This string ends up in the pubspec.yaml file.
                               (defaults to "A new Flutter project.")

    --org                      The organization responsible for your new Flutter project, in reverse domain name notation. This string is used in Java package names and as prefix in the iOS bundle identifier.
                               (defaults to "com.example")

    --project-name             The project name for this new Flutter project. This must be a valid dart package name.
-i, --ios-language             [objc, swift (default)]
-a, --android-language         [java, kotlin (default)]
    --[no-]androidx            Generate a project using the AndroidX support libraries
                               (defaults to on)

Run "flutter help" to see global options.
```

|选项|子选项|解释|
|:---:|:---:|:---:|
| --[no-]pub||创建命令完成后，是否执行'flutter pub get'命令。（默认执行）|
|--[no-]offline||是否以离线模式执行'flutter pub get'。前提是之前缓存过项目所有相关的依赖文件|
|--[no-]with-driver-test||是否添加flutter_driver依赖并生成flutterdrive例子，这是用来进行单元测试的工具|
|-t, --template=\<type\>||声明创建工程的类型，默认app|
||app|（默认）生成一个Flutter应用|
||module| 生成一个Flutter模块，并添加到Android或者iOS应用中|
||package|生成一个纯dart包|
||plugin|生成一个原生插件包|
|-s, --sample=<id>||选个例子......|

TODO:
https://github.com/flutter/flutter/tree/master/examples
https://github.com/flutter/samples
https://github.com/flutter/samples/tree/master/gallery/lib/studies
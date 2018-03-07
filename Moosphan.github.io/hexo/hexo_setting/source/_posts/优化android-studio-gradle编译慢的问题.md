---
title: 优化android studio gradle编译慢的问题
date: 2017-09-30 08:48:08
tags: 
     - Android
     - 编译优化
     - gradle
---

gradle同步比较慢,主要根源在于gradle从外网拉取下载第三方框架很慢,参考:https://www.zhihu.com/question/37810416
优化方案:

- 购买使用vpn
- 使用国内大公司如阿里的镜像仓库地址:

```
maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
maven{ url 'http://maven.aliyun.com/nexus/content/repositories/jcenter'}

```
- 使用命令行指定代理:
```
./gradlew -DsocksProxyHost=127.0.0.1 -DsocksProxyPort=1080 tasks
```

- 下载gradle-zip文件放到本地
前往https://gradle.org/install/页面下载自己需要的gradle版本到本地,将其放到\Users\xxx\.gradle\wrapper\dists\gradle-2.4-all目录下


---
layout: post
title: PHP curl 请求https资源的问题
date: 2014-08-12
author: scsidisk
category: PHP
tags: [PHP, Yii]
---

今天使用 PHP 连接 CAS 进行登录验证。 一直不行，非常郁闷。

网上查了以后找到定位 curl 日志输出。很快定位问题。记录一下。

## 记录 curl 请求日志

```
    $curl = curl_init();
    $curl_log = fopen("/tmp/curl.txt", 'w');
    $url = "https://github.com/";

    curl_setopt_array($curl, array(
        CURLOPT_URL             => $url,        // Our destination URL
        CURLOPT_VERBOSE         => 1,           // 记录日志到 STDERR
        CURLOPT_STDERR          => $curl_log,   // 输出 STDERR 日志到文件
        CURLOPT_FAILONERROR     => 0,           // true to fail silently for http requests > 400
        CURLOPT_RETURNTRANSFER  => 1,            // Return data received from server
        CURLOPT_CONNECTTIMEOUT => 120,
        CURLOPT_TIMEOUT => 120,
        CURLOPT_SSL_VERIFYHOST => 2,
        CURLOPT_SSL_VERIFYPEER  => false,           // Do not verify certificate
    ));

    $output = fread($curl_log, 2048);
    echo $output;

    $response = curl_exec($curl);
```

激活一下请求。然后查看日志文件

```
cat /tmp/curl.txt
```

就可以看到日志输出。

## SSL23_GET_SERVER_HELLO 错误

如果发现这种错误，在链接参数里面添加

```
CURLOPT_SSLVERSION => 3,
```


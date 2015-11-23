---
layout: post
title: golang 使用 redis
date: 2013-10-20 11:02
author: scsidisk
category: Golang
tags: [Go, Golang, Redis]
---

###安装

    go get github.com/hoisie/redis

###例子

Most of the examples connect to a redis database running in the default port -- 6367.

.Hello World example

```go
package main

import "github.com/hoisie/redis"

func main() {
    var client redis.Client
    var key = "hello"
    client.Set(key, []byte("world"))
    val, _ := client.Get("hello")
    println(key, string(val))
}
```

.Strings

```go
var client redis.Client
client.Set("a", []byte("hello"))
val, _ := client.Get("a")
println(string(val))
client.Del("a")
```

.Lists

```go
var client redis.Client
vals := []string{"a", "b", "c", "d", "e"}
for _, v := range vals {
    client.Rpush("l", []byte(v))
}
dbvals,_ := client.Lrange("l", 0, 4)
for i, v := range dbvals {
    println(i,":",string(v))
}
client.Del("l")
```

.Publish/Subscribe

```go
sub := make(chan string, 1)
sub <- "foo"
messages := make(chan Message, 0)
go client.Subscribe(sub, nil, nil, nil, messages)

time.Sleep(10 * 1000 * 1000)
client.Publish("foo", []byte("bar"))

msg := <-messages
println("received from:", msg.Channel, " message:", string(msg.Message))

close(sub)
close(messages)
```

More examples coming soon. See redis_test.go for more usage examples.

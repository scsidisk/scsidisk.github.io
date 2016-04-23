---
layout: post
title: "Node.js中用mongoose模块对mongodb的一些基本操作"
date: 2012-09-27
author: scsidisk
categories: Node
tags: Node Node.js MongoDB
---

下面是 Node.js 中用 mongoose 模块对 mongodb 的一些基本操作。

```js
var express = require('express'),
    mongoose = require('mongoose'); //引入mongoose模块
//连接mongodb数据库　nodejs为数据库名称
mongoose.connect('mongodb://localhost/nodejs');

//获取Schema 以及 ObjectId 对象
var Schema = mongoose.Schema,
    ObjectId = Schema.ObjectId;

//创建一个评论Schema(结构＆架构) 这里相当于mongodb中的collection(集合)
var commentsShema = new Schema({
    name:String,
    content:String
});
//生成一个用于操作comments这个collection的Ｍodel对象　　第一个参数为mongodb中的collection名称（需要先创建）
var CommentModel = mongoose.model('comments', commentsShema);

//新闻Schema
var newsSchema = new Schema({
    title : String,
    source : String,
    content : String,
    comments :{type:[commentsShema], default:[]} //新闻的评论，这里是一个子集合(collection),记得一定要用中括号包起来(type:[commentsShema])，默认为一个空数组(default:[])
});
//生成新闻的Model
var NewsModel = mongoose.model('news', newsSchema);

//新建一个新闻
var news = new NewsModel();
news.title = '国家税务总局：节假日加班工资须缴个税';
news.source = 'http://finance.qq.com/a/20120221/001221.htm';
news.content = '昨日，国税总局纳税服务司就纳税咨询热点问题作出解答....';
news.save(function(err){
    console.log(err);
});

//查询新闻
NewsModel.find({}, function(err,docs){

    if(!err){
        if(docs[0]){
            //添加一条评论
            var comment = new CommentModel();
            comment.name = 'iblue';
            comment.content = '测试评论!';
            docs[0].comments.push(comment);
            NewsModel.update({_id:docs[0]._id}, {comments:docs[0].comments}, function(err){
                console.log(err);
            });
        }
    }
});
```
---
layout: post
title: YII Relation中非主键字段关联
date: 2014-09-16
author: scsidisk
category: PHP
tags: PHP, Yii
---

有两张表，设计结构如下：

商品表：字段：p_id,member_id

商铺表：字段：shop_id,member_id

其中 p_id,shop_id 都是主键，member_id 是索引

考虑过这个原因，是最初设计的时候，每一个普通用户都可以发布商品，所以商品表没有记录shop_id。

可能也会考虑过，每一个用户会有多个商铺吧，所以 shop 表的 member_id 也不是唯一索引。

然后，我用Yii在做表关联，事实上，在我们改动程序的时候，我们已经把普通用户能够发布商品这个功能去掉了，因此，事实上对我们来说 member_id，其实肯定是于 shop 表中的 member_id 有对应关系。

于是我在 ShopProduct 的 relations 这样

```
/**
* @return array relational rules.
*/
public function relations()
{
    // NOTE: you may need to adjust the relation name and the related
    // class name for the relations automatically generated below.
    return array(
        'member' => array(self::BELONGS_TO,'ShopMember','member_id','with'=>'extends','condition'=>"member.member_state = '1'"),
        'shop' => array(self::BELONGS_TO , 'ShopInfo' , 'member_id','on'=>'t.member_id = shop.member_id' ),
    );
}
```

是的，看上去好象没问题，但是实际却关联不上。

在Yii的代码中，关于 relation 的on是这样写的：

```
'on': the ON clause. The condition specified here will be appended to the joining condition using the AND operator. This option has been available since version 1.0.2.
```

所以，上述的代码在SQL中显示出来就是 `select * from product left outer join shop on (product.member_id = shop.shop_id and product.member_id = shop.member_id)` 这个代码是伪代码，只是为了说明问题。

出现这种情况可以将 foreignKey 字段设为空，然后再指定 ON 。

```
/**
* @return array relational rules.
*/
public function relations()
{
    // NOTE: you may need to adjust the relation name and the related
    // class name for the relations automatically generated below.
    return array(
        'member' => array(self::BELONGS_TO,'ShopMember','member_id','with'=>'extends','condition'=>"member.member_state = '1'"),
        'shop' => array(self::BELONGS_TO , 'ShopInfo' , '','on'=>'t.member_id = shop.member_id' ),
    );
}
```

查找的时候必须使用 'with' 关键字，例如

```
Member::model()->with('shop')->findAll($criteria);
```


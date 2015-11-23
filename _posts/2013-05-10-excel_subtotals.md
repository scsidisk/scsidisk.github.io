---
layout: post
title: 怎样在Excel中对数据进行分类汇总
author: scsidisk
date: 2013-05-10 13:25:59
category: 文档处理
tags: [AsciiDoc]
---

文章来源：www.excel123.cn

当需要在Excel中对数据进行分类计算时，除了使用数据透视表，还可以使用分类汇总命令。与数据透视表不同的是，它可以直接在数据区域中插入汇总行，从而可以同时看到数据明细和汇总。下面是分类汇总的使用方法：

在进行分类汇总前，需保证数据具有下列格式，即数据区域的第一行为标题行，数据区域中没有空行和空列，数据区域四周是空行和空列，如下图是几种商品在一些城市的销售数据。另外，如果数据区域在应用分类汇总前已被设置成Excel 2003列表或Excel 2007表，需将其转换为普通区域。因为对于Excel 2003列表或Excel 2007表无法使用分类汇总。

image:/images/2010050611191559.jpg[]

== 仅对某列进行分类汇总

例如上例中需要对各城市的销售量进行分类汇总，方法如下：

1. 首先对数据按需要分类汇总的列（本例为“城市”列）进行排序。
+
选择“城市”列中的任意单元格，在Excel 2003中单击工具栏中的排序按钮如“A→Z”。在Excel 2007中，选择功能区中“数据”选项卡，在“排序和筛选”组中单击“A→Z”按钮。
+
2. 选择数据区域中的某个单元格，在Excel 2003中单击菜单“数据→分类汇总”。如果是Excel 2007，则在“数据”选项卡的“分级显示”组中单击“分类汇总”。
+
3. 在弹出的“分类汇总”对话框中，在“分类字段”下选择“城市”，在“汇总方式”中选择某种汇总方式，可供选择的汇总方式有“求和”、“计数”、“平均值”等，本例中选择默认的“求和”。在“选定汇总项”下仅选择“销售额”。
+
image:/images/2010050611351536.jpg[]
+
4. 单击确定，Excel将按城市进行分类汇总。
+
image:/images/2010050613255355.jpg[]

== 对多列进行分类汇总

如上例中需要同时对“城市”列和“商品名称”列进行分类汇总，可以插入嵌套分类汇总。

1. 对数据进行多列排序，即进行多关键字排序。
+
首先选择数据区域中的某个单元格。
+
在Excel 2003中，单击菜单“数据→排序”。弹出“排序”对话框，其中主要关键字选择“城市”，次要关键字选择“商品名称”，其他选择默认。
+
image:/images/2010050613382088.jpg[]
+
如果是Excel 2007，在“数据”选项卡的“排序和筛选”组中单击“排序”命令，在弹出的“排序”对话框中，单击“添加条件”按钮添加次要关键字排序条件，然后主要关键字选择“城市”，次要关键字选择“商品名称”，其他选择默认。
+
image:/images/2010050613434321.jpg[]
+
2. 对“城市”列进行分类汇总（外部分类汇总）。
+
按上述方法打开“分类汇总”对话框，在“分类字段”下选择“城市”，在“汇总方式”中选择默认的“求和”，在“选定汇总项”下仅选择“销售额”。单击“确定”。
+
3. 对“商品名称”列进行分类汇总（嵌套分类汇总）。
+
再次打开“分类汇总”对话框，在“分类字段”下选择“商品名称”，取消选择“替换当前分类汇总”，单击“确定”。
+
image:/images/2010050613542468.jpg[]
+
这时Excel将按“城市”列和“商品名称”列对“销售额”进行分类汇总。
+
image:/images/2010050613575818.jpg[]
+
如果不需要显示明细数据，可以单击左侧的分级显示符号，如本例中右上角的数字和左侧的减号来隐藏明细数据。
+
image:/images/2010050614053360.jpg[]

== 删除分类汇总

在“分类汇总”对话框中，单击“全部删除”即可。

image:/images/2010050614125576.jpg[]
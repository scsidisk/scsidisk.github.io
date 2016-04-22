---
layout: post
title: "SQL_CALC_FOUND_ROWS真的很慢么？"
date: 2010-12-28 13:52
author: scsidisk
category: Database
tags: [MySQL]
Slug: sql_calc_found_rows_really_slow_it
---

作者：老王

分页程序一般由两条SQL组成：

SELECT COUNT(\*) FROM ... WHERE ....

SELECT ... FROM ... WHERE LIMIT ...

如果使用SQL\_CALC\_FOUND\_ROWS的话，一条SQL就可以了：

SELECT SQL\_CALC\_FOUND\_ROWS ... FROM ... WHERE LIMIT ...

在得到数据后，通过FOUND\_ROWS()可以得到不带LIMIT的结果数：

SELECT FOUND\_ROWS()

看上去，似乎SQL\_CALC\_FOUND\_ROWS应该快于COUNT(\*)，但实际情况并不是这样简单，请看：

To SQL\_CALC\_FOUND\_ROWS or not to SQL\_CALC\_FOUND\_ROWS?

用数据说话，证明了COUNT(\*)相对SQL\_CALC\_FOUND\_ROWS来说更快。

不过我觉得这个结论也不全面，某些情况下，SQL\_CALC\_FOUND\_ROWS更有优势，看我的实验：

表结构如下：

CREATE TABLE IF NOT EXISTS \`foo\` (

\`a\` int(10) unsigned NOT NULL AUTO\_INCREMENT,

\`b\` int(10) unsigned NOT NULL,

\`c\` varchar(100) NOT NULL,

PRIMARY KEY (\`a\`),

KEY \`bar\` (\`b\`,\`a\`)

) ENGINE=MyISAM;

导入一些测试数据：

for (\$i = 0; \$i \<10000; \$i++) {

mysql\_query("INSERT INTO foo SET b=ROUND(RAND()\*10), c=MD5({\$i})");

}

先测试COUNT(\*)方式：

\$start = microtime(true);

for (\$i = 0; \$i \< 1000; \$i++) {

mysql\_query("SELECT SQL\_NO\_CACHE COUNT(\*) FROM foo WHERE b = 1");

mysql\_query("SELECT SQL\_NO\_CACHE a FROM foo WHERE b = 1 LIMIT 100,
10");

}

\$end = microtime(true);

echo \$end - \$start;

结果输出（数据大小视测试机性能而定）：0.75777006149292

再测试SQL\_CALC\_FOUND\_ROWS方式：

\$start = microtime(true);

for (\$i = 0; \$i \< 1000; \$i++) {

mysql\_query("SELECT SQL\_NO\_CACHE SQL\_CALC\_FOUND\_ROWS a FROM foo
WHERE b = 1 LIMIT 100, 10");

mysql\_query("SELECT FOUND\_ROWS()");

}

\$end = microtime(true);

echo \$end - \$start;

结果输出（数据大小视测试机性能而定）：0.6681969165802

有数据有真相，那为什么我的实验结论和MySQL Performance
Blog的结论相悖呢？这是因为：

在MySQL Performance Blog的实验里，COUNT(\*)查询是执行的的Covering
Index，而SQL\_CALC\_FOUND\_ROWS是执行的表查询；而在我的实验里，因为我定义了适当的索引，COUNT(\*)和SQL\_CALC\_FOUND\_ROWS都是执行的Covering
Index，所以结论出现了差异。

既然使用了Covering Index，就意味着不能再使用SELECT
\*的形式了，只能使用类似SELECT
id这样的形式了，用的列在索引里都能查到，如此说来，我们需要的实际数据从哪来呢？这个很简单，有了主键之后，实际数据可以通过Key/Value形式的缓存获得，这样的架构很常见。

结论：SQL\_CALC\_FOUND\_ROWS如果执行的是Covering
Index的话，是很快的！换个角度看，如果COUNT(\*)和SQL\_CALC\_FOUND\_ROWS都只能通过表查询来检索，那么分页时，SQL\_CALC\_FOUND\_ROWS同样会快于COUNT(\*)，读者可自行测试。

来源：http://hi.baidu.com/thinkinginlamp/blog/item/df85b31c93cd148686d6b64f.html

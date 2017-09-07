---
layout:     post
title:      "Solr支持同句同段搜索"
subtitle:   ""
date:       2016-01-28
author:     "itcamel"
tags:
    - solr
---

### 需求
搜索的时候，用户希望输入的关键字在同一句/或同一段中才算命中。

### 分析
* solr原生是没有这方面的支持，在网络上也没找到相应的解决方案。
* solr可以指定词距范围搜索，例如：
	>`q:"中国北京"~2`
	表示"中国"	,"北京"两个词在原文中的距离小于等于2时命中，否则未命中。
	
* 顺着这个思路，只要让不同句的词的距离超大，这样当两个词在不同句中，用上述词距范围搜索就无法命中。
* solr定义field的时候支持设置multiValued及positionIncrementGap属性，multiValued表示该域可以有多个值，positionIncrementGap表示给多个值之间插入的距离distance。
* 这样的话，索引的时候把文章按句或段拆成多个值，将多个值之间插入足够大的距离，然后再利用词距范围搜索，就可以实现同句/同段搜索了。

### 实现
**以实现同句搜索为例**
1. 在schema.xml中定义一个multiValued fieldType及相应的field， 并将positionIncrementGap设置到足够大。
```xml
<!-- for同句搜索 -->
<fieldType name="text_cn_gap1000" class="solr.TextField" positionIncrementGap="1000" multiValued="true">
 <analyzer>
   <tokenizer class="solr.HMMChineseTokenizerFactory"/>
   <filter class="solr.StopFilterFactory" words="stopwords.txt"/>
   <filter class="solr.PorterStemFilterFactory"/>
 </analyzer>
</fieldType>
<field name="bodySentences" type="text_cn_gap1000" indexed="true" stored="false"/>
```

2. 在数据导入时，利用RegexTransformer将正文拆成多个值。
```xml
<entity name="item" transformer="RegexTransformer" query="select id, body from table">
	<field column="bodySentences" splitBy="。" sourceColName="body"/>
</entity> 
```

3. 在搜索的时候，加上的词距限制。
> `q:"中国北京"~1000`
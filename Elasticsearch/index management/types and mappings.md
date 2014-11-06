## 类型和映射 ##

在ES中的type代表的是一类相似的文档。就好像之前介绍的那样，type就像RDBMS中的table一样。

一个type包含了name信息以及mapping信息。对于mapping，它表达了这个type中的各种字段具有的属性，比如字段的类型是string，integer还是date等，以及这些字段是如何被Lucene索引和存储的。

### Lucene是如何看待文档的 ###

Lucene中的文档包含的是一个简单的field-value列表。一个字段至少要有一个值，但是任何字段都可以拥有多个值。类似的，一个字符串值也可以通过解析阶段而被转换为多个值。Lucene不管值是字符串类型，还是数值类型及什么别的类型 - 所有的值都会被同等对待为 "opaque bytes"。

当我们使用Lucene对文档进行索引时，每个字段的值都会被添加到倒排索引的对应字段中。原始值也可以被选择是否不作修改的而被保存到索引中，以此来方便后续获取。

### type是如何实现的 ###

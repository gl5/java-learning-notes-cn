## 数据重索引 ##

虽然你可以向索引中添加新的类型，或者像类型中添加新的字段，但是你不能添加新的解析器或者对现有字段进行修改。如果你这么做了，就会让已经索引的数据变的不正确，导致搜索不能正常的进行。

为已经存在的数据适用这些更改的最简单的方法就是重索引(Reindex)：新建一个拥有最新配置的索引，然后将所有旧索引中的数据拷贝到新的索引中。

`_source`字段的一个优势是在ES中你已经拥有了整个文档。你不需要通过数据库来重建你的索引，这种方法通常会更慢。

为了从旧索引中高效地对所有文档进行重索引，可以使用[scan和scroll](http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/scan-scroll.html)来批量地从旧索引中获取文档，然后使用[bulk API](http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/bulk.html)将它们添加到新索引中。

> **批量重索引**
> 
> 你可以同时执行多个重索引任务，但是你显然不想要它们的结果有任何重叠。可以根据`date`或者`timestamp`字段对重索引的任务进行划分，成为规模较小的任务：
> 
> ```json
> GET /old_index/_search?search_type=scan&scroll=1m
> {
>     "query": {
>         "range": {
>             "date": {
>                 "gte":  "2014-01-01",
>                 "lt":   "2014-02-01"
>             }
>         }
>     },
>     "size":  1000
> }
> ```
> 
> 如果你正在持续地更改旧索引中的数据，想必你也希望这些更改也会被反映到新索引中。这可以通过再次运行重索引任务来完成，但是还是可以通过对日期字段进行过滤来得到在上次重索引开始后才被添加的文档。
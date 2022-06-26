### Elasticsearch

jdk16 对应的Elasticsearch7.12.x- 7.15.x

实验用的Elasticsearch7.12.x

9300端口为Elasticsearch集群建组件的通信端口，9200端口为浏览器访问的http协议RESTful端口

![image-20220625163837210](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220625163837210.png)

这里ES是以json格式处理和返回数据的。JavaScript object Notation

将上述文件命名为`obj`，则可以装入objs，var objs = [obj]

#### 数据格式

是面向文档型的数据库，一条数据库就是一个文档

![image-20220626150933786](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20220626150933786.png)





ES中不叫Index，叫索引，在postman中发出put请求，相当于在关系型数据库中**创建**一张表。

```http
#发送put请求，并创建一个shopping的表
#只允许 [GET/PUT/HEAD/DELETE]
http://127.0.0.1:9200/shopping
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "shopping"
}
```

创建索引后进行get**方式查询**索引，代码如下：

```json
#发送GET请求，并查询一个shopping的表
http://127.0.0.1:9200/shopping
{
    "shopping": {
        "aliases": {},
        "mappings": {},
        "settings": {
            "index": {
                "routing": {
                    "allocation": {
                        "include": {
                            "_tier_preference": "data_content"
                        }
                    }
                },
                "number_of_shards": "1",
                "provided_name": "shopping",
                "creation_date": "1656228285862",
                "number_of_replicas": "1",
                "uuid": "7ae8wSVVSsGexR3xPVMcJg",
                "version": {
                    "created": "7120099"
                }
            }
        }
    }
}
```

查看所有索引：`get http://127.0.0.1:9200/indices?v`

```mysql
health status index    uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   shopping 7ae8wSVVSsGexR3xPVMcJg   1   1          0            0       208b           208b
```

删除所有索引：`delete http://127.0.0.1:9200/shopping`

```json
{
    "acknowledged": true
}
```

### 文档操作

#### 创建文档

1.创建文档，索引创建好之后，接下来可以创建文档，并添加数据。这里的文档可以类比于数据库中的表数据，添加的数据格式为json

在postman中，向es服务发送post请求，**要在body中指定json格式的文档**:

`post http://127.0.0.1:9200/shopping/_doc`

```json
{
    "titie":"小米手机",
    "category":"小米",
    "images":"http://www.gulixueyuan.com/xm.jpg",
    "price":3999.00
}
```

POST完成后，所返回的值：

```json
{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "6TMJn4EBh_8vbBZL7wSt",  #这里是唯一标识
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 0,
    "_primary_term": 1
}
```

幂等性：也可以多次指定要发送的url后接的参数 1001 【比如】,因此创建的结果是一样的。

**补充一点，PUT方法用于更新数据，post方法用于创建数据。**

**但是在HTTP规范中，POST是非等幂的，多次post会造成不同的结果，比如:创建一个用户,由于网络原因或是其他原因多创建了几次,那么将会有多个用户被创建.而PUT id/456则会创建一个id为456的用户,多次调用还是会创建的结果是一样的,所以PUT是等幂的.**

`post http://127.0.0.1:9200/shopping/_doc/1001`

####  查询文档

以GET请求查询文档，结果如下：

1.`GET http://127.0.0.1:9200/shopping/_doc/1001`，这里可以把1001当做主键

```json
{
    "_index": "shopping",
    "_type": "_doc",
    "_id": "1001",
    "_version": 1,
    "_seq_no": 1,
    "_primary_term": 1,
    "found": true,
    "_source": {
        "titie": "小米手机",
        "category": "小米",
        "images": "http://www.gulixueyuan.com/xm.jpg",
        "price": 3999.00
    }
}
```

2.`GET http://127.0.0.1:9200/shopping/_search`，这里把下划线当做主键

```json
{
    "took": 41,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 2,
            "relation": "eq"
        },
        "max_score": 1.0,
        "hits": [
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "6TMJn4EBh_8vbBZL7wSt",
                "_score": 1.0,
                "_source": {
                    "titie": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999.00
                }
            },
            {
                "_index": "shopping",
                "_type": "_doc",
                "_id": "1001",
                "_score": 1.0,
                "_source": {
                    "titie": "小米手机",
                    "category": "小米",
                    "images": "http://www.gulixueyuan.com/xm.jpg",
                    "price": 3999.00
                }
            }
        ]
    }
}
```






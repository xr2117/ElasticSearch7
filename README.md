[TOC]

## ElasticSerach 7 教程

- 作者：Crazy.X
- QQ：200538725
- 微信：Xri2117
- Github：[SpringCloud入门教程](https://github.com/xr2117/xri-smart)
- Github：[ElasticSearch入门教程](https://github.com/xr2117/ElasticSearch7)
- CSDN：https://blog.csdn.net/weixin_38880770/article/details/103002456
- 若小伙伴们喜欢教程的麻烦给点个star 非常感谢，有什么需要的教程可以联系我，有时间我会一一送上
- 作者有Java技术群，想加入Java群的可以加微信，备注消息Java拉入群聊

### 一、 ElasticSerach 认识

#### 1.1 索引 index
- 一个索引可以理解成一个关系数据库的库

#### 1.2 类型 type
- 一种type就像一类表，比如user表，order表

#### 1.3 映射 mapping
-  mapping定义了每个字段的类型等信息。相当于关系型数据库中的表结

#### 1.4 文档 document
- 一个document相当于关系型数据库中的一⾏行行记录

#### 1.5 字段 field
- 相当于关系型数据库表的字段

#### 1.6 集群 cluster
-集群由一个或多个节点组成，一个集群有一个默认名称"elasticsearch"

#### 1.7 节点 node
- 集群的节点，一台机器 或者一个进程

#### 1.8 分片和副本 node
- 副本是分片的副本。分⽚有主分片(primary Shard)和副本分片(replica Shard)之分。Index数据在物理理上被分布在多个主分片中，每个主分片只存放部分数据。每个主分⽚可以有多个副本，叫副本分片，是主分片的复制。

#### 1.9 核心数据类型

##### 1.9.1 字符串: 

- text
1. 用于全文索引，该类型的字段将通过分词器 进⾏分词

- keyword
1. 不分词，只能搜索该字段的完整的值

##### 1.9.2 数值型

1. long, integer, short, byte, double, float, half_float, scaled_float

##### 1.9.3 数值型 boolean

- boolean

##### 1.9.3 二进制

- binary
1. 该类型的字段把值当做经过 base64 编码的字符串，默认不存储，且不可搜索

##### 1.9.4 范围类型

1. 范围类型表示值是一个范围，而不是⼀个具体的值
2. integer_range, float_range, long_range, double_range, date_range
3. 譬如 age 的类型是 integer_range，那么值可以是 {"gte" : 20, "lte" : 40}；搜索 "term" : {"age": 21} 可以搜索该值

##### 1.9.5 日期

- date

1. 由于Json没有date类型，所以es通过识别字符串是否符合format定义的格式来判断是否为date类型
2. format默认为：strict_date_optional_time||epoch_millis
3. "2022-01-01" "2022/01/01 12:10:30" 这种字符串格式
### 二、索引基本操作

#### 2.1 创建索引 PUT请求

- 请求
```java
localhost:9200/nba
```

- 响应
```java
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "nba"
}
```

#### 2.2 查看索引 GET请求

- 请求
```java
localhost:9200/nba
```

- 响应
```json
{
    "nba": {
        "aliases": {},  // 别名
        "mappings": {}, // 表结构
        "settings": {   // 索引设置
            "index": {  // 创建时间
                "creation_date": "1573278626713",
                "number_of_shards": "1",    // 分片数量
                "number_of_replicas": "1",  // 副本数量
                "uuid": "eeQmIsZ8Tl-GJ-xpFuOirg",   // UUID 索引的唯一ID
                "version": {
                    "created": "7020199"
                },
                "provided_name": "nba"
            }
        }
    }
}
```

#### 2.4 删除索引 DELETE请求

- 请求
```java
localhost:9200/nba
```

- 响应
```json
{
    "acknowledged": true
}
```

#### 2.5 批量获取索引 GET请求

- 请求
```java
localhost:9200/cba,nba
```

-响应
```json
{
    "cba": {
        "aliases": {},
        "mappings": {},
        "settings": {
            "index": {
                "creation_date": "1573281458107",
                "number_of_shards": "1",
                "number_of_replicas": "1",
                "uuid": "ikxZrzk2TVqQn7zRi2_glw",
                "version": {
                    "created": "7020199"
                },
                "provided_name": "cba"
            }
        }
    },
    "nba": {
        "aliases": {},
        "mappings": {},
        "settings": {
            "index": {
                "creation_date": "1573281355145",
                "number_of_shards": "1",
                "number_of_replicas": "1",
                "uuid": "hkhv1WKSQqWil3P9UXt3Aw",
                "version": {
                    "created": "7020199"
                },
                "provided_name": "nba"
            }
        }
    }
}
```

#### 2.6 获取全部索引 GET请求

- 请求
```java
localhost:9200/_all
```

-响应
```json
}
    }
    "cba": {
        "aliases": {},
        "mappings": {},
        "settings": {
            "index": {
                "creation_date": "1573281458107",
                "number_of_shards": "1",
                "number_of_replicas": "1",
                "uuid": "ikxZrzk2TVqQn7zRi2_glw",
                "version": {
                    "created": "7020199"
                },
                "provided_name": "cba"
            }
        }
    },
    "nba": {
        "aliases": {},
        "mappings": {},
        "settings": {
            "index": {
                "creation_date": "1573281355145",
                "number_of_shards": "1",
                "number_of_replicas": "1",
                "uuid": "hkhv1WKSQqWil3P9UXt3Aw",
                "version": {
                    "created": "7020199"
                },
                "provided_name": "nba"
            }
        }
    }
```
#### 2.7 使用_cat获取全部索引 GET请求

- 请求
```java
localhost:9200/_cat/indices?v
```

-响应
```text
health status index                uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .kibana_task_manager YA0slSRkRJqrE_jF0aOlFA   1   0          2            0     45.5kb         45.5kb
yellow open   cba                  ikxZrzk2TVqQn7zRi2_glw   1   1          0            0       230b           230b
yellow open   nba                  hkhv1WKSQqWil3P9UXt3Aw   1   1          0            0       283b           283b
green  open   .kibana_1            579wwXgCQJKJU1Ge7cJVXw   1   0          4            1       24kb           24kb

```

#### 2.8 判断索引是否存在 HEAD请求

- 请求
```java
localhost:9200/nba
```

-响应
```text
状态码"200"则为存在，不存在则为"404"
```

#### 2.9 关闭索引 不删除 POST请求

- 请求
```java
localhost:9200/nba/_close
```

-响应
```json
{
    "acknowledged": true,
    "shards_acknowledged": true
}
```

#### 2.10 打开索引 POST请求

- 请求
```java
localhost:9200/nba/_open
```

-响应
```json
{
    "acknowledged": true,
    "shards_acknowledged": true
}
```

### 三、映射的介绍与使用

- type: text 可分词
- type: keyword 不可分词

#### 3.1 创建Mapping PUT请求

- 请求
```java
localhost:9200/nba/_mapping
```

- 请求体
```json
{
	"properties":{	//字段的信息
		"name":{
			"type":"text"
		},
		"team_name":{
			"type":"text"
		},
		"position":{
			"type":"keyword"
		},
		"play_year":{
			"type":"keyword"
		},
		"jerse_no":{
			"type":"keyword"
		}
	}
}
```

-响应
```json
{
    "acknowledged": true
}
```

#### 3.2 查看Mapping信息 GET请求

- 请求
```java
localhost:9200/nba/_mapping
```

- 响应
```json
{
    "nba": {
        "mappings": {
            "properties": {
                "jerse_no": {
                    "type": "keyword"
                },
                "name": {
                    "type": "text"
                },
                "play_year": {
                    "type": "keyword"
                },
                "position": {
                    "type": "keyword"
                },
                "team_name": {
                    "type": "text"
                }
            }
        }
    }
}
```

#### 3.3 批量获取Mapping信息 GET请求

- 请求
```java
localhost:9200/nba,cba/_mapping
```

- 响应
```json
{
    "nba": {
        "mappings": {
            "properties": {
                "jerse_no": {
                    "type": "keyword"
                },
                "name": {
                    "type": "text"
                },
                "play_year": {
                    "type": "keyword"
                },
                "position": {
                    "type": "keyword"
                },
                "team_name": {
                    "type": "text"
                }
            }
        }
    },
    "cba": {
        "mappings": {}
    }
}
```

#### 3.4 获取所有Mapping信息第一种方式 GET请求

- 请求
```java
localhost:9200/_mapping
```

- 响应
```json
{
    "nba": {
        "mappings": {
            "properties": {
                "jerse_no": {
                    "type": "keyword"
                },
                "name": {
                    "type": "text"
                },
                "play_year": {
                    "type": "keyword"
                },
                "position": {
                    "type": "keyword"
                },
                "team_name": {
                    "type": "text"
                }
            }
        }
    },
    "cba": {
        "mappings": {}
    }
}
```

#### 3.5 获取所有Mapping信息第二种方式 GET请求

- 请求
```java
localhost:9200/_all/_mapping
```

- 响应
```json
{
    "nba": {
        "mappings": {
            "properties": {
                "jerse_no": {
                    "type": "keyword"
                },
                "name": {
                    "type": "text"
                },
                "play_year": {
                    "type": "keyword"
                },
                "position": {
                    "type": "keyword"
                },
                "team_name": {
                    "type": "text"
                }
            }
        }
    },
    "cba": {
        "mappings": {}
    }
}
```

#### 3.6 增加Mapping字段 POST请求

- Mapping 只可增加字段不可修改字段

- 请求
```java
localhost:9200/nba/_mapping
```

- 请求体
```json
{
	"properties":{	
		"name":{
			"type":"text"
		},
		"team_name":{
			"type":"text"
		},
		"position":{
			"type":"keyword"
		},
		"play_year":{
			"type":"keyword"
		},
		"jerse_no":{
			"type":"keyword"
		},
		"country":{  // 增加的国家字段
			"type":"keyword"
		}
	}
}
```

- 响应
```json
{
    "acknowledged": true
}
```

### 四、文档的增删改查

#### 4.1 新增文档 指定ID PUT/POST请求

- 请求
```java
localhost:9200/nba/_doc/1
```

- 请求体
```json
{
	"name":"哈登",
	"team_name":"火箭",
	"position":"得分后卫",
	"play_year":"10",
	"jerse_no":"13"
}
```

- 响应
```json
{
    "_index": "nba",
    "_type": "_doc",
    "_id": "1", //文档的ID
    "_version": 1,
    "result": "created", // 响应结果
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 0,
    "_primary_term": 1
}
```

#### 4.2 新增文档 自动生成ID POST请求

- 注意: 不指定IP只能使用POST请求
- 注意: 自动生成ID开关要打开,关闭状态无法自动创建ID

- 请求
```java
localhost:9200/nba/_doc
```

- 请求体
```json
{
	"name":"库里",
	"team_name":"勇士",
	"position":"组织后卫",
	"play_year":"10",
	"jerse_no":"30"
}
```

- 响应
```json
{
    "_index": "nba",
    "_type": "_doc",
    "_id": "7PkGT24BeuZ7t7g8CXe-", // 自动生成ID
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 3,
    "_primary_term": 1
}
```

#### 4.3 自动创建索引 POST请求

- 查看auto_create_index开关状态，请求http://localhost:9200/_cluster/settings

- 注意:当索引不存在并且auto_create_index为true的时候，新增文档时会⾃动创建索引，若为false是不能自动创建索引

- 请求
```java
localhost:9200/wnba/_doc/1
```

- 请求体
```json
{
	"name":"周琦",
	"team_name":"波兰国家队",
	"position":"中锋",
	"play_year":"3",
	"jerse_no":"9"
}
```

- 响应
```json
{
    "_index": "wnba", // 自动创建的索引
    "_type": "_doc",
    "_id": "1",
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

#### 4.4 查看自动创建的索引 GET请求

- 请求
```java
localhost:9200/wnba
```

- 响应
```json
{
    "wnba": {
        "aliases": {},
        "mappings": {
            "properties": {
                "jerse_no": {
                    "type": "text",
                    "fields": {
                        "keyword": {
                            "type": "keyword",
                            "ignore_above": 256
                        }
                    }
                },
                "name": {
                    "type": "text",
                    "fields": {
                        "keyword": {
                            "type": "keyword",
                            "ignore_above": 256
                        }
                    }
                },
                "play_year": {
                    "type": "text",
                    "fields": {
                        "keyword": {
                            "type": "keyword",
                            "ignore_above": 256
                        }
                    }
                },
                "position": {
                    "type": "text",
                    "fields": {
                        "keyword": {
                            "type": "keyword",
                            "ignore_above": 256
                        }
                    }
                },
                "team_name": {
                    "type": "text",
                    "fields": {
                        "keyword": {
                            "type": "keyword",
                            "ignore_above": 256
                        }
                    }
                }
            }
        },
        "settings": {
            "index": {
                "creation_date": "1573284418718",
                "number_of_shards": "1",
                "number_of_replicas": "1",
                "uuid": "fKs9KZ11R3-_zgKi8WFQTQ",
                "version": {
                    "created": "7020199"
                },
                "provided_name": "wnba"
            }
        }
    }
}
```

#### 4.5 指定操作类型

- 新增或修改的时候可能会把原有文档修改掉，这里可以指定类型
- 比如文档存在，我要新增一条文档，但是没有指定类型可能修改掉原有的文档

- 请求
```java
localhost:9200/nba/_doc/1?op_type=create
```

- 请求体
```json
{
	"name":"周琦",
	"team_name":"波兰国家队",
	"position":"中锋",
	"play_year":"3",
	"jerse_no":"9"
}
```

- 响应
```json
{
    "error": {
        "root_cause": [
            {
                "type": "version_conflict_engine_exception",
                // 文档已经存在
                "reason": "[1]: version conflict, document already exists (current version [5])",
                "index_uuid": "hkhv1WKSQqWil3P9UXt3Aw",
                "shard": "0",
                "index": "nba"
            }
        ],
        "type": "version_conflict_engine_exception",
        "reason": "[1]: version conflict, document already exists (current version [5])",
        "index_uuid": "hkhv1WKSQqWil3P9UXt3Aw",
        "shard": "0",
        "index": "nba"
    },
    "status": 409
}
```

#### 4.6 查看指定ID文档 GET请求

- 请求
```java
localhost:9200/nba/_doc/1
```

- 响应
```json
{
	"_index": "nba",
	"_type": "_doc",
	"_id": "1",
	"_version": 5,
	"_seq_no": 5,
	"_primary_term": 1,
	"found": true,
	"_source": {
		"name": "哈登",
		"team_name": "火箭",
		"position": "得分后卫",
		"play_year": "10",
		"jerse_no": "13"
	}
}
```
#### 4.7 查看多条文档 第一种方式 GET/POST请求

- 请求
```java
localhost:9200/_mget
```

- 请求体
```json
{
	"docs" : [ // 指定标签
		{
			"_index" : "nba", // 指定索引
			"_type" : "_doc", // 默认类型
			"_id" : "1"       // 指定ID
		},
		{
			"_index" : "nba",
			"_type" : "_doc",
			"_id" : "2"
		}
	]
}

```

- 响应
```json
{
	"docs": [
		{
			"_index": "nba",
			"_type": "_doc",
			"_id": "1",
			"_version": 5,
			"_seq_no": 5,
			"_primary_term": 1,
			"found": true,
			"_source": {
				"name": "哈登",
				"team_name": "火箭",
				"position": "得分后卫",
				"play_year": "10",
				"jerse_no": "13"
			}
		},
		{
			"_index": "nba",
			"_type": "_doc",
			"_id": "2",
			"found": false
		}
	]
}
```
#### 4.8 查看多条文档 第二种方式 GET/POST请求

- 请求
```java
localhost:9200/nba/_mget // 先指定索引
```

- 请求体
```json
{
	"docs" : [
		{
			"_type" : "_doc",
			"_id" : "1"
		},
		{
			"_type" : "_doc",
			"_id" : "2"
		}
	]
}
```

- 响应
```json
{
	"docs": [
		{
			"_index": "nba",
			"_type": "_doc",
			"_id": "1",
			"_version": 5,
			"_seq_no": 5,
			"_primary_term": 1,
			"found": true,
			"_source": {
				"name": "哈登",
				"team_name": "火箭",
				"position": "得分后卫",
				"play_year": "10",
				"jerse_no": "13"
			}
		},
		{
			"_index": "nba",
			"_type": "_doc",
			"_id": "2",
			"found": false
		}
	]
}
```

#### 4.9 查看多条文档 第三种方式 GET/POST请求

- 请求
```java
localhost:9200/nba/_doc/_mget    //指定索引、类型
```

- 请求体
```json
{
	"docs" : [
		{
			"_id" : "1"
		},
		{
			"_id" : "2"
		}
	]
}
```

- 响应
```json
{
	"docs": [
		{
			"_index": "nba",
			"_type": "_doc",
			"_id": "1",
			"_version": 5,
			"_seq_no": 5,
			"_primary_term": 1,
			"found": true,
			"_source": {
				"name": "哈登",
				"team_name": "火箭",
				"position": "得分后卫",
				"play_year": "10",
				"jerse_no": "13"
			}
		},
		{
			"_index": "nba",
			"_type": "_doc",
			"_id": "2",
			"found": false
		}
	]
}
```

#### 4.10 查看多条文档 第四种方式 GET/POST请求

- 请求
```java
localhost:9200/nba/_doc/_mget
```

- 请求体
```json
{
	"ids":["1","2"]
}
```

- 响应
```json
{
	"docs": [
		{
			"_index": "nba",
			"_type": "_doc",
			"_id": "1",
			"_version": 5,
			"_seq_no": 5,
			"_primary_term": 1,
			"found": true,
			"_source": {
				"name": "哈登",
				"team_name": "火箭",
				"position": "得分后卫",
				"play_year": "10",
				"jerse_no": "13"
			}
		},
		{
			"_index": "nba",
			"_type": "_doc",
			"_id": "2",
			"found": false
		}
	]
}
```

#### 4.11 修改文档 POST请求

- 根据提供的文档片段更新数据

- 请求
```java
localhost:9200/nba/_update/1
```

- 请求体
```json
{
	"doc": { // doc标签必须存在
			"name": "哈登",
			"team_name": "火箭",
			"position": "双能卫",
			"play_year": "10",
			"jerse_no": "13"
	}
}
```

- 响应
```json
{
    "_index": "nba",
    "_type": "_doc",
    "_id": "1",
    "_version": 7,
    "result": "updated",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 7,
    "_primary_term": 1
}
```
#### 4.12 向_source增加字段 POST请求

- 请求
```java
localhost:9200/nba/_update/1
```

- 请求体
```json
{
    // script:标签 ctx:上下文 ._source = _source
    // 语义:通过上下文拿到 _source字段,新增age为18
	"script": "ctx._source.age = 18"
}
```

- 响应
```json
{
    "_index": "nba",
    "_type": "_doc",
    "_id": "1",
    "_version": 8,
    "result": "updated",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 8,
    "_primary_term": 1
}
```
#### 4.13 向_source删除字段 POST请求

- 请求
```java
localhost:9200/nba/_update/1
```

- 请求体
```json
{
    // json格式无法出现多个" 所以需要转义符
	"script": "ctx._source.remove(\"age\")"
}
```

- 响应
```json
{
    "_index": "nba",
    "_type": "_doc",
    "_id": "1",
    "_version": 9,
    "result": "updated",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 9,
    "_primary_term": 1
}
```
#### 4.14 更新指定文档的字段 POST请求

- 请求
```java
localhost:9200/nba/_update/1
```

- 请求体
```json
{
	"script": {
        // 先获取ID为1的数据，之后进行age+4
		"source": "ctx._source.age += params.age",
        // 指定参数
		"params": {
			"age": 4
		}
	},
    // 若存在则修改,若不存在则新增
	"upsert":{
		"age": 1
	}
}
```

- 响应
```json
{
    "_index": "nba",
    "_type": "_doc",
    "_id": "1",
    "_version": 11,
    "result": "updated",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 11,
    "_primary_term": 1
}
```
- 查询更新结果

- 请求
```java
localhost:9200/nba/_doc/1
```
- 响应
```json
{
    "_index": "nba",
    "_type": "_doc",
    "_id": "1",
    "_version": 11,
    "_seq_no": 11,
    "_primary_term": 1,
    "found": true,
    "_source": {
        "name": "大胡子",
        "team_name": "火箭",
        "position": "双能卫",
        "play_year": "10",
        "jerse_no": "13",
        "age": 22
    }
}
```

#### 4.15 upsert介绍 
- upsert 当指定的⽂文档不不存在时，upsert参数包含的内容将会被插⼊入到索引中，作为⼀一个新⽂文档；如果指定的⽂文档存在，ElasticSearch引擎将会执⾏行行指定的更更新逻辑

- 请求
```java
localhost:9200/nba/_update/3
```

- 请求体
```json
{
	"script": {
		"source": "ctx._source.allstar += params.allstar",
		"params": {
			"allstar": 4
		}
	},
	"upsert": {
		"allstar": 1
	}
}

```

- 响应
```json
{
    "_index": "nba",
    "_type": "_doc",
    "_id": "3",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 12,
    "_primary_term": 1
}
```
- 查询更新结果upsert结果

- 请求
```java
localhost:9200/nba/_doc/3
```

- 响应
```json
{
    "_index": "nba",
    "_type": "_doc",
    "_id": "3",
    "_version": 1,
    "_seq_no": 12,
    "_primary_term": 1,
    "found": true,
    "_source": {
        "allstar": 1
    }
}
```

#### 4.16 删除文档 DELETE请求

- 请求
```java
localhost:9200/nba/_doc/3
```

- 响应
```json
{
    "_index": "nba",
    "_type": "_doc",
    "_id": "3",
    "_version": 2,
    "result": "deleted",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 13,
    "_primary_term": 1
}
```

### 五、文档的搜索
- term(词条)查询和full text(全⽂文)查询

- 词条查询：词条查询不会分析查询条件，只有当词条和查询字符串完全匹配时，才匹配搜索

- 全⽂查询：ElasticSearch引擎会先分析查询字符串，将其拆分成多个分词，只要已分析的字段中包含词条的任意⼀个，或全部包含，就匹配查询条件，返回该⽂档；如果不包含任意一个分词，表示没有任何文档匹配查询条件

#### 5.1 单条trem查询 GET/POST请求

- term 关键字查询，精确查询

- 例如Sql的where条件

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{   
    // 语义: 查询词条球号为23
	"query":{   // 查询
	    "term":{    // 词条
	    	"jerse_no":"23" // 字段名称
		}
	}
}
```

- 响应
```json
{
	"took": 2,
	"timed_out": false,
	"_shards": {
		"total": 1,
		"successful": 1,
		"skipped": 0,
		"failed": 0
	},
	"hits": {
		"total": {
			"value": 1,
			"relation": "eq"
		},
		"max_score": 0.9808292,
		"hits": [
			{
				"_index": "nba",
				"_type": "_doc",
				"_id": "3",
				"_score": 0.9808292,
				"_source": {
					"name": "詹姆斯",
					"team_name": "湖人",
					"position": "小前锋",
					"play_year": "15",
					"jerse_no": "23"
				}
			}
		]
	}
}
```

#### 5.2 多条trem查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
	"query":{
		"terms":{ // 这里使用terms
			"jerse_no": ["23","13"]
		}
	}
}
```

- 响应
```json
{
	"took": 0, // 消耗时间
	"timed_out": false, // 是否超时
	"_shards": { // 分片
		"total": 1, // 分片总体 1 
		"successful": 1, // 成功 1
		"skipped": 0,
		"failed": 0
	},
	"hits": { // 命中
		"total": { // 查询总数
			"value": 2, // 2条
			"relation": "eq"
		},
		"max_score": 1.0, // 最大分数 返回结果按照分数最大排序
		"hits": [
			{
				"_index": "nba",
				"_type": "_doc",
				"_id": "1",
				"_score": 1.0,
				"_source": {
					"name": "哈登",
					"team_name": "火箭",
					"position": "得分后卫",
					"play_year": "10",
					"jerse_no": "13"
				}
			},
			{
				"_index": "nba",
				"_type": "_doc",
				"_id": "3",
				"_score": 1.0,
				"_source": {
					"name": "詹姆斯",
					"team_name": "湖人",
					"position": "小前锋",
					"play_year": "15",
					"jerse_no": "23"
				}
			}
		]
	}
}
```

####  5.2 match_all查询 GET/POST请求

- 全文查询

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
	"query":{
		"match_all":{} // 查询全部文档，默认显示10条记录
	},
	"from": 0, // 从0开始
	"size": 100 // 查询100条
}
```

- 响应
```json
{
	"took": 0,
	"timed_out": false,
	"_shards": {
		"total": 1,
		"successful": 1,
		"skipped": 0,
		"failed": 0
	},
	"hits": {
		"total": {
			"value": 3,
			"relation": "eq"
		},
		"max_score": 1.0,
		"hits": [
			{
				"_index": "nba",
				"_type": "_doc",
				"_id": "1",
				"_score": 1.0,
				"_source": {
					"name": "哈登",
					"team_name": "火箭",
					"position": "得分后卫",
					"play_year": "10",
					"jerse_no": "13"
				}
			},
			{
				"_index": "nba",
				"_type": "_doc",
				"_id": "2",
				"_score": 1.0,
				"_source": {
					"name": "库里",
					"team_name": "勇士",
					"position": "控球后卫",
					"play_year": "10",
					"jerse_no": "30"
				}
			},
			{
				"_index": "nba",
				"_type": "_doc",
				"_id": "3",
				"_score": 1.0,
				"_source": {
					"name": "詹姆斯",
					"team_name": "湖人",
					"position": "小前锋",
					"play_year": "15",
					"jerse_no": "23"
				}
			}
		]
	}
}
```
####  5.3 match查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
	"query":{
		"match":{ // 这里使用match
			"name": "库小里" //name:字段 会进行分词匹配
		}
	},
	"from": 0,
	"size": 100
}
```

- 响应
```json
{
	"took": 1,
	"timed_out": false,
	"_shards": {
		"total": 1,
		"successful": 1,
		"skipped": 0,
		"failed": 0
	},
	"hits": {
		"total": {
			"value": 1,
			"relation": "eq"
		},
		"max_score": 2.0834165,
		"hits": [
			{
				"_index": "nba",
				"_type": "_doc",
				"_id": "2",
				"_score": 2.0834165,
				"_source": {
					"name": "库里",
					"team_name": "勇士",
					"position": "控球后卫",
					"play_year": "10",
					"jerse_no": "30"
				}
			}
		]
	}
}
```

####  5.4 multi_match 多个查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
	"query":{
		"multi_match":{ // 选定多个字段所以使用multi_match
			"query": "shooter",
			"fields": ["title","name"]  // 指定字段
		}
	}
}
```

- 响应
```json
{
    "took": 1,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 0.18232156,
        "hits": [
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "2",
                "_score": 0.18232156,
                "_source": {
                    "name": "库里",
                    "team_name": "勇士",
                    "position": "控球后卫",
                    "play_year": 10,
                    "jerse_no": "30",
                    "title": "the best shooter"
                }
            }
        ]
    }
}
```

####  5.4 match_phrase 多个查询 GET/POST请求

- 准确查询 类似词条查询

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
	"query":{
		"match_phrase":{
			"position": "得分后卫"
		}
	}
}
```

- 响应
```json
{
	"took": 3,
	"timed_out": false,
	"_shards": {
		"total": 1,
		"successful": 1,
		"skipped": 0,
		"failed": 0
	},
	"hits": {
		"total": {
			"value": 1,
			"relation": "eq"
		},
		"max_score": 3.277387,
		"hits": [
			{
				"_index": "nba",
				"_type": "_doc",
				"_id": "1",
				"_score": 3.277387,
				"_source": {
					"name": "哈登",
					"team_name": "火箭",
					"position": "得分后卫",
					"play_year": "10",
					"jerse_no": "13"
				}
			}
		]
	}
}
```

####  5.5 match_phrase_profix 多个查询 GET/POST请求

- 可以增加前缀

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
	"query":{
		"match_phrase_prefix":{
			"title": "the" // 指定了前缀
		}
	}
}
```

- 响应
```json
{
    "took": 1,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 0.2876821,
        "hits": [
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "2",
                "_score": 0.2876821,
                "_source": {
                    "name": "库里",
                    "team_name": "勇士",
                    "position": "控球后卫",
                    "play_year": 10,
                    "jerse_no": "30",
                    "title": "the best shooter"
                }
            }
        ]
    }
}
```

### 六、ElasticSerach 搜索

#### 6.1 批量导入数据

- Bulkl
1. ES提供了Bulk的API 来进行批量操作

- 数据结构类型，以换行区分

```json
// 必须有一个索引叫book 
{"index": {"_index": "book", "_type": "_doc", "_id": 1}}
{"name": "权力的游戏"}
{"index": {"_index": "book", "_type": "_doc", "_id": 2}}
{"name": "疯狂的石头"}
 // 结尾必须有换行
```

- 请求
```java
curl -X POST "localhost:9200/_bulk" -H 'Content-Type: application/json' --data-binary @name // name就是文件的路径和名字
```

- 测试 查询
```json
{
	"query":{
		"match_all":{} 
	},
	"from": 0, 
	"size": 100
}
```

#### 6.2 trem多种查询

- 单词级别查询

1. 这些查询通常⽤用于结构化的数据，⽐比如：number, date, keyword等，而不是对text。
2. 也就是说，全⽂本查询之前要先对⽂本内容进⾏分词，⽽单词级别的查询直接在相应字段的反向索引中精确查找，单词级别的查询⼀般⽤于数值、⽇期等类型的字段上

- 准备索引 进行批量导入,数据在player中
```json
 {
 	"mappings":{
 		"properties":{
 			"birthDay":{
 				"type":"date"
 				
 			},
 			"birthDayStr": {
 				"type":"keyword"
 				
 			},
 			"age":{
 				"type":"integer"
 				
 			},
 			"code": {
 				"type":"text"
 				
 			},
 			"country":{
 				"type":"text"
 				
 			},
 			"countryEn": {
 				"type":"text"
 				
 			},
 			"displayAffiliation":{
 				"type":"text"
 				
 			},
 			"displayName": {
 				"type":"text"
 				
 			},
 			"displayNameEn":{
 				"type":"text"
 				
 			},
 			"draft": {
 				"type":"long"
 				
 			},
 			"heightValue":{
 				"type":"float"
 				
 			},
 			"jerseyNo": {
 				"type":"text"
 				
 			},
 			"playYear":{
 				"type":"long"
 			},
 			"playerId": {
 				"type":"keyword"
 				
 			},
 			"position":{
 				"type":"text"
 				
 			},
 			"schoolType": {
 				"type":"text"
 				
 			},
 			"teamCity":{
 				"type":"text"
 				
 			},
 			"teamCityEn": {
 				"type":"text"
 				
 			},
 			"teamConference": {
 				"type":"keyword"
 				
 			},
 			"teamConferenceEn":{
 				"type":"keyword"
 				
 			},
 			"teamName": {
 				"type":"keyword"
 				
 			},
 			"teamNameEn":{
 				"type":"keyword"
 				
 			},
 			"weight": {
 				"type":"text"
 			}
 		}
 	}
 }
 ```

#####  6.2.1 term query 精准匹配查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{   
    // 语义:精确查找球衣23号数据并分页
  "query": {
    "term": {
      "jerseyNo": 23
    }
  },
  "from": 0,
  "size": 20
}
```

- 响应
```json
{
    "took": 0,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 11,
            "relation": "eq"
        },
        "max_score": 3.6167762,
        "hits": [
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "73",
                "_score": 3.6167762,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "雄鹿",
                    "birthDay": 792392400000,
                    "country": "美国",
                    "teamCityEn": "Milwaukee",
                    "code": "sterling_brown",
                    "displayAffiliation": "Southern Methodist/United States",
                    "displayName": "斯特林 布朗",
                    "schoolType": "College",
                    "teamConference": "东部",
                    "teamConferenceEn": "Eastern",
                    "weight": "105.2 公斤",
                    "teamCity": "密尔沃基",
                    "playYear": 2,
                    "jerseyNo": "23",
                    "teamNameEn": "Bucks",
                    "draft": 2017,
                    "displayNameEn": "Sterling Brown",
                    "heightValue": 1.98,
                    "birthDayStr": "1995-02-10",
                    "position": "后卫",
                    "age": 24,
                    "playerId": "1628425"
                }
            },
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "78",
                "_score": 3.6167762,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "独行侠",
                    "birthDay": 721544400000,
                    "country": "美国",
                    "teamCityEn": "Dallas",
                    "code": "trey_burke",
                    "displayAffiliation": "Michigan/United States",
                    "displayName": "特雷 伯克",
                    "schoolType": "College",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "79.4 公斤",
                    "teamCity": "达拉斯",
                    "playYear": 6,
                    "jerseyNo": "23",
                    "teamNameEn": "Mavericks",
                    "draft": 2013,
                    "displayNameEn": "Trey Burke",
                    "heightValue": 1.85,
                    "birthDayStr": "1992-11-12",
                    "position": "后卫",
                    "age": 27,
                    "playerId": "203504"
                }
            },
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "167",
                "_score": 3.6167762,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "雷霆",
                    "birthDay": 895377600000,
                    "country": "美国",
                    "teamCityEn": "Oklahoma City",
                    "code": "terrance_ferguson",
                    "displayAffiliation": "United States",
                    "displayName": "特伦斯 弗格森",
                    "schoolType": "",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "86.2 公斤",
                    "teamCity": "俄克拉荷马城",
                    "playYear": 2,
                    "jerseyNo": "23",
                    "teamNameEn": "Thunder",
                    "draft": 2017,
                    "displayNameEn": "Terrance Ferguson",
                    "heightValue": 2.01,
                    "birthDayStr": "1998-05-17",
                    "position": "后卫-前锋",
                    "age": 21,
                    "playerId": "1628390"
                }
            }
        ]
    }
}
```

#####  6.2.2 exsit query 在特定的字段中查找非空值的文档 GET/POST请求

- 请求
```java

```

- 请求体
```json
{
    // 语义:查找teamNameEn 不为空的数据
  "query": {
    "exists": {
      "field": "teamNameEn" 
    }
  },
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
    "took": 0,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 566,
            "relation": "eq"
        },
        "max_score": 1.0,
        "hits": [
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "1",
                "_score": 1.0,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "老鹰",
                    "birthDay": 831182400000,
                    "country": "美国",
                    "teamCityEn": "Atlanta",
                    "code": "jaylen_adams",
                    "displayAffiliation": "United States",
                    "displayName": "杰伦 亚当斯",
                    "schoolType": "College",
                    "teamConference": "东部",
                    "teamConferenceEn": "Eastern",
                    "weight": "86.2 公斤",
                    "teamCity": "亚特兰大",
                    "playYear": 1,
                    "jerseyNo": "10",
                    "teamNameEn": "Hawks",
                    "draft": 2018,
                    "displayNameEn": "Jaylen Adams",
                    "heightValue": 1.88,
                    "birthDayStr": "1996-05-04",
                    "position": "后卫",
                    "age": 23,
                    "playerId": "1629121"
                }
            },
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "2",
                "_score": 1.0,
                "_source": {
                    "countryEn": "New Zealand",
                    "teamName": "雷霆",
                    "birthDay": 743140800000,
                    "country": "新西兰",
                    "teamCityEn": "Oklahoma City",
                    "code": "steven_adams",
                    "displayAffiliation": "Pittsburgh/New Zealand",
                    "displayName": "斯蒂文 亚当斯",
                    "schoolType": "College",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "120.2 公斤",
                    "teamCity": "俄克拉荷马城",
                    "playYear": 6,
                    "jerseyNo": "12",
                    "teamNameEn": "Thunder",
                    "draft": 2013,
                    "displayNameEn": "Steven Adams",
                    "heightValue": 2.13,
                    "birthDayStr": "1993-07-20",
                    "position": "中锋",
                    "age": 26,
                    "playerId": "203500"
                }
            },
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "3",
                "_score": 1.0,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "热火",
                    "birthDay": 869198400000,
                    "country": "美国",
                    "teamCityEn": "Miami",
                    "code": "bam_adebayo",
                    "displayAffiliation": "Kentucky/United States",
                    "displayName": "巴姆 阿德巴约",
                    "schoolType": "College",
                    "teamConference": "东部",
                    "teamConferenceEn": "Eastern",
                    "weight": "115.7 公斤",
                    "teamCity": "迈阿密",
                    "playYear": 2,
                    "jerseyNo": "13",
                    "teamNameEn": "Heat",
                    "draft": 2017,
                    "displayNameEn": "Bam Adebayo",
                    "heightValue": 2.08,
                    "birthDayStr": "1997-07-18",
                    "position": "中锋-前锋",
                    "age": 22,
                    "playerId": "1628389"
                }
            }
        ]
    }
}
```

#####  6.2.3 prefix query 查找包含带有指定前缀term的文档 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义:查找teamNameEn 不为空的数据 teamNameEn必须为text类型
    // 使用前缀类型必须为keyword,不能为text
  "query": {
    "prefix": {
      "teamNameEn": "Rock"
    }
  },
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
    "took": 4,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 21,
            "relation": "eq"
        },
        "max_score": 1.0,
        "hits": [
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "86",
                "_score": 1.0,
                "_source": {
                    "countryEn": "Switzerland",
                    "teamName": "火箭",
                    "birthDay": 769233600000,
                    "country": "瑞士",
                    "teamCityEn": "Houston",
                    "code": "clint_capela",
                    "displayAffiliation": "Switzerland/Switzerland",
                    "displayName": "克林特 卡佩拉",
                    "schoolType": "",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "108.9 公斤",
                    "teamCity": "休斯顿",
                    "playYear": 5,
                    "jerseyNo": "15",
                    "teamNameEn": "Rockets",
                    "draft": 2014,
                    "displayNameEn": "Clint Capela",
                    "heightValue": 2.08,
                    "birthDayStr": "1994-05-18",
                    "position": "中锋",
                    "age": 25,
                    "playerId": "203991"
                }
            },
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "99",
                "_score": 1.0,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "火箭",
                    "birthDay": 816930000000,
                    "country": "美国",
                    "teamCityEn": "Houston",
                    "code": "chris_chiozza",
                    "displayAffiliation": "University of Florida/United States",
                    "displayName": "克里斯 Chiozza",
                    "schoolType": "",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "79.4 公斤",
                    "teamCity": "休斯顿",
                    "playYear": 1,
                    "jerseyNo": "2",
                    "teamNameEn": "Rockets",
                    "draft": 2018,
                    "displayNameEn": "Chris Chiozza",
                    "heightValue": 1.83,
                    "birthDayStr": "1995-11-21",
                    "position": "后卫",
                    "age": 24,
                    "playerId": "1629185"
                }
            },
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "101",
                "_score": 1.0,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "火箭",
                    "birthDay": 784962000000,
                    "country": "美国",
                    "teamCityEn": "Houston",
                    "code": "gary_clark",
                    "displayAffiliation": "University of Cincinnati/United States",
                    "displayName": "加里 克拉克",
                    "schoolType": "",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "102.1 公斤",
                    "teamCity": "休斯顿",
                    "playYear": 1,
                    "jerseyNo": "6",
                    "teamNameEn": "Rockets",
                    "draft": 2018,
                    "displayNameEn": "Gary Clark",
                    "heightValue": 2.03,
                    "birthDayStr": "1994-11-16",
                    "position": "前锋",
                    "age": 25,
                    "playerId": "1629109"
                }
            }
        ]
    }
}
```

#####  6.2.4 wildcard query 支持通配符查询 GET/POST请求

-  *表示任意字符，?表示任意单个字符,类似SQL模糊查询

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 查询teamNameEn为Ro*s *为模糊搜索
  "query": {
    "wildcard": {
      "teamNameEn": "Ro*s"
    }
  },
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
    "took": 1,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 21,
            "relation": "eq"
        },
        "max_score": 1.0,
        "hits": [
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "86",
                "_score": 1.0,
                "_source": {
                    "countryEn": "Switzerland",
                    "teamName": "火箭",
                    "birthDay": 769233600000,
                    "country": "瑞士",
                    "teamCityEn": "Houston",
                    "code": "clint_capela",
                    "displayAffiliation": "Switzerland/Switzerland",
                    "displayName": "克林特 卡佩拉",
                    "schoolType": "",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "108.9 公斤",
                    "teamCity": "休斯顿",
                    "playYear": 5,
                    "jerseyNo": "15",
                    "teamNameEn": "Rockets",
                    "draft": 2014,
                    "displayNameEn": "Clint Capela",
                    "heightValue": 2.08,
                    "birthDayStr": "1994-05-18",
                    "position": "中锋",
                    "age": 25,
                    "playerId": "203991"
                }
            },
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "99",
                "_score": 1.0,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "火箭",
                    "birthDay": 816930000000,
                    "country": "美国",
                    "teamCityEn": "Houston",
                    "code": "chris_chiozza",
                    "displayAffiliation": "University of Florida/United States",
                    "displayName": "克里斯 Chiozza",
                    "schoolType": "",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "79.4 公斤",
                    "teamCity": "休斯顿",
                    "playYear": 1,
                    "jerseyNo": "2",
                    "teamNameEn": "Rockets",
                    "draft": 2018,
                    "displayNameEn": "Chris Chiozza",
                    "heightValue": 1.83,
                    "birthDayStr": "1995-11-21",
                    "position": "后卫",
                    "age": 24,
                    "playerId": "1629185"
                }
            },
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "101",
                "_score": 1.0,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "火箭",
                    "birthDay": 784962000000,
                    "country": "美国",
                    "teamCityEn": "Houston",
                    "code": "gary_clark",
                    "displayAffiliation": "University of Cincinnati/United States",
                    "displayName": "加里 克拉克",
                    "schoolType": "",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "102.1 公斤",
                    "teamCity": "休斯顿",
                    "playYear": 1,
                    "jerseyNo": "6",
                    "teamNameEn": "Rockets",
                    "draft": 2018,
                    "displayNameEn": "Gary Clark",
                    "heightValue": 2.03,
                    "birthDayStr": "1994-11-16",
                    "position": "前锋",
                    "age": 25,
                    "playerId": "1629109"
                }
            }
        ]
    }
}
```

#####  6.2.5 regexp query 正则表达式查询 GET/POST请求

- 请求
```java

```

- 请求体
```json
{
    // 语义: 查询teamNameEn 为Ro.*s  .表示任意 *为多个
  "query": {
    "regexp": {
      "teamNameEn": "Ro.*s"
    }
  },
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
    "took": 0,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 21,
            "relation": "eq"
        },
        "max_score": 1.0,
        "hits": [
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "86",
                "_score": 1.0,
                "_source": {
                    "countryEn": "Switzerland",
                    "teamName": "火箭",
                    "birthDay": 769233600000,
                    "country": "瑞士",
                    "teamCityEn": "Houston",
                    "code": "clint_capela",
                    "displayAffiliation": "Switzerland/Switzerland",
                    "displayName": "克林特 卡佩拉",
                    "schoolType": "",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "108.9 公斤",
                    "teamCity": "休斯顿",
                    "playYear": 5,
                    "jerseyNo": "15",
                    "teamNameEn": "Rockets",
                    "draft": 2014,
                    "displayNameEn": "Clint Capela",
                    "heightValue": 2.08,
                    "birthDayStr": "1994-05-18",
                    "position": "中锋",
                    "age": 25,
                    "playerId": "203991"
                }
            },
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "99",
                "_score": 1.0,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "火箭",
                    "birthDay": 816930000000,
                    "country": "美国",
                    "teamCityEn": "Houston",
                    "code": "chris_chiozza",
                    "displayAffiliation": "University of Florida/United States",
                    "displayName": "克里斯 Chiozza",
                    "schoolType": "",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "79.4 公斤",
                    "teamCity": "休斯顿",
                    "playYear": 1,
                    "jerseyNo": "2",
                    "teamNameEn": "Rockets",
                    "draft": 2018,
                    "displayNameEn": "Chris Chiozza",
                    "heightValue": 1.83,
                    "birthDayStr": "1995-11-21",
                    "position": "后卫",
                    "age": 24,
                    "playerId": "1629185"
                }
            },
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "101",
                "_score": 1.0,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "火箭",
                    "birthDay": 784962000000,
                    "country": "美国",
                    "teamCityEn": "Houston",
                    "code": "gary_clark",
                    "displayAffiliation": "University of Cincinnati/United States",
                    "displayName": "加里 克拉克",
                    "schoolType": "",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "102.1 公斤",
                    "teamCity": "休斯顿",
                    "playYear": 1,
                    "jerseyNo": "6",
                    "teamNameEn": "Rockets",
                    "draft": 2018,
                    "displayNameEn": "Gary Clark",
                    "heightValue": 2.03,
                    "birthDayStr": "1994-11-16",
                    "position": "前锋",
                    "age": 25,
                    "playerId": "1629109"
                }
            }
        ]
    }
}
```

#####  6.2.6 ids query 通过id批量查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
  "query": {
    "ids": {
      "values": [1,2,3]
    }
  },
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
    "took": 2,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 3,
            "relation": "eq"
        },
        "max_score": 1.0,
        "hits": [
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "1",
                "_score": 1.0,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "老鹰",
                    "birthDay": 831182400000,
                    "country": "美国",
                    "teamCityEn": "Atlanta",
                    "code": "jaylen_adams",
                    "displayAffiliation": "United States",
                    "displayName": "杰伦 亚当斯",
                    "schoolType": "College",
                    "teamConference": "东部",
                    "teamConferenceEn": "Eastern",
                    "weight": "86.2 公斤",
                    "teamCity": "亚特兰大",
                    "playYear": 1,
                    "jerseyNo": "10",
                    "teamNameEn": "Hawks",
                    "draft": 2018,
                    "displayNameEn": "Jaylen Adams",
                    "heightValue": 1.88,
                    "birthDayStr": "1996-05-04",
                    "position": "后卫",
                    "age": 23,
                    "playerId": "1629121"
                }
            },
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "2",
                "_score": 1.0,
                "_source": {
                    "countryEn": "New Zealand",
                    "teamName": "雷霆",
                    "birthDay": 743140800000,
                    "country": "新西兰",
                    "teamCityEn": "Oklahoma City",
                    "code": "steven_adams",
                    "displayAffiliation": "Pittsburgh/New Zealand",
                    "displayName": "斯蒂文 亚当斯",
                    "schoolType": "College",
                    "teamConference": "西部",
                    "teamConferenceEn": "Western",
                    "weight": "120.2 公斤",
                    "teamCity": "俄克拉荷马城",
                    "playYear": 6,
                    "jerseyNo": "12",
                    "teamNameEn": "Thunder",
                    "draft": 2013,
                    "displayNameEn": "Steven Adams",
                    "heightValue": 2.13,
                    "birthDayStr": "1993-07-20",
                    "position": "中锋",
                    "age": 26,
                    "playerId": "203500"
                }
            },
            {
                "_index": "nba",
                "_type": "_doc",
                "_id": "3",
                "_score": 1.0,
                "_source": {
                    "countryEn": "United States",
                    "teamName": "热火",
                    "birthDay": 869198400000,
                    "country": "美国",
                    "teamCityEn": "Miami",
                    "code": "bam_adebayo",
                    "displayAffiliation": "Kentucky/United States",
                    "displayName": "巴姆 阿德巴约",
                    "schoolType": "College",
                    "teamConference": "东部",
                    "teamConferenceEn": "Eastern",
                    "weight": "115.7 公斤",
                    "teamCity": "迈阿密",
                    "playYear": 2,
                    "jerseyNo": "13",
                    "teamNameEn": "Heat",
                    "draft": 2017,
                    "displayNameEn": "Bam Adebayo",
                    "heightValue": 2.08,
                    "birthDayStr": "1997-07-18",
                    "position": "中锋-前锋",
                    "age": 22,
                    "playerId": "1628389"
                }
            }
        ]
    }
}
```

####  6.3 范围查询 

##### 6.3.1 第一种范围查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 查找在nba打了2年年到10年以内的球员 
  "query": {
    "range": {
      "playYear":{ //playYear 是Long类型 不要写成字符串
      	"gte": 2,
      	"lte" : 10
      }
    }
  },
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 77,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "3",
        "_score" : 1.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "热火",
          "birthDay" : 869198400000,
          "country" : "美国",
          "teamCityEn" : "Miami",
          "code" : "bam_adebayo",
          "displayAffiliation" : "Kentucky/United States",
          "displayName" : "巴姆 阿德巴约",
          "schoolType" : "College",
          "teamConference" : "东部",
          "teamConferenceEn" : "Eastern",
          "weight" : "115.7 公斤",
          "teamCity" : "迈阿密",
          "playYear" : 2,
          "jerseyNo" : "13",
          "teamNameEn" : "Heat",
          "draft" : 2017,
          "displayNameEn" : "Bam Adebayo",
          "heightValue" : 2.08,
          "birthDayStr" : "1997-07-18",
          "position" : "中锋-前锋",
          "age" : 22,
          "playerId" : "1628389"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "9",
        "_score" : 1.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "篮网",
          "birthDay" : 893131200000,
          "country" : "美国",
          "teamCityEn" : "Brooklyn",
          "code" : "jarrett_allen",
          "displayAffiliation" : "Texas/United States",
          "displayName" : "贾瑞特 艾伦",
          "schoolType" : "College",
          "teamConference" : "东部",
          "teamConferenceEn" : "Eastern",
          "weight" : "107.5 公斤",
          "teamCity" : "布鲁克林",
          "playYear" : 2,
          "jerseyNo" : "31",
          "teamNameEn" : "Nets",
          "draft" : 2017,
          "displayNameEn" : "Jarrett Allen",
          "heightValue" : 2.11,
          "birthDayStr" : "1998-04-21",
          "position" : "中锋",
          "age" : 21,
          "playerId" : "1628386"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "10",
        "_score" : 1.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "尼克斯",
          "birthDay" : 727074000000,
          "country" : "美国",
          "teamCityEn" : "New York",
          "code" : "kadeem_allen",
          "displayAffiliation" : "Arizona/United States",
          "displayName" : "卡迪姆 艾伦",
          "schoolType" : "College",
          "teamConference" : "东部",
          "teamConferenceEn" : "Eastern",
          "weight" : "90.7 公斤",
          "teamCity" : "纽约",
          "playYear" : 2,
          "jerseyNo" : "0",
          "teamNameEn" : "Knicks",
          "draft" : 2017,
          "displayNameEn" : "Kadeem Allen",
          "heightValue" : 1.9,
          "birthDayStr" : "1993-01-15",
          "position" : "后卫",
          "age" : 26,
          "playerId" : "1628443"
        }
      }
    ]
  }
}

```

##### 6.3.2 第二种范围查询 GET/POST请求

- 请求
```java

```

- 请求体
```json
{
    // 语义: 查找1980年年到1999年年出⽣生的球员
  "query": {
    "range": {
      "birthDay":{
      	"gte": "01/01/1999",
      	"lte" : "1999",,
      	"format": "dd/MM/yyyy||yyyy" // 日期格式要对应get lte
      }
    }
  },
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 521,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "老鹰",
          "birthDay" : 831182400000,
          "country" : "美国",
          "teamCityEn" : "Atlanta",
          "code" : "jaylen_adams",
          "displayAffiliation" : "United States",
          "displayName" : "杰伦 亚当斯",
          "schoolType" : "College",
          "teamConference" : "东部",
          "teamConferenceEn" : "Eastern",
          "weight" : "86.2 公斤",
          "teamCity" : "亚特兰大",
          "playYear" : 1,
          "jerseyNo" : "10",
          "teamNameEn" : "Hawks",
          "draft" : 2018,
          "displayNameEn" : "Jaylen Adams",
          "heightValue" : 1.88,
          "birthDayStr" : "1996-05-04",
          "position" : "后卫",
          "age" : 23,
          "playerId" : "1629121"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "2",
        "_score" : 1.0,
        "_source" : {
          "countryEn" : "New Zealand",
          "teamName" : "雷霆",
          "birthDay" : 743140800000,
          "country" : "新西兰",
          "teamCityEn" : "Oklahoma City",
          "code" : "steven_adams",
          "displayAffiliation" : "Pittsburgh/New Zealand",
          "displayName" : "斯蒂文 亚当斯",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "120.2 公斤",
          "teamCity" : "俄克拉荷马城",
          "playYear" : 6,
          "jerseyNo" : "12",
          "teamNameEn" : "Thunder",
          "draft" : 2013,
          "displayNameEn" : "Steven Adams",
          "heightValue" : 2.13,
          "birthDayStr" : "1993-07-20",
          "position" : "中锋",
          "age" : 26,
          "playerId" : "203500"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "3",
        "_score" : 1.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "热火",
          "birthDay" : 869198400000,
          "country" : "美国",
          "teamCityEn" : "Miami",
          "code" : "bam_adebayo",
          "displayAffiliation" : "Kentucky/United States",
          "displayName" : "巴姆 阿德巴约",
          "schoolType" : "College",
          "teamConference" : "东部",
          "teamConferenceEn" : "Eastern",
          "weight" : "115.7 公斤",
          "teamCity" : "迈阿密",
          "playYear" : 2,
          "jerseyNo" : "13",
          "teamNameEn" : "Heat",
          "draft" : 2017,
          "displayNameEn" : "Bam Adebayo",
          "heightValue" : 2.08,
          "birthDayStr" : "1997-07-18",
          "position" : "中锋-前锋",
          "age" : 22,
          "playerId" : "1628389"
        }
      }
    ]
  }
}

```

#### 6.4 布尔查询

|   type   |  description    |
| ---- | ---- |
|   must   |   必须出现在匹配文档中   |
|   filter   |   必须出现在文档中，但是不打分   |
|   must_not   |   不能出现在文档中   |
|   should   |   应该出现在文档中   |


##### 6.4.1 must查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 查询名字叫做james的球员
  "query": {
    "bool": { // 布尔查询
      "must":[ //匹配查询方式
        {
          "match": { // 全文搜索
            "displayNameEn": "james"
          }
        }
      ]
    }
  },
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 5,
      "relation" : "eq"
    },
    "max_score" : 4.699642,
    "hits" : [
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "214",
        "_score" : 4.699642,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "火箭",
          "birthDay" : 620107200000,
          "country" : "美国",
          "teamCityEn" : "Houston",
          "code" : "james_harden",
          "displayAffiliation" : "Arizona State/United States",
          "displayName" : "詹姆斯 哈登",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "99.8 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 10,
          "jerseyNo" : "13",
          "teamNameEn" : "Rockets",
          "draft" : 2009,
          "displayNameEn" : "James Harden",
          "heightValue" : 1.96,
          "birthDayStr" : "1989-08-26",
          "position" : "后卫",
          "age" : 30,
          "playerId" : "201935"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "266",
        "_score" : 4.699642,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "国王",
          "birthDay" : 854082000000,
          "country" : "美国",
          "teamCityEn" : "Sacramento",
          "code" : "justin_james",
          "displayAffiliation" : "United States",
          "displayName" : "贾斯汀 詹姆斯",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "86.2 公斤",
          "teamCity" : "萨克拉门托",
          "playYear" : 0,
          "jerseyNo" : "",
          "teamNameEn" : "Kings",
          "draft" : 2019,
          "displayNameEn" : "Justin James",
          "heightValue" : 2.01,
          "birthDayStr" : "1997-01-24",
          "position" : "后卫-前锋",
          "age" : 22,
          "playerId" : "1629713"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "267",
        "_score" : 4.699642,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "湖人",
          "birthDay" : 473230800000,
          "country" : "美国",
          "teamCityEn" : "Los Angeles",
          "code" : "lebron_james",
          "displayAffiliation" : "No College/United States",
          "displayName" : "勒布朗 詹姆斯",
          "schoolType" : "High School",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "113.4 公斤",
          "teamCity" : "洛杉矶",
          "playYear" : 16,
          "jerseyNo" : "23",
          "teamNameEn" : "Lakers",
          "draft" : 2003,
          "displayNameEn" : "LeBron James",
          "heightValue" : 2.03,
          "birthDayStr" : "1984-12-30",
          "position" : "前锋",
          "age" : 35,
          "playerId" : "2544"
        }
      }
    ]
  }
}

```

##### 6.4.2 filter查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 查询名字叫做james的球员 不包含打分
  "query": {
    "bool": {
      "filter":[
        {
          "match": {
            "displayNameEn": "james"
          }
        }
      ]
    }
  },
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 5,
      "relation" : "eq"
    },
    "max_score" : 0.0,
    "hits" : [
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "158",
        "_score" : 0.0, // 不包含打分
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "76人",
          "birthDay" : 646804800000,
          "country" : "美国",
          "teamCityEn" : "Philadelphia",
          "code" : "james_ennis iii",
          "displayAffiliation" : "Cal State-Long Beach/United States",
          "displayName" : "詹姆斯 恩尼斯三世",
          "schoolType" : "College",
          "teamConference" : "东部",
          "teamConferenceEn" : "Eastern",
          "weight" : "95.3 公斤",
          "teamCity" : "费城",
          "playYear" : 5,
          "jerseyNo" : "11",
          "teamNameEn" : "76ers",
          "draft" : 2013,
          "displayNameEn" : "James Ennis III",
          "heightValue" : 2.01,
          "birthDayStr" : "1990-07-01",
          "position" : "前锋",
          "age" : 29,
          "playerId" : "203516"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "214",
        "_score" : 0.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "火箭",
          "birthDay" : 620107200000,
          "country" : "美国",
          "teamCityEn" : "Houston",
          "code" : "james_harden",
          "displayAffiliation" : "Arizona State/United States",
          "displayName" : "詹姆斯 哈登",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "99.8 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 10,
          "jerseyNo" : "13",
          "teamNameEn" : "Rockets",
          "draft" : 2009,
          "displayNameEn" : "James Harden",
          "heightValue" : 1.96,
          "birthDayStr" : "1989-08-26",
          "position" : "后卫",
          "age" : 30,
          "playerId" : "201935"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "266",
        "_score" : 0.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "国王",
          "birthDay" : 854082000000,
          "country" : "美国",
          "teamCityEn" : "Sacramento",
          "code" : "justin_james",
          "displayAffiliation" : "United States",
          "displayName" : "贾斯汀 詹姆斯",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "86.2 公斤",
          "teamCity" : "萨克拉门托",
          "playYear" : 0,
          "jerseyNo" : "",
          "teamNameEn" : "Kings",
          "draft" : 2019,
          "displayNameEn" : "Justin James",
          "heightValue" : 2.01,
          "birthDayStr" : "1997-01-24",
          "position" : "后卫-前锋",
          "age" : 22,
          "playerId" : "1629713"
        }
      }
    ]
  }
}

```

##### 6.4.3 must_not查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 查询名字叫做james的球员,一定不在东部的
  "query": {
    "bool": {
      "filter":[
        {
          "match": {
            "displayNameEn": "james"
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "teamConferenceEn": {
              "value": "eastern"
            }
          }
        }
      ]
    }
  },
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 5,
      "relation" : "eq"
    },
    "max_score" : 0.0,
    "hits" : [
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "158",
        "_score" : 0.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "76人",
          "birthDay" : 646804800000,
          "country" : "美国",
          "teamCityEn" : "Philadelphia",
          "code" : "james_ennis iii",
          "displayAffiliation" : "Cal State-Long Beach/United States",
          "displayName" : "詹姆斯 恩尼斯三世",
          "schoolType" : "College",
          "teamConference" : "东部",
          "teamConferenceEn" : "Eastern",
          "weight" : "95.3 公斤",
          "teamCity" : "费城",
          "playYear" : 5,
          "jerseyNo" : "11",
          "teamNameEn" : "76ers",
          "draft" : 2013,
          "displayNameEn" : "James Ennis III",
          "heightValue" : 2.01,
          "birthDayStr" : "1990-07-01",
          "position" : "前锋",
          "age" : 29,
          "playerId" : "203516"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "214",
        "_score" : 0.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "火箭",
          "birthDay" : 620107200000,
          "country" : "美国",
          "teamCityEn" : "Houston",
          "code" : "james_harden",
          "displayAffiliation" : "Arizona State/United States",
          "displayName" : "詹姆斯 哈登",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "99.8 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 10,
          "jerseyNo" : "13",
          "teamNameEn" : "Rockets",
          "draft" : 2009,
          "displayNameEn" : "James Harden",
          "heightValue" : 1.96,
          "birthDayStr" : "1989-08-26",
          "position" : "后卫",
          "age" : 30,
          "playerId" : "201935"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "266",
        "_score" : 0.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "国王",
          "birthDay" : 854082000000,
          "country" : "美国",
          "teamCityEn" : "Sacramento",
          "code" : "justin_james",
          "displayAffiliation" : "United States",
          "displayName" : "贾斯汀 詹姆斯",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "86.2 公斤",
          "teamCity" : "萨克拉门托",
          "playYear" : 0,
          "jerseyNo" : "",
          "teamNameEn" : "Kings",
          "draft" : 2019,
          "displayNameEn" : "Justin James",
          "heightValue" : 2.01,
          "birthDayStr" : "1997-01-24",
          "position" : "后卫-前锋",
          "age" : 22,
          "playerId" : "1629713"
        }
      }
    ]
  }
}

```

##### 6.4.4 should 第一种 查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 查找名字叫做james的打球时间应该在11到20年的西部球员
    // should应该 但是不是必须
  "query": {
    "bool": {
      "filter":[
        {
          "match": {
            "displayNameEn": "james"
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "teamConferenceEn": {
              "value": "eastern"
            }
          }
        }
      ],
      "should": [
        {
          "range": {
            "playYear": {
              "gte": 11,
              "lte": 20
            }
          }
        }
      ]
    }
  },
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 5,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "267",
        "_score" : 1.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "湖人",
          "birthDay" : 473230800000,
          "country" : "美国",
          "teamCityEn" : "Los Angeles",
          "code" : "lebron_james",
          "displayAffiliation" : "No College/United States",
          "displayName" : "勒布朗 詹姆斯",
          "schoolType" : "High School",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "113.4 公斤",
          "teamCity" : "洛杉矶",
          "playYear" : 16,
          "jerseyNo" : "23",
          "teamNameEn" : "Lakers",
          "draft" : 2003,
          "displayNameEn" : "LeBron James",
          "heightValue" : 2.03,
          "birthDayStr" : "1984-12-30",
          "position" : "前锋",
          "age" : 35,
          "playerId" : "2544"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "158",
        "_score" : 0.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "76人",
          "birthDay" : 646804800000,
          "country" : "美国",
          "teamCityEn" : "Philadelphia",
          "code" : "james_ennis iii",
          "displayAffiliation" : "Cal State-Long Beach/United States",
          "displayName" : "詹姆斯 恩尼斯三世",
          "schoolType" : "College",
          "teamConference" : "东部",
          "teamConferenceEn" : "Eastern",
          "weight" : "95.3 公斤",
          "teamCity" : "费城",
          "playYear" : 5,
          "jerseyNo" : "11",
          "teamNameEn" : "76ers",
          "draft" : 2013,
          "displayNameEn" : "James Ennis III",
          "heightValue" : 2.01,
          "birthDayStr" : "1990-07-01",
          "position" : "前锋",
          "age" : 29,
          "playerId" : "203516"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "214",
        "_score" : 0.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "火箭",
          "birthDay" : 620107200000,
          "country" : "美国",
          "teamCityEn" : "Houston",
          "code" : "james_harden",
          "displayAffiliation" : "Arizona State/United States",
          "displayName" : "詹姆斯 哈登",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "99.8 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 10,
          "jerseyNo" : "13",
          "teamNameEn" : "Rockets",
          "draft" : 2009,
          "displayNameEn" : "James Harden",
          "heightValue" : 1.96,
          "birthDayStr" : "1989-08-26",
          "position" : "后卫",
          "age" : 30,
          "playerId" : "201935"
        }
      }
    ]
  }
}
```

##### 6.4.5 should 第二种 查询 GET/POST请求

- 请求
```java

```

- 请求体
```json
{
    // 语义: 查找名字叫做james的打球时间应该在11到20年的西部球员
    // minimum_should_match = 1 最小匹配精度,至少有一个条件满足
  "query": {
    "bool": {
      "filter":[
        {
          "match": {
            "displayNameEn": "james"
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "teamConferenceEn": {
              "value": "eastern"
            }
          }
        }
      ],
      "should": [
        {
          "range": {
            "playYear": {
              "gte": 11,
              "lte": 20
            }
          }
        }
      ]
      , "minimum_should_match": 1
    }
  },
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "267",
        "_score" : 1.0,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "湖人",
          "birthDay" : 473230800000,
          "country" : "美国",
          "teamCityEn" : "Los Angeles",
          "code" : "lebron_james",
          "displayAffiliation" : "No College/United States",
          "displayName" : "勒布朗 詹姆斯",
          "schoolType" : "High School",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "113.4 公斤",
          "teamCity" : "洛杉矶",
          "playYear" : 16,
          "jerseyNo" : "23",
          "teamNameEn" : "Lakers",
          "draft" : 2003,
          "displayNameEn" : "LeBron James",
          "heightValue" : 2.03,
          "birthDayStr" : "1984-12-30",
          "position" : "前锋",
          "age" : 35,
          "playerId" : "2544"
        }
      }
    ]
  }
}
```

#### 6.5 排序查询

##### 6.5.1 第一种排序查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: ⽕箭队中按打球时间从⼤到⼩排序的球员
  "query": {
    "match": {
      "teamName": "Rockets"
    }
  },
  "sort": [
    {
      "playYear": {
        "order": "desc"
      }
    }
  ], 
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "395",
        "_score" : null,
        "_source" : {
          "countryEn" : "Brazil",
          "teamName" : "火箭",
          "birthDay" : 400737600000,
          "country" : "巴西",
          "teamCityEn" : "Houston",
          "code" : "nene_hilario",
          "displayAffiliation" : "Brazil",
          "displayName" : "内内",
          "schoolType" : "",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "113.4 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 17,
          "jerseyNo" : "42",
          "teamNameEn" : "Rockets",
          "draft" : 2002,
          "displayNameEn" : "Nene",
          "heightValue" : 2.11,
          "birthDayStr" : "1982-09-13",
          "position" : "中锋-前锋",
          "age" : 37,
          "playerId" : "2403"
        },
        "sort" : [
          17
        ]
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "425",
        "_score" : null,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "火箭",
          "birthDay" : 484200000000,
          "country" : "美国",
          "teamCityEn" : "Houston",
          "code" : "chris_paul",
          "displayAffiliation" : "Wake Forest/United States",
          "displayName" : "克里斯 保罗",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "79.4 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 14,
          "jerseyNo" : "3",
          "teamNameEn" : "Rockets",
          "draft" : 2005,
          "displayNameEn" : "Chris Paul",
          "heightValue" : 1.83,
          "birthDayStr" : "1985-05-06",
          "position" : "后卫",
          "age" : 34,
          "playerId" : "101108"
        },
        "sort" : [
          14
        ]
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "206",
        "_score" : null,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "火箭",
          "birthDay" : 507099600000,
          "country" : "美国",
          "teamCityEn" : "Houston",
          "code" : "gerald_green",
          "displayAffiliation" : "No College/United States",
          "displayName" : "杰拉德 格林",
          "schoolType" : "High School",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "93.0 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 12,
          "jerseyNo" : "14",
          "teamNameEn" : "Rockets",
          "draft" : 2005,
          "displayNameEn" : "Gerald Green",
          "heightValue" : 2.01,
          "birthDayStr" : "1986-01-26",
          "position" : "后卫-前锋",
          "age" : 33,
          "playerId" : "101123"
        },
        "sort" : [
          12
        ]
      }
    ]
  }
}

```

##### 6.5.2 第一种排序查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: ⽕箭队中按打球时间从⼤到⼩，如果年龄相同则按照身⾼从⾼到低排序的球员
  "query": {
    "match": {
      "teamNameEn": "Rockets"
    }
  },
  "sort": [
    {
      "playYear": {
        "order": "desc"
      }
    },
    {
      "heightValue": {
        "order": "asc"
      }
    }
  ], 
  "from": 0,
  "size": 3
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "395",
        "_score" : null,
        "_source" : {
          "countryEn" : "Brazil",
          "teamName" : "火箭",
          "birthDay" : 400737600000,
          "country" : "巴西",
          "teamCityEn" : "Houston",
          "code" : "nene_hilario",
          "displayAffiliation" : "Brazil",
          "displayName" : "内内",
          "schoolType" : "",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "113.4 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 17,
          "jerseyNo" : "42",
          "teamNameEn" : "Rockets",
          "draft" : 2002,
          "displayNameEn" : "Nene",
          "heightValue" : 2.11,
          "birthDayStr" : "1982-09-13",
          "position" : "中锋-前锋",
          "age" : 37,
          "playerId" : "2403"
        },
        "sort" : [
          17,
          2.11
        ]
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "425",
        "_score" : null,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "火箭",
          "birthDay" : 484200000000,
          "country" : "美国",
          "teamCityEn" : "Houston",
          "code" : "chris_paul",
          "displayAffiliation" : "Wake Forest/United States",
          "displayName" : "克里斯 保罗",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "79.4 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 14,
          "jerseyNo" : "3",
          "teamNameEn" : "Rockets",
          "draft" : 2005,
          "displayNameEn" : "Chris Paul",
          "heightValue" : 1.83,
          "birthDayStr" : "1985-05-06",
          "position" : "后卫",
          "age" : 34,
          "playerId" : "101108"
        },
        "sort" : [
          14,
          1.83
        ]
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "206",
        "_score" : null,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "火箭",
          "birthDay" : 507099600000,
          "country" : "美国",
          "teamCityEn" : "Houston",
          "code" : "gerald_green",
          "displayAffiliation" : "No College/United States",
          "displayName" : "杰拉德 格林",
          "schoolType" : "High School",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "93.0 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 12,
          "jerseyNo" : "14",
          "teamNameEn" : "Rockets",
          "draft" : 2005,
          "displayNameEn" : "Gerald Green",
          "heightValue" : 2.01,
          "birthDayStr" : "1986-01-26",
          "position" : "后卫-前锋",
          "age" : 33,
          "playerId" : "101123"
        },
        "sort" : [
          12,
          2.01
        ]
      }
    ]
  }
}
```

#### 6.6 聚合查询指标聚合

- ES聚合分析是什么

1. 聚合分析是数据库中重要的功能特性，完成对⼀个查询的数据集中数据的聚合计算，如：找出某字段（或计算表达式的结果）的最⼤值、最⼩值，计算和、平均值等。ES作为搜索引擎兼数据库，同样提供了强⼤的聚合分析能力。

2. 对⼀个数据集求最大、最小、和、平均值等指标的聚合，在ES中称为**指标聚合** 

3. 而关系型数据库中除了有聚合函数外，还可以对查询出的数据行分组group by，再在组上进行指标聚合。在ES中称为**桶聚合**

##### 6.6.1 max min sum avg 指标聚合查询 GET/POST

- max min sum avg 指标聚合

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 查询火箭队球员平均年龄
  "query": {
    "term": {
      "teamNameEn": "Rockets"
    }
  },
  "aggs": { // aggs 代表使用聚合函数
    "avgAge": { // avgAge是自定义的，因为是查年年龄平均所以起名avgAge
      "avg": { // avg 平均
        "field": "age"
      }
    }
  }, 
  "size": 0 // 不看数据只看指标聚合
}
```

- 响应
```json
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "avgAge" : {
      "value" : 26.761904761904763 // 平均年龄
    }
  }
}
```

##### 6.6.2 value_count 指标聚合查询 GET/POST

- 统计非空字段的文档数

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 查询火箭队中球员打球时间不为空的数量
  "query": {
    "term": {
      "teamNameEn": "Rockets"
    }
  },
  "aggs": {
    "countPlayerYear": {
      "value_count": {
        "field": "playYear"
      }
    }
  }, 
  "size": 0
}
```

- 响应
```json
{
  "took" : 4,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "countPlayerYear" : {
      "value" : 21
    }
  }
}
```

##### 6.6.3 _count 指标聚合查询（严格来说不属于聚合） GET/POST

- 查询火箭队有多少文档，也就是有多少球员

- 请求
```java
localhost:9200/nba/_count
```

- 请求体
```json
{
  "query": {
    "term": {
      "teamNameEn": "Rockets"
    }
  }
}
```

- 响应
```json
{
    "count": 21,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    }
}
```

##### 6.6.4 cardinality 指标聚合查询 GET/POST

- Cardinality 值去重计算

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 查询火箭队中年龄不同的数量
  "query": {
    "term": {
      "teamNameEn": "Rockets"
    }
  },
  "aggs": {
    "countAge": { // 自定义名字
      "cardinality": { // 去掉相同的只保留一个值
        "field": "age"
      }
    }
  },
  "size": 0
}
```

- 响应
```json
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "countAge" : {
      "value" : 13
    }
  }
}

```

##### 6.6.5 status 种指标聚合查询 GET/POST

- stats 统计count max min avg sum 5个值

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义:　查询火箭球员的年龄status
  "query": {
    "term": {
      "teamNameEn": "Rockets"
    }
  },
  "aggs": {
    "status": {
      "stats": {
        "field": "age"
      }
    }
  },
  "size": 0
}
```

- 响应
```json
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "status" : {
      "count" : 21,
      "min" : 21.0,
      "max" : 37.0,
      "avg" : 26.761904761904763,
      "sum" : 562.0
    }
  }
}

```

##### 6.6.6 extended status 种指标聚合查询 GET/POST

- Extended stats ⽐比stats多4个统计结果： 平⽅方和、⽅方差、标准差、平均值加减两个标准差的区间

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 查出火箭队球员的年龄Extend stats
  "query": {
    "term": {
      "teamNameEn": "Rockets"
    }
  },
  "aggs": {
    "extendedStatsAge": {
      "extended_stats": {
        "field": "age"
      }
    }
  },
  "size": 0
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "extendedStatsAge" : {
      "count" : 21,
      "min" : 21.0,
      "max" : 37.0,
      "avg" : 26.761904761904763,
      "sum" : 562.0,
      "sum_of_squares" : 15534.0,
      "variance" : 23.5147392290249,
      "std_deviation" : 4.84919985451465,
      "std_deviation_bounds" : {
        "upper" : 36.46030447093406,
        "lower" : 17.063505052875463
      }
    }
  }
}

```

##### 6.6.7 percentiles 种指标聚合查询 GET/POST

- Percentiles 占比百分位对应的值统计，默认返回[ 1, 5, 25, 50, 75, 95, 99 ]分位上的值

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 查出火箭的球员的年龄占比
  "query": {
    "term": {
      "teamNameEn": "Rockets"
    }
  },
  "aggs": {
    "percentilesAge": {
      "percentiles": {
        "field": "age",
        // 这里可以指定，不使用默认
        // "percents": [
        //   25,
        //   50,
        //   75
        // ]
      }
    }
  },
  "size": 0
}
```

- 响应
```json
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "percentilesAge" : {
      "values" : {
        "1.0" : 21.0,
        "5.0" : 21.0,
        "25.0" : 22.75, // 小于22.75岁的 占25%
        "50.0" : 25.0,
        "75.0" : 30.25,// 小于30.25岁的 占75%
        "95.0" : 35.349999999999994,
        "99.0" : 37.0
      }
    }
  }
}

```

#### 6.7 聚合查询桶聚合

##### 6.7.1 terms aggregation 桶聚合查询 GET/POST

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 火箭队根据年龄进行分组
  "query": {
    "term": {
      "teamNameEn": "Rockets"
    }
  },
  "aggs": {
    "aggrAge": {
      "terms": { // 因为根据年龄做桶聚合所以使用terms
        "field": "age",
        "size": 3 // 指定显示桶数
      }
    }
  },
  "size": 0
}
```

- 响应
```json
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "aggrAge" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 3,
      "buckets" : [
        {
          "key" : 21, // 指定的字段 age
          "doc_count" : 4 // 21岁的文档数有4个
        },
        {
          "key" : 25,
          "doc_count" : 3
        },
        {
          "key" : 23,
          "doc_count" : 2
        }
      ]
    }
  }
}

```

##### 6.7.2 order 第一种分组聚合查询 GET/POST

- 请求体
```json
{
    // 语义: 根据火箭队年龄进行分组，分组信息通过年龄从大到小排序（通过指定字段）
  "query": {
    "term": {
      "teamNameEn": "Rockets"
    }
  },
  "aggs": {
    "aggrAge": {
      "terms": {
        "field": "age",
        "size": 3,
        "order": {
          "_key": "desc" //"_key关键字 倒序排序
        //   "_count": "desc"  通过文档排序
        }
      }
    }
  },
  "size": 0
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 21,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "aggrAge" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 17,
      "buckets" : [
        {
          "key" : 37,
          "doc_count" : 1
        },
        {
          "key" : 34,
          "doc_count" : 2
        },
        {
          "key" : 33,
          "doc_count" : 1
        }
      ]
    }
  }
}

```

##### 6.7.3 order 第二种分组聚合查询 GET/POST

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 每支球队按该队所有球员的平均年龄进行分组排序（通过分组指标值）
  "aggs": {
    "aggsTeamName": {
      "terms": { // 因为使用分组所以用terms
        "field": "teamNameEn",
        "size": 3, // 显示3条
        "order": {
          "avgAge": "desc"
        }
      }, // 因为需要通过对球队的平均年龄进行分组排序，所以再写aggs
      "aggs": {
        "avgAge": {
          "avg": {
            "field": "age"
          }
        }
      }
    }
  },
  "size": 0
}
```

- 响应
```json
{
  "took" : 3,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 566,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "aggsTeamName" : {
      "doc_count_error_upper_bound" : -1,
      "sum_other_doc_count" : 511,
      "buckets" : [
        {
          "key" : "Bucks",
          "doc_count" : 14,
          "avgAge" : {
            "value" : 28.142857142857142
          }
        },
        {
          "key" : "Mavericks",
          "doc_count" : 20,
          "avgAge" : {
            "value" : 27.85
          }
        },
        {
          "key" : "Lakers",
          "doc_count" : 21,
          "avgAge" : {
            "value" : 27.714285714285715
          }
        }
      ]
    }
  }
}

```


##### 6.7.3 include 筛选分组聚合查询 GET/POST

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 湖人和火箭队按球队平均年龄进行分组排序（指定值列表 ）
  "aggs": {
    "aggsTeamName": {
      "terms": {
        "field": "teamNameEn",
        // include指定队伍  ["Lakes","Rockets","warriors"]
        "include": ["Lakes","Rockets","warriors"], 
        // exclude去除["warriors"]
        "exclude": ["warriors"], 
        "size": 3,
        "order": {
          "avgAge": "desc"
        }
      },
      "aggs": {
        "avgAge": {
          "avg": {
            "field": "age"
          }
        }
      }
    }
  },
  "size": 0
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 566,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "aggsTeamName" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "Rockets",
          "doc_count" : 21,
          "avgAge" : {
            "value" : 26.761904761904763
          }
        }
      ]
    }
  }
}

```

##### 6.7.4 include 正则筛选分组聚合查询 GET/POST

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 湖人和火箭队按球队平均年龄进行分组排序（指定值列表 ）
  "aggs": {
    "aggsTeamName": {
      "terms": {
        "field": "teamNameEn",
        // 使用正则表达式 
        "include": "Lakers|Ro.*|Warriors.*", 
        "exclude": "warriors", 
        "size": 3,
        "order": {
          "avgAge": "desc"
        }
      },
      "aggs": {
        "avgAge": {
          "avg": {
            "field": "age"
          }
        }
      }
    }
  },
  "size": 0
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 566,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "aggsTeamName" : {
      "doc_count_error_upper_bound" : 0,
      "sum_other_doc_count" : 0,
      "buckets" : [
        {
          "key" : "Lakers",
          "doc_count" : 21,
          "avgAge" : {
            "value" : 27.714285714285715
          }
        },
        {
          "key" : "Rockets",
          "doc_count" : 21,
          "avgAge" : {
            "value" : 26.761904761904763
          }
        },
        {
          "key" : "Warriors",
          "doc_count" : 20,
          "avgAge" : {
            "value" : 26.25
          }
        }
      ]
    }
  }
}

```

##### 6.7.4 range aggregation 范围分组聚合查询 GET/POST

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 球员年龄按20、20-35、35 这样分组
  "aggs": {
    "ageRange": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "to": 20,  // 小于等于 20
            "key": "A" //别名 A
          },
          {
            "from": 20, // 大于等于20
            "to": 35, //  小于等于 35
            "key": "B"  //别名 B
          },
          {
            "from": 35, // 大于等于35 
            "key": "C" //别名 C
          }
        ]
      }
    }
  }, 
  "size": 0
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 566,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "ageRange" : {
      "buckets" : [
        {
          "key" : "A",
          "to" : 20.0,
          "doc_count" : 15
        },
        {
          "key" : "B",
          "from" : 20.0,
          "to" : 35.0,
          "doc_count" : 531
        },
        {
          "key" : "C",
          "from" : 35.0,
          "doc_count" : 20
        }
      ]
    }
  }
}

```

##### 6.7.5 date rabge aggregation 时间范围分组聚合查询 GET/POST

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 球员按出生年月分组
  "aggs": {
    "birthDayRange": {
      "date_range": {   // 时间类型
        "field": "birthDay", // 生日 生日是时间类型
        "format": "MM-yyy",  //时间格式 月份年份
        "ranges": [
          {
            "from": "01-1989" // 小于1989年1月出生
          },
          {
            "from": "01-1989", // 1989年1月到1999年1月
            "to": "01-1999"
          },
          {
            "from": "01-1999", // 1999年1月到2009年1月
            "to": "01-2009"
          },
          {
            "from": "01-2009"
          }
        ]
      }
    }
  }, 
  "size": 0
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 566,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "birthDayRange" : {
      "buckets" : [
        {
          "key" : "01-1989-01-1999",
          "from" : 5.99616E11,
          "from_as_string" : "01-1989",
          "to" : 9.151488E11,
          "to_as_string" : "01-1999",
          "doc_count" : 426
        },
        {
          "key" : "01-1989-*",
          "from" : 5.99616E11,
          "from_as_string" : "01-1989",
          "doc_count" : 469
        },
        {
          "key" : "01-1999-01-2009",
          "from" : 9.151488E11,
          "from_as_string" : "01-1999",
          "to" : 1.230768E12,
          "to_as_string" : "01-2009",
          "doc_count" : 43
        },
        {
          "key" : "01-2009-*",
          "from" : 1.230768E12,
          "from_as_string" : "01-2009",
          "doc_count" : 0
        }
      ]
    }
  }
}


```

##### 6.7.6 date histogram aggregation 时间范围分组聚合查询 GET/POST

- 按天、月、年等进行聚合统计。可按 year (1y), quarter (1q), month (1M), week (1w), day (1d), hour (1h), minute (1m), second (1s) 间隔聚合

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 球员按出⽣生年年分组
  "aggs": {
    "birthday_aggs": {
      "date_histogram": {
        "field": "birthDay", //生日字段
        "format": "yyyy",  // 时间格式
        "calendar_interval": "year" // 根据年
      }
    }
  }, 
  "size": 0
}
```

- 响应
```json
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 566,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "aggregations" : {
    "birthday_aggs" : {
      "buckets" : [
        {
          "key_as_string" : "1977",
          "key" : 220924800000,
          "doc_count" : 1
        },
        {
          "key_as_string" : "1978",
          "key" : 252460800000,
          "doc_count" : 1
        },
        {
          "key_as_string" : "1979",
          "key" : 283996800000,
          "doc_count" : 0
        },
        {
          "key_as_string" : "1980",
          "key" : 315532800000,
          "doc_count" : 3
        },
        {
          "key_as_string" : "1981",
          "key" : 347155200000,
          "doc_count" : 2
        },
        {
          "key_as_string" : "1982",
          "key" : 378691200000,
          "doc_count" : 3
        },
        {
          "key_as_string" : "1983",
          "key" : 410227200000,
          "doc_count" : 2
        },
        {
          "key_as_string" : "1984",
          "key" : 441763200000,
          "doc_count" : 8
        },
        {
          "key_as_string" : "1985",
          "key" : 473385600000,
          "doc_count" : 15
        },
        {
          "key_as_string" : "1986",
          "key" : 504921600000,
          "doc_count" : 19
        },
        {
          "key_as_string" : "1987",
          "key" : 536457600000,
          "doc_count" : 16
        },
        {
          "key_as_string" : "1988",
          "key" : 567993600000,
          "doc_count" : 27
        },
        {
          "key_as_string" : "1989",
          "key" : 599616000000,
          "doc_count" : 24
        },
        {
          "key_as_string" : "1990",
          "key" : 631152000000,
          "doc_count" : 35
        },
        {
          "key_as_string" : "1991",
          "key" : 662688000000,
          "doc_count" : 31
        },
        {
          "key_as_string" : "1992",
          "key" : 694224000000,
          "doc_count" : 36
        },
        {
          "key_as_string" : "1993",
          "key" : 725846400000,
          "doc_count" : 46
        },
        {
          "key_as_string" : "1994",
          "key" : 757382400000,
          "doc_count" : 45
        },
        {
          "key_as_string" : "1995",
          "key" : 788918400000,
          "doc_count" : 57
        },
        {
          "key_as_string" : "1996",
          "key" : 820454400000,
          "doc_count" : 56
        },
        {
          "key_as_string" : "1997",
          "key" : 852076800000,
          "doc_count" : 57
        },
        {
          "key_as_string" : "1998",
          "key" : 883612800000,
          "doc_count" : 39
        },
        {
          "key_as_string" : "1999",
          "key" : 915148800000,
          "doc_count" : 28
        },
        {
          "key_as_string" : "2000",
          "key" : 946684800000,
          "doc_count" : 15
        }
      ]
    }
  }
}

```

#### 6.8 query_string 查询

- query_string 查询，如果熟悉lucene的查询语法，我们可以直接⽤用lucene查询语法写⼀一个查询串进行查询，ES中接到请求后，通过查询解析器 ,解析查询串生成对应的查询。

##### 6.8.1 query_string AND OR 单字段查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 指定单个字段查询（查询詹姆斯或者库里的球员）
  "query": {
    "query_string": {
      "default_field": "displayNameEn",
      "query": "james OR curry" // 这里可以使用or或and也就是sql里的or和and
    //  "query": "james AND curry"
    }
  },
  "size": 3
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 7,
      "relation" : "eq"
    },
    "max_score" : 5.4989905,
    "hits" : [
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "123",
        "_score" : 5.4989905,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "独行侠",
          "birthDay" : 651384000000,
          "country" : "美国",
          "teamCityEn" : "Dallas",
          "code" : "seth_curry",
          "displayAffiliation" : "Duke/United States",
          "displayName" : "賽斯 库里",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "83.9 公斤",
          "teamCity" : "达拉斯",
          "playYear" : 6,
          "jerseyNo" : "30",
          "teamNameEn" : "Mavericks",
          "draft" : 2013,
          "displayNameEn" : "Seth Curry",
          "heightValue" : 1.88,
          "birthDayStr" : "1990-08-23",
          "position" : "后卫",
          "age" : 29,
          "playerId" : "203552"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "124",
        "_score" : 5.4989905,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "勇士",
          "birthDay" : 574318800000,
          "country" : "美国",
          "teamCityEn" : "Golden State",
          "code" : "stephen_curry",
          "displayAffiliation" : "Davidson/United States",
          "displayName" : "斯蒂芬 库里",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "86.2 公斤",
          "teamCity" : "金州",
          "playYear" : 10,
          "jerseyNo" : "30",
          "teamNameEn" : "Warriors",
          "draft" : 2009,
          "displayNameEn" : "Stephen Curry",
          "heightValue" : 1.9,
          "birthDayStr" : "1988-03-14",
          "position" : "后卫",
          "age" : 31,
          "playerId" : "201939"
        }
      },
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "214",
        "_score" : 4.699642,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "火箭",
          "birthDay" : 620107200000,
          "country" : "美国",
          "teamCityEn" : "Houston",
          "code" : "james_harden",
          "displayAffiliation" : "Arizona State/United States",
          "displayName" : "詹姆斯 哈登",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "99.8 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 10,
          "jerseyNo" : "13",
          "teamNameEn" : "Rockets",
          "draft" : 2009,
          "displayNameEn" : "James Harden",
          "heightValue" : 1.96,
          "birthDayStr" : "1989-08-26",
          "position" : "后卫",
          "age" : 30,
          "playerId" : "201935"
        }
      }
    ]
  }
}

```

##### 6.8.2 query_string AND OR 多字段查询 GET/POST请求

- 请求
```java
localhost:9200/nba/_search
```

- 请求体
```json
{
    // 语义: 指定多个字段查询（查询詹姆斯或者库里的球员）
  "query": {
    "query_string": {
      "fields": [ // 指定多个字段
        "displayNameEn",
        "teamNameEn"
        ],
        "query": "james AND Rockets" // 查询条件
    }
  },
  "size": 3
}
```

- 响应
```json
{
  "took" : 0,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 7.9719486,
    "hits" : [
      {
        "_index" : "nba",
        "_type" : "_doc",
        "_id" : "214",
        "_score" : 7.9719486,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "火箭",
          "birthDay" : 620107200000,
          "country" : "美国",
          "teamCityEn" : "Houston",
          "code" : "james_harden",
          "displayAffiliation" : "Arizona State/United States",
          "displayName" : "詹姆斯 哈登",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "99.8 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 10,
          "jerseyNo" : "13",
          "teamNameEn" : "Rockets",
          "draft" : 2009,
          "displayNameEn" : "James Harden",
          "heightValue" : 1.96,
          "birthDayStr" : "1989-08-26",
          "position" : "后卫",
          "age" : 30,
          "playerId" : "201935"
        }
      }
    ]
  }
}

```

### 七、ElasticSerach 高级搜索

#### 7.1 索引别名的使用

- 在开发中，随着业务需求的迭代，较老的业务逻辑就要面临更新甚至是重构，而对于es来说，为了了适应新的业务逻辑，可能就要对原有的索引做一些修改，比如对某些字段做调整，甚至是重建索引。而做这些操作的时候，可能会对业务造成影响，甚至是停机调整等问题。由此，es提供了索引别名来解决这些问题。 索引别名就像一个快捷方式或是软连接，可以指向一个或多个索引，也可以给任意一个需要索引名的API来使⽤用。别名的应用为程序提供了极大地灵活性

##### 7.1.1 查询别名 GET请求

- 请求
```java
localhost:9200/nba/_alias // 查询nba别名
localhost:9200/_aliases  // 查询所有别名
```

- 响应
```json
{
  // 语义: 查看所有别名
    "nba": {
        "aliases": {}
    },
    ".kibana_task_manager": {
        "aliases": {}
    },
    ".kibana_1": {
        "aliases": {
            ".kibana": {}
        }
    }
}
```

##### 7.1.2 新增别名 POST请求

- 请求
```java
localhost:9200/_aliases
```


- 请求体
```json
{
    // 语义: nba 新建别名nba_v1.0
  "actions": [
    {
      "add": {
        "index": "nba",
        "alias": "nba_v1.0"
      }
    }
  ]
}
```

- 响应
```json
{
    "acknowledged": true
}
```

##### 7.1.3 删除别名 POST请求

- 请求
```java
localhost:9200/_aliases
```

- 请求体
```json
{
  // 语义: 删除nba的nba_v1.0别名
  "actions": [
    {
      "remove": {
        "index": "nba",
        "alias": "nba_v1.0"
      }
    }
  ]
}
```
- 响应
```json
{
    "acknowledged": true
}
```

##### 7.1.4 重命名别名 POST请求

- 请求
```java
localhost:9200/_aliases
```

- 请求体
```json
{
  // 先删除nba_v1.0 别名 在新增 nba_v2.0别名,即是重命名
  "actions": [
    {
      "remove": {
        "index": "nba",
        "alias": "nba_v1.0"
      }
    },
    {
      "add": {
        "index": "nba",
        "alias": "nba_v2.0"
      }
    }
  ]
}
```
- 响应
```json
{
    "acknowledged": true
}
```

##### 7.1.5 通过别名获取索引 GET请求

- 请求
```java
localhost:9200/nba_v2.0
```

- 响应
```json
{
    "nba": {
        "aliases": {
            "nba_v2.0": {}
        },
        "mappings": {
            "properties": {
                "age": {
                    "type": "integer"
                },
                "birthDay": {
                    "type": "date"
                },
                "birthDayStr": {
                    "type": "keyword"
                },
                "code": {
                    "type": "text"
                },
                "country": {
                    "type": "text"
                },
                "countryEn": {
                    "type": "text"
                },
                "displayAffiliation": {
                    "type": "text"
                },
                "displayName": {
                    "type": "text"
                },
                "displayNameEn": {
                    "type": "text"
                },
                "draft": {
                    "type": "long"
                },
                "heightValue": {
                    "type": "float"
                },
                "jerseyNo": {
                    "type": "text"
                },
                "playYear": {
                    "type": "long"
                },
                "playerId": {
                    "type": "keyword"
                },
                "position": {
                    "type": "text"
                },
                "schoolType": {
                    "type": "text"
                },
                "teamCity": {
                    "type": "text"
                },
                "teamCityEn": {
                    "type": "text"
                },
                "teamConference": {
                    "type": "keyword"
                },
                "teamConferenceEn": {
                    "type": "keyword"
                },
                "teamName": {
                    "type": "keyword"
                },
                "teamNameEn": {
                    "type": "keyword"
                },
                "weight": {
                    "type": "text"
                }
            }
        },
        "settings": {
            "index": {
                "creation_date": "1573300240575",
                "number_of_shards": "1",
                "number_of_replicas": "1",
                "uuid": "X7FLu-8sRcijAYKUkgBZQQ",
                "version": {
                    "created": "7020199"
                },
                "provided_name": "nba"
            }
        }
    }
}
```

##### 7.1.6 通过别名写索引 POST请求

- 当别名指定了一个索引，则可以做写的操作

- 请求
```java
localhost:9200/nba_v2.0/_doc/566
```

- 请求体
```json
{
  // 语义: 通过别名nba_v2.0 修改id为566的球员
    "countryEn": "Croatia",
    "teamName": "快船",
    "birthDay": 858661200000,
    "country": "克罗地亚",
    "teamCityEn": "LA",
    "code": "ivica_zubac",
    "displayAffiliation": "Croatia",
    "displayName": "伊维察 祖巴茨哥哥", //"伊维察 祖巴茨"修改为"伊维察 祖巴茨哥哥"
    "schoolType": "",
    "teamConference": "西部",
    "teamConferenceEn": "Western",
    "weight": "108.9 公斤",
    "teamCity": "洛杉矶",
    "playYear": 3,
    "jerseyNo": "40",
    "teamNameEn": "Clippers",
    "draft": 2016,
    "displayNameEn": "Ivica Zubac",
    "heightValue": 2.16,
    "birthDayStr": "1997-03-18",
    "position": "中锋",
    "age": 22,
    "playerId": "1627826"
   
}
```

- 响应
```json
{
    "_index": "nba",
    "_type": "_doc",
    "_id": "566",
    "_version": 2,
    "result": "updated",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 566,
    "_primary_term": 1
}
```

#### 7.2 如何重建索引

- Elasticsearch是⼀一个实时的分布式搜索引擎，为用户提供搜索服务，当我们决定存储某种数据时，在创建索引的时候需要将数据结构完整确定下来，于此同时索引的设定和很多固定配置将用不能改变。当需要改变数据结构时，就需要重新建立索引，为此，Elastic团队提供了很多辅助工具帮助开发人员进行重建索引

- 步骤

1. nba取一个别名nba_latest, nba_latest作为对外使用

- POST 请求
```java
localhost:9200/_aliases
```

- 请求体
```json
{
  "actions": [
    {
      "add": {
        "index": "nba",
        "alias": "nba_latest"
      }
    }
  ]
}
```

- 响应
```json
{
  "acknowledged" : true
}

```

2. 新增一个索引nba_20220101，结构复制于nba索引，根据业务要求修改字段


- PUT 请求
```java
localhost:9200/nba_20220101
```

- 请求体
```json
{
	"mappings": {
        "properties": {
            "age": {
                "type": "integer"
            },
            "birthDay": {
                "type": "date"
            },
            "birthDayStr": {
                "type": "keyword"
            },
            "code": {
                "type": "text"
            },
            "country": {
                "type": "keyword"
            },
            "countryEn": {
                "type": "keyword"
            },
            "displayAffiliation": {
                "type": "text"
            },
            "displayName": {
                "type": "text"
            },
            "displayNameEn": {
                "type": "text"
            },
            "draft": {
                "type": "long"
            },
            "heightValue": {
                "type": "float"
            },
            "jerseyNo": {
                "type": "keyword"
            },
            "playYear": {
                "type": "long"
            },
            "playerId": {
                "type": "keyword"
            },
            "position": {
                "type": "text"
            },
            "schoolType": {
                "type": "text"
            },
            "teamCity": {
                "type": "text"
            },
            "teamCityEn": {
                "type": "text"
            },
            "teamConference": {
                "type": "keyword"
            },
            "teamConferenceEn": {
                "type": "keyword"
            },
            "teamName": {
                "type": "keyword"
            },
            "teamNameEn": {
                "type": "keyword"
            },
            "weight": {
                "type": "text"
            }
        }
	}
}
```

- 响应
```json
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "nba_20220101"
}
```

3. 将nba数据同步到nba_20220101

- POST 请求
```java
localhost:9200/_reindex //同步请求 等待响应
localhost:9200/_reindex?wait_for_completion=false  // 异步请求 直接返回
```

- 请求体
```json
{
  "source": {
    "index": "nba"
  },
  "dest": {
    "index": "nba_20220101"
  }
}
```

- 响应
```json
{
  "took" : 102,
  "timed_out" : false,
  "total" : 566,
  "updated" : 0,
  "created" : 566,
  "deleted" : 0,
  "batches" : 1,
  "version_conflicts" : 0,
  "noops" : 0,
  "retries" : {
    "bulk" : 0,
    "search" : 0
  },
  "throttled_millis" : 0,
  "requests_per_second" : -1.0,
  "throttled_until_millis" : 0,
  "failures" : [ ]
}

```

4. 给nba_20220101添加别名nba_latest，删除nba别名nba_latest

- POST请求
```java
localhost:9200/_aliases
```

- 请求体
```json
{
  "actions": [
    {
      "add": {
        "index": "nba_20220101",
        "alias": "nba_latest"
      }
    }
  ]
}
```

- 响应
```json
{
  "acknowledged" : true
}

```

5. 删除nba索引

- DELETE请求
```java
localhost:9200/nba
```

- 响应
```json
{
  "acknowledged" : true
}
```

#### 7.3 refresh操作

- 理想的搜索

1. 新的数据一添加到索引中立马就能搜索到，但是真实情况不是这样的

2. 我们使⽤用链式命令请求，先添加⼀一个⽂文档，再⽴立刻搜索

```java
curl -X PUT localhost:9200/star/_doc/888 -H 'Content-Type:

application/json'	-d '{ "displayName": "蔡徐坤" }'

curl	-X GET localhost:9200/star/_doc/_search?pretty
```

3. 强制刷新

```java
curl -X PUT localhost:9200/star/_doc/666?refresh -H 'Content-Type:

application/json'	-d '{ "displayName": "杨超越" }'

curl	-X GET localhost:9200/star/_doc/_search?pretty
```
4. 修改默认更更新时间(默认时间是1s)


- PUT 请求
```java
localhost:9200/star/_settings
```

- 请求体
```json
{
  "index":{
    "refresh_interval": "5s"
  }
}
```

- 响应
```json
{
  "acknowledged" : true
}
```

5. 将refresh关闭

- PUT 请求
```java
localhost:9200/star/_settings
```

- 请求体
```json
{
  "index":{
    "refresh_interval": "-1"
  }
}
```

- 响应
```json
{
  "acknowledged" : true
}
```

#### 7.4 高亮查询

- 如果返回的结果集中很多符合条件的结果，那怎么能一眼就能看到我们想要的那个结果呢？比如，我们搜索苍井空 ，在结果集中，将所有苍井空 高亮显示？



- 请求
```java
localhost:9200/nba_latest/_search
```

- 请求体
```json
{
  // 语义: 查找NBA james球员，对james做高亮显示
  "query": {
    "match": {
      "displayNameEn": "james"
    }
  },
  "highlight": { // 高亮关键字
    "fields": { // 对displayNameEn做高亮
      "displayNameEn": {}
    }
  },
  "size": 2
}

 // 也可以自定义标签
{
  "query": {
    "match": {
      "displayNameEn": "james"
    }
  },
  "highlight": {
    "fields": {
      "displayNameEn": {
        // 定义高亮标签为 h1
        "pre_tags": ["<h1>"],
        "post_tags": ["</h1>"]
        
      }
    }
  },
  "size": 2
}
```

- 响应
```json
{
  "took" : 2,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 5,
      "relation" : "eq"
    },
    "max_score" : 4.699642,
    "hits" : [
      {
        "_index" : "nba_20220101",
        "_type" : "_doc",
        "_id" : "214",
        "_score" : 4.699642,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "火箭",
          "birthDay" : 620107200000,
          "country" : "美国",
          "teamCityEn" : "Houston",
          "code" : "james_harden",
          "displayAffiliation" : "Arizona State/United States",
          "displayName" : "詹姆斯 哈登",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "99.8 公斤",
          "teamCity" : "休斯顿",
          "playYear" : 10,
          "jerseyNo" : "13",
          "teamNameEn" : "Rockets",
          "draft" : 2009,
          "displayNameEn" : "James Harden",
          "heightValue" : 1.96,
          "birthDayStr" : "1989-08-26",
          "position" : "后卫",
          "age" : 30,
          "playerId" : "201935"
        },
        "highlight" : {
          // "<em> 包裹起来是 高亮的意思
          "displayNameEn" : [
            "<em>James</em> Harden"
          ]
        }
      },
      {
        "_index" : "nba_20220101",
        "_type" : "_doc",
        "_id" : "266",
        "_score" : 4.699642,
        "_source" : {
          "countryEn" : "United States",
          "teamName" : "国王",
          "birthDay" : 854082000000,
          "country" : "美国",
          "teamCityEn" : "Sacramento",
          "code" : "justin_james",
          "displayAffiliation" : "United States",
          "displayName" : "贾斯汀 詹姆斯",
          "schoolType" : "College",
          "teamConference" : "西部",
          "teamConferenceEn" : "Western",
          "weight" : "86.2 公斤",
          "teamCity" : "萨克拉门托",
          "playYear" : 0,
          "jerseyNo" : "",
          "teamNameEn" : "Kings",
          "draft" : 2019,
          "displayNameEn" : "Justin James",
          "heightValue" : 2.01,
          "birthDayStr" : "1997-01-24",
          "position" : "后卫-前锋",
          "age" : 22,
          "playerId" : "1629713"
        },
        "highlight" : {
          // "<em> 包裹起来是 高亮的意思
          "displayNameEn" : [
            "Justin <em>James</em>"
          ]
        }
      }
    ]
  }
}

```

#### 7.5 查询建议

- 查询建议是什么

1. 查询建议，是为了给用户提供更好的搜索体验。包括：词条检查，自动补全


- Suggester

1. Term suggester
2. Phrase suggester
3. Completion suggester


|   text   |   指定搜索文本   |
| ---- | ---- |
|   field   |   获取建议词的搜索字段   |
|   analyzer   |   指定分词器   |
|   size   |   每个词返回的最大建议词数   |
|   sort   |   如何对建议词进行排序，可用选项:score:先按评分排序、再按文档频率排、term顺序；frequency:先按文档频率排，再按评分、term顺序排。   |
|   suggest_mode   |   建议模式，控制提供建议词的方式：missing:仅在搜索的词项在索引中不存在时才提供建议词，默认值；popular:仅建议文档频率比搜索词项高的词；always：总是提供匹配的建议词   |

##### 7.5.1 term suggester

-  term 词条建议器 ，对给输入的文本进行分词，为每个分词提供词项建议

- 请求
```java
localhost:9200/nba_latest/_search
```

- 请求体
```json
{
  "suggest": {
    "my-suggest": {
      "text": "jamse", //用户可能输入错误 jsmes写成jamse
      "term": {
        "suggest_mode": "missing",
        "field": "displayNameEn"
      }
    }
  }
}
```

- 响应
```json
{
  "took" : 3,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 0,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "suggest" : {
    "my-suggest" : [
      {
        "text" : "jamse",
        "offset" : 0,
        "length" : 5,
        "options" : [
          {
            "text" : "james",
            "score" : 0.8,
            "freq" : 5
          },
          {
            "text" : "jamal",
            "score" : 0.6,
            "freq" : 2
          },
          {
            "text" : "jake",
            "score" : 0.5,
            "freq" : 1
          },
          {
            "text" : "jose",
            "score" : 0.5,
            "freq" : 1
          }
        ]
      },
      {
        "text" : "hardne",
        "offset" : 6,
        "length" : 6,
        "options" : [
          {
            "text" : "harden",
            "score" : 0.8333333,
            "freq" : 1
          }
        ]
      }
    ]
  }
}

```

##### 7.5.2 phrase suggester

-  phrase 短语建议，在term的基础上，会考量多个term之间的关系，比如是否同时出现在索引的原文⾥里，相邻程度，以及词频等

- 请求
```java
localhost:9200/nba_latest/_search
```

- 请求体
```json
{
  "suggest": {
    "my-suggest": {
      "text": "jamse hardne",
      "phrase": { // 词组 对单词进行联系
        "field": "displayNameEn"
      }
    }
  }
}
```

- 响应
```json
{
  "took" : 26,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 0,
      "relation" : "eq"
    },
    "max_score" : null,
    "hits" : [ ]
  },
  "suggest" : {
    "my-suggest" : [
      {
        "text" : "jamse hardne",
        "offset" : 0,
        "length" : 12,
        "options" : [
          {
            "text" : "james hardne",
            "score" : 0.0025682184
          },
          {
            "text" : "jamal hardne",
            "score" : 0.0016773979
          },
          {
            "text" : "jamse harden",
            "score" : 0.0016222595
          },
          {
            "text" : "jake hardne",
            "score" : 0.001299489
          },
          {
            "text" : "jose hardne",
            "score" : 0.001299489
          }
        ]
      }
    ]
  }
}

```

##### 7.5.3 completion suggester

- Completion 完成建议

- 请求
```java
localhost:9200/nba_latest/_search
```

- 请求体
```json
{
  "suggest": {
    "my-suggest": {
      "text": "Miam",
      "completion":{
      "field": "teamCityEn"
      }
    }
  }
}
```

### 八、NBA中国官网实战

- 官方网站
` https://china.nba.com/playerindex/ `

#### 8.1 项目搭建

- springboot 整合 elasticsearch 和 mysql

##### 8.1.1 POM依赖

```xml
        <!-- elasticsearch-rest-high-level-client -->
        <dependency>
            <groupId>org.elasticsearch.client</groupId>
            <artifactId>elasticsearch-rest-high-level-client</artifactId>
            <version>7.2.1</version>
        </dependency>
        <!-- elasticsearch -->
        <dependency>
            <groupId>org.elasticsearch</groupId>
            <artifactId>elasticsearch</artifactId>
            <version>7.2.1</version>
        </dependency>
```
##### 8.1.2 YML依赖

```yaml
elasticsearch:
  host: localhost
  port: 9200
```

##### 8.1.3 ElasticSearch配置文件

```java
package com.frame.elasticsearch.config;

import lombok.Data;
import org.apache.http.HttpHost;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestHighLevelClient;
import org.springframework.boot.SpringBootConfiguration;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;

/**
 * ElasticSearch配置文件
 * Es7使用RestHighLevelClient操作ES
 */
@SpringBootConfiguration
@ConfigurationProperties(prefix = "elasticsearch")
@Data
public class ElasticSearchConfig {
    private String host;
    private Integer port;

    /**
     * 创建RestHighLevelClient 实例
     */
    @Bean
    public RestHighLevelClient restHighLevelClient() {
        HttpHost http = new HttpHost(host, port, "http");
        return new RestHighLevelClient(RestClient.builder(http));
    }
}

```

##### 8.1.4 ElasticSearch CRUD入门操作

- 对象转Map工具类

```java
import org.springframework.cglib.beans.BeanMap;

import java.util.HashMap;
import java.util.Map;

public class BeanUtils {
    /**
     * 对象转为Map
     */
    public static <T> Map<String, Object> beanToMap(T bean) {
        HashMap<String, Object> map = new HashMap<>();
        if (bean != null) {
            BeanMap beanMap = BeanMap.create(bean);
            for (Object o : beanMap.keySet()) {
                if (beanMap.get(o) != null) {
                    map.put(o.toString(), beanMap.get(o));
                }
            }
        }
        return map;
    }
}
```
 
- CRUD 实现方法

```java
import com.alibaba.fastjson.JSON;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.frame.common.entity.NbaPlayer;
import com.frame.common.utils.BeanUtils;
import com.frame.elasticsearch.mapper.NbaPlayerMapper;
import com.frame.elasticsearch.service.NbaPlayerService;
import org.elasticsearch.action.delete.DeleteRequest;
import org.elasticsearch.action.delete.DeleteResponse;
import org.elasticsearch.action.get.GetRequest;
import org.elasticsearch.action.get.GetResponse;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.index.IndexResponse;
import org.elasticsearch.action.update.UpdateRequest;
import org.elasticsearch.action.update.UpdateResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.index.reindex.BulkByScrollResponse;
import org.elasticsearch.index.reindex.DeleteByQueryRequest;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.util.Map;

/**
 * <p>
 * 服务实现类
 * </p>
 *
 * @author Crazy.X
 * @since 2019-11-10
 */
@Service
public class NbaPlayerServiceImpl implements NbaPlayerService {

    /* ES索引 */
    private final String NBA_INDEX = "nba_latest";

    @Autowired
    private RestHighLevelClient restHighLevelClient;

    /**
     * 添加一条文档
     */
    @Override
    public boolean addPlayer(NbaPlayer player, String id) throws IOException {
        // 获取 IndexRequest
        IndexRequest request = new IndexRequest(NBA_INDEX)
                .id(id)
                .source(BeanUtils.beanToMap(player));
        // 获取 IndexResponse
        IndexResponse response = restHighLevelClient.index(request, RequestOptions.DEFAULT);
        System.out.println(JSON.toJSONString(response));
        return false;
    }

    /**
     * 获取一条文档
     */
    @Override
    public Map<String, Object> getPlayer(String id) throws IOException {
        // 获取 GetRequest
        GetRequest getRequest = new GetRequest(NBA_INDEX, id);
        // 获取 GetResponse
        GetResponse getResponse = restHighLevelClient.get(getRequest, RequestOptions.DEFAULT);
        // 返回结果
        return getResponse.getSource();
    }

    /**
     * 更新一条文档
     */
    @Override
    public boolean updatePlayer(NbaPlayer nbaPlayer, String id) throws IOException {
        // 获取 UpdateRequest
        UpdateRequest updateRequest = new UpdateRequest(NBA_INDEX, id)
                .doc(BeanUtils.beanToMap(nbaPlayer));
        // 获取 UpdateResponse
        UpdateResponse update = restHighLevelClient.update(updateRequest, RequestOptions.DEFAULT);
        System.out.println(JSON.toJSONString(update));
        return true;
    }

    /**
     * 删除一条文档
     */
    @Override
    public boolean deletePlayer(String id) throws IOException {
        // 获取 DeleteRequest
        DeleteRequest deleteRequest = new DeleteRequest(NBA_INDEX, id).id(id);
        // 获取 DeleteResponse
        DeleteResponse delete = restHighLevelClient.delete(deleteRequest, RequestOptions.DEFAULT);
        System.out.println(delete);
        return true;
    }

    /**
     * 删除所有文档
     * 先查询所所有在进行删除
     */
    @Override
    public boolean deleteAllPlayer() throws IOException {
        // 获取 deleteByQueryRequest
        DeleteByQueryRequest deleteByQueryRequest = new DeleteByQueryRequest(NBA_INDEX);
        // 获取 BulkByScrollResponse
        BulkByScrollResponse bulkByScrollResponse = restHighLevelClient.deleteByQuery(
                deleteByQueryRequest,
                RequestOptions.DEFAULT
        );
        return true;
    }
}
```

- CRUD 测试类

```java
import com.frame.common.entity.NbaPlayer;
import com.frame.elasticsearch.service.NbaPlayerService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

import java.io.IOException;
import java.util.Map;

@RunWith(SpringRunner.class)
@SpringBootTest
public class NbaPlayerServiceImplTest {

    @Autowired
    NbaPlayerService nbaPlayerService;

    @Test
    public void addPlayer() throws IOException {
        NbaPlayer nbaPlayer = new NbaPlayer()
                .setId(999)
                .setDisplayName("苍井空");
        boolean result = nbaPlayerService.addPlayer(nbaPlayer, "999");
    }

    @Test
    public void getPlayer() throws IOException {
        Map<String, Object> player = nbaPlayerService.getPlayer("999");
        System.out.println(player);
    }


    @Test
    public void updatePlayer() throws IOException {
        NbaPlayer nbaPlayer = new NbaPlayer()
                .setId(999)
                .setDisplayName("小泽玛利亚");
        boolean result = nbaPlayerService.updatePlayer(nbaPlayer, "999");
    }

    @Test
    public void deletePlayer() throws IOException {
        boolean result = nbaPlayerService.deletePlayer("999");
    }

    @Test
    public void deleteAllPlayer() throws IOException {
        boolean result = nbaPlayerService.deleteAllPlayer();
    }
}
```

#### 8.2 接口开发

- Result结果集Vo

```java
import com.frame.common.enums.ResultEnum;
import lombok.AllArgsConstructor;
import lombok.Data;

/**
 * 结果封装Vo
 */
@Data
@AllArgsConstructor
public class Result<T> {

    private Integer code;
    private String msg;
    private T data;

    public static <T> Result success() {
        return success(null);
    }

    public static <T> Result success(T data) {
        return success(ResultEnum.SUCCESS.getCode(), data);
    }

    public static <T> Result success(Integer code, T data) {
        return success(code, ResultEnum.SUCCESS.getMsg(), data);
    }

    public static <T> Result success(String msg, T data) {
        return success(ResultEnum.SUCCESS.getCode(), msg, data);
    }

    public static <T> Result success(Integer code, String msg, T data) {
        return new Result<T>(code, msg, data);
    }

    public static <T> Result fail() {
        return fail(ResultEnum.FAIL.getCode());
    }

    public static <T> Result fail(Integer code) {
        return fail(code, ResultEnum.FAIL.getMsg());
    }

    public static <T> Result fail(String msg) {
        return fail(ResultEnum.FAIL.getCode(), msg);
    }

    public static <T> Result fail(Integer code, String msg) {
        return success(code, msg, null);
    }
}
```
- 结果集对应的枚举

```java
import lombok.AllArgsConstructor;
import lombok.Getter;

/**
 * Result结果集枚举类
 */
@Getter
@AllArgsConstructor
public enum ResultEnum {

    SUCCESS(200, "操作成功"),
    FAIL(400,"操作失败")
    ;
    private Integer code;
    private String msg;
}

```


##### 8.2.1 将数据库数据导入到elastic search

- controller层

```java
    /**
     * 从mysql导入数据到es
     */
    @GetMapping("/import")
    public Result importAllPlayer() {
        try {
            nbaPlayerService.importAllPlayer();
            return Result.success("数据导入成功");
        } catch (IOException e) {
            log.error("ElasticSearch导入数据失败");
            e.printStackTrace();
            return Result.fail("ElasticSearch导入数据失败");
        }
    }
```

- service层

```java
    /**
     * 从mysql导入数据到es
     */
    @Override
    @Transactional
    public boolean importAllPlayer() throws IOException {
        // 查询所有数据,这里使用MybatisPlus不作解释
        List<NbaPlayer> nbaPlayers = list();
        for (NbaPlayer e : nbaPlayers) {
            // 此处调用的增加文档方法
            addPlayer(e, String.valueOf(e.getId()));
        }
        return true;
    }
```
##### 8.2.2 通过姓名查找球员

- controller层

```java
    /**
     * 通过姓名查找球员
     */
    @GetMapping("/searchMatch")
    public Result searchMatch(@RequestParam("key") String key,
                              @RequestParam("val") String val,
                              @RequestParam(value = "page", required = false, defaultValue = "0") Integer page,
                              @RequestParam(value = "limit", required = false, defaultValue = "10") Integer limit) {
        List<NbaPlayer> nbaPlayers = null;
        try {
            nbaPlayers = nbaPlayerService.searchTerm(key, val, page, limit);
        } catch (IOException e) {
            log.error("searchMatch失败,参数[key={}]][val={}]]", key, val);
            e.printStackTrace();
        }
        return Result.success(nbaPlayers);
    }
```

- service层

```java
    /**
     * 通过姓名查找球员
     */
    @Override
    public List<NbaPlayer> searchMatch(String key, String val, Integer page, Integer limit) throws IOException {
        // 获取 SearchRequest
        SearchRequest searchRequest = new SearchRequest(NBA_INDEX);
        // 创建 SearchSourceBuilder 用于查询语句
        SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();
        searchSourceBuilder
                // 查询 通过match查询 key 字段名称 val 字段值
                .query(QueryBuilders.termQuery(key, val))
                // 起始页
                .from(page)
                // 显示条数
                .size(limit);
        // 设置 请求源 理解为设置查询语句
        searchRequest.source(searchSourceBuilder);
        // 获取 SearchResponse 查询数据
        SearchResponse searchResponse = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);
        // 获取碰撞之后的结果 (固定写法)
        SearchHit[] hits = searchResponse.getHits().getHits();
        // 进行对象转换并返回
        return Stream.of(hits)
                .map(e -> JSON.parseObject(e.getSourceAsString(), NbaPlayer.class))
                .collect(Collectors.toList());
    }
```

##### 8.2.3 通过国家或者球队查询球员

- controller层

```java
    /**
     * 通过国家或者球队查询球员
     */
    @GetMapping("/searchTerm")
    public Result searchTerm(@RequestParam(value = "country", required = false) String country,
                             @RequestParam(value = "teamName", required = false) String teamName,
                             @RequestParam(value = "page", required = false, defaultValue = "0") Integer page,
                             @RequestParam(value = "limit", required = false, defaultValue = "10") Integer limit) {
        List<NbaPlayer> nbaPlayers = null;
        try {
            // 路由选择国家 或者 球队
            if (StringUtils.isNotBlank(country)) {
                nbaPlayers = nbaPlayerService.searchTerm("country", country, page, limit);
            } else {
                nbaPlayers = nbaPlayerService.searchTerm("teamName", teamName, page, limit);
            }
        } catch (IOException e) {
            log.error("searchTerm失败,参数[country={}]][teamName={}]]", country, teamName);
            e.printStackTrace();
        }
        return CollectionUtils.isNotEmpty(nbaPlayers) ? Result.success(nbaPlayers) : Result.success();
    }
```

- service层

```java
    /**
     * 通过国家或者球队查询球员
     */
    @Override
    public List<NbaPlayer> searchTerm(String key, String val, Integer page, Integer limit) throws IOException {
        // 获取 SearchRequest
        SearchRequest searchRequest = new SearchRequest(NBA_INDEX);
        // 创建 SearchSourceBuilder 用于查询语句
        SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();
        searchSourceBuilder
                // 查询 通过match查询 key 字段名称 val 字段值
                .query(QueryBuilders.termQuery(key, val))
                // 起始页
                .from(page)
                // 显示条数
                .size(limit);
        // 设置 请求源 理解为设置查询语句
        searchRequest.source(searchSourceBuilder);
        // 获取 SearchResponse 查询数据
        SearchResponse searchResponse = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);
        // 获取碰撞之后的结果 (固定写法)
        SearchHit[] hits = searchResponse.getHits().getHits();
        // 进行对象转换并返回
        return Stream.of(hits)
                .map(e -> JSON.parseObject(e.getSourceAsString(), NbaPlayer.class))
                .collect(Collectors.toList());
    }
```

##### 8.2.4 通过姓名字母查找球员

- controller层

```java
    /**
     * 通过姓名字母查找球员
     */
    @GetMapping("/searchPrefix")
    public Result searchPrefix(@RequestParam(value = "prefix", required = false, defaultValue = "A") String prefix,
                               @RequestParam(value = "page", required = false, defaultValue = "0") Integer page,

                               @RequestParam(value = "limit", required = false, defaultValue = "10") Integer limit) {
        List<NbaPlayer> nbaPlayers = null;
        try {
            nbaPlayers = nbaPlayerService.searchPrefix(prefix, page, limit);
        } catch (IOException e) {
            log.error("searchPrefix失败,参数[prefix={}]]", prefix);
            e.printStackTrace();
        }
        return CollectionUtils.isNotEmpty(nbaPlayers) ? Result.success(nbaPlayers) : Result.success();
    }
```   

- service层

```java
    /**
     * 通过姓名字母查找球员
     */
    @Override
    public List<NbaPlayer> searchPrefix(String prefix, Integer page, Integer limit) throws IOException {
        // 获取 SearchRequest
        SearchRequest searchRequest = new SearchRequest(NBA_INDEX);
        // 创建 SearchSourceBuilder 用于查询语句
        SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();
        searchSourceBuilder
                // 查询 通过prefix查询 displayNameEn.keyword 名字字段 prefix 前缀字母
                .query(QueryBuilders.prefixQuery("displayNameEn.keyword", prefix))
                // 起始页
                .from(page)
                // 显示条数
                .size(limit);
        // 设置 请求源 理解为设置查询语句
        searchRequest.source(searchSourceBuilder);
        // 获取 SearchResponse 查询数据
        SearchResponse searchResponse = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);
        // 获取碰撞之后的结果 (固定写法)
        SearchHit[] hits = searchResponse.getHits().getHits();
        // 进行对象转换并返回
        return Stream.of(hits)
                .map(e -> JSON.parseObject(e.getSourceAsString(), NbaPlayer.class))
                .collect(Collectors.toList());
    }
```

### 九、 走入高可用分布式集群


### 十、 深入挖掘ElasticSearch原理

------------------
####  

- 请求
```java

```

- 请求体
```json

```

- 响应
```json

```
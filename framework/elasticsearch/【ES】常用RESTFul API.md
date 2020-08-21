
#############################################创建、更新#############################################

# 创建索引
PUT /testindex

# 往索引里面添加文档,写入了文档并不会马上能够被搜索，有个刷新的等待时间 默认是1秒
# 可以使用 ?refresh=true 来显示立即刷新
PUT /testindex/_doc/1
{
  "name":"zhangsan",
  "sex":"woman",
  "location":{
    "lot":"23.333",
    "lon":"12.332"
  }
}

# 增加一个文档，而不指定文档的id   返回的id x4-IC3QBdO5pIG83P__i
POST /testindex/_doc 
{
  "name":"随机ID文档"
}

# 如果想添加一个新的文档而不是在原有的基础上更新。添加参数 op_type=create ，如果已经存在文档，就会发生错误。如果是需要覆盖之前已经存在的 需要使用 op_type=create
PUT /testindex/_doc/1?op_type=index
{
  "name":"zhangsi"
}

# 更新文档  当再次POST或者PUT一个已经存在的文档的时候，会执行更新操作
# 使用PUT更新文档的时候需要填充每个字段,如果字段未填写完 则会丢失未填写的字段
# 使用POST更新文档可以只更新需要的字段
POST /testindex/_doc/2
{
  "name":"zhangsi"
}

# 当不知道文档的ID，并且需要更新通过指定条件查询出来的文档的内容时
POST testindex/_update_by_query
{
  "query":{
      "match": {
        "name": "zhangsan"
      }
    },
  "script": {
    "source": "ctx._source.name=params.name;ctx._source.sex = params.sex",
    "lang":"painless",
    "params": {
      "name":"lisi",
      "sex":"man"
    }
  }
}

# 使用script来更新文档
POST testindex/_update/1
{
  "script": {
    "source":"ctx._source.name=params.name",
    "lang":"painless",
    "params": {
      "name":"张三"
    }
  }
}

# 给文档添加字段，如果文档不存在就创建一个文档
POST testindex/_update/1
{
  "doc": {
    "age":12
  },
  "doc_as_upsert":true
}





# 删除文档:删除ID为1的文档
DELETE /testindex/_doc/1

# 根据条件删除文档: 删除name等于张三的文档
POST testindex/_delete_by_query
{
  "query":{
    "match":{
      "name":"张三"
    }
  }
}

# 批处理

POST _bulk
{ "index" : { "_index" : "twitter", "_id": 1} }
{"user":"双榆树-张三","message":"今儿天气不错啊，出去转转去","uid":2,"age":20,"city":"北京","province":"北京","country":"中国","address":"中国北京市海淀区","location":{"lat":"39.970718","lon":"116.325747"}}
{ "index" : { "_index" : "twitter", "_id": 2 }}
{"user":"东城区-老刘","message":"出发，下一站云南！","uid":3,"age":30,"city":"北京","province":"北京","country":"中国","address":"中国北京市东城区台基厂三条3号","location":{"lat":"39.904313","lon":"116.412754"}}
{ "index" : { "_index" : "twitter", "_id": 3} }
{"user":"东城区-李四","message":"happy birthday!","uid":4,"age":30,"city":"北京","province":"北京","country":"中国","address":"中国北京市东城区","location":{"lat":"39.893801","lon":"116.408986"}}
{ "index" : { "_index" : "twitter", "_id": 4} }
{"user":"朝阳区-老贾","message":"123,gogogo","uid":5,"age":35,"city":"北京","province":"北京","country":"中国","address":"中国北京市朝阳区建国门","location":{"lat":"39.718256","lon":"116.367910"}}
{ "index" : { "_index" : "twitter", "_id": 5} }
{"user":"朝阳区-老王","message":"Happy BirthDay My Friend!","uid":6,"age":50,"city":"北京","province":"北京","country":"中国","address":"中国北京市朝阳区国贸","location":{"lat":"39.918256","lon":"116.467910"}}
{ "index" : { "_index" : "twitter", "_id": 6} }
{"user":"虹桥-老吴","message":"好友来了都今天我生日，好友来了,什么 birthday happy 就成!","uid":7,"age":90,"city":"上海","province":"上海","country":"中国","address":"中国上海市闵行区","location":{"lat":"31.175927","lon":"121.383328"}}


#############################################搜索###############################################
# 获取集群信息
GET _cluster/state

# 获取索引信息
GET /bank/_settings

#获取所有索引的详细信息
GET _cat/indices?v

# 获取整个文档的映射信息
GET /testindex/_doc/1

# 获取文档的source部分 ， 在7.8中该方法已经过时
GET /testindex/_doc/1/_source

# 获取文档的source部分的指定字段
GET /testindex/_doc/1/?_source=location

# 获取多个文档内容
GET _mget
{
  "docs":[
    {
      "_index":"testindex",
      "_id":1
    },
    {
      "_index":"custom",
      "_id":"1"
    }
  ]
}

# 同时请求id 为 1和2 的文档

GET _mget
{
    "docs":[
      {
        "_index":"testindex",
        "_id":1
      },
      {
        "_index":"testindex",
        "_id":2
      }
    ]
}

# 将上面请求另一种写法
GET /testindex/_mget
{
  "ids":[1,2]
}

# 查询文档
GET twitter/_search


GET twitter/_search
{
  "query": {
    "match": {
      "city": "北京"
    }
  },
  "size": 0, 
  "aggs": {
    "按年龄分组": {
      "terms": {
        "field": "age"
      }
    }
  }
}
# 返回 当前集群的所有索引，默认返回10条
GET /_search?size=5

# 进行分页查询
GET twitter/_search?size=20&from=0

# filter_path 来控制输出的字段
GET twitter/_search?filter_path=hits.total

# 通过source来得到想要的结果:::↓↓source里面只会返回city和user两个字段
POST twitter/_search
{
  "_source": ["user","city"],
  "query": {
    "match_all": {}
  }
}

# 将source设置为false 就不会返回source字段
POST twitter/_search
{
  "_source": false,
  "query":{
    "match_all": {}
  }
}

# 使用 include 和exclude 来选择和排除某些想要的字段,可以使用通配符 *
POST twitter/_search
{
  "query": {
    "match_all": {}
  }, 
  "_source": {
    "includes": [
        "user*",
        "location*"
    ],
    "excludes": [
      "location.lon"  
    ]
  }
}

# 返回出生年份
GET twitter/_search
{
  "query": {
    "match_all": {}
  },
  "_source": ["age"], 
  "script_fields": {
    "出生日期":{
      "script": "2019 - doc['age'].value"
    }
  }
}

# 统计索引中文档的数量
GET twitter/_count

# 统计满足条件的文档的数量
GET twitter/_count
{
  "query": {
    "match": {
      "city": "北京"
    }
  }
}


# 获取设置信息
GET twitter/_settings

# 获取索引的映射信息 , 可以查看每个字段的类型。keyword类型是不能进行分词的,text类型的才能进行分词
GET twitter/_mapping


##### 重新设置索引的映射信息，如果重新设置了mapping以前的索引就不能被搜索，所以必须删除 重新创建
DELETE twitter
PUT twitter
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 1
  }
}
 
PUT twitter/_mapping
{
  "properties": {
    "address": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "age": {
      "type": "long"
    },
    "city": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "country": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "location": {
      "type": "geo_point"
    },
    "message": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "province": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    },
    "uid": {
      "type": "long"
    },
    "user": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword",
          "ignore_above": 256
        }
      }
    }
  }
}

# 通过条件来获取文档 , 根据相关性来搜索文档---匹配的关键词越多 排序越靠前
GET twitter/_search
{
  "query": {
    "match": {
      "city": "北"
    }
  }
}

# 使用term和keyword来实现精确搜索
GET twitter/_search
{
  "query": {
    "bool": {
      "filter": {
        "term": {
          "city.keyword": "北"
        }
      }
    }
  }
}

# 使用 match query 实现更加精确的匹配:: 要匹配 这几个字中的其中三个字
GET twitter/_search
{
  "query": {
    "match": {
      "user": {
        "query": "朝阳区-老贾",
        "operator": "or",
        "minimum_should_match": 3
      }
    }
  },
  
  "highlight": {
    "fields": {
      "user": {}
    },
    "pre_tags": "<em class=\"c_color\">",
    "post_tags": "</em>"
  }
}
## highlight 高亮字段

# should ： 会影响score 如果含有则相关性更高
GET twitter/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {
          "country": "中国"
        }
        }
      ], 
      "should": [
        {
          "match": {
            "city": "北京"
          }
        }
      ]
    }
  }
}


######################## 位置查询

# 1.查找地址为北京 ，并且 在坐标 (116.454182,39.920086)附近3km的人
GET twitter/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "address": "北京"
          }
        }
      ]
    }
  },
  "post_filter": {
    "geo_distance": {
      "distance": "3km",
      "location": {
        "lat": 39.920086,
        "lon": 116.454182
      }
    }
  }
}

# 2.查找附近5Km的文档，并且按距离由近到远排序
GET twitter/_search
{
  
  "post_filter": {
    "geo_distance": {
      "distance": "5km",
      "location": {
        "lat": 39.920086,
        "lon": 116.454182
      }
    }
  },
  "sort": [
    {
      "_geo_distance": {
        "location": "39.920086,116.454182",
        "order": "asc",
        "unit": "km"
      }
    }
  ]
}

### 范围查询

# 1.查询年龄在30 - 40 之间的文档,并降序排列

GET twitter/_search
{
  "query": {
    "range": {
      "age": {
        "gte": 30,
        "lte": 40
      }
    }
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ]
}

### exists  文档必须包含字段才会返回true
GET twitter/_search
{
  "query": {
    "exists": {
      "field": "city"
    }
  }
}

# 检查一个文档是否存在
HEAD testindex/_doc/1



PUT twitter/_doc/20
{
  "user" : "王二",
  "message" : "今儿天气不错啊，出去转转去",
  "uid" : 20,
  "age" : 40,
  "province" : "北京",
  "country" : "中国",
  "address" : "中国北京市海淀区",
  "location" : {
    "lat" : "39.970718",
    "lon" : "116.325747"
  }
}











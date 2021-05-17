---
title: Elasticsearch 基础操作
date: 2021-05-17 09:48:45
tags: Elasticsearch
---

### 查询

1. 条件查询

```
GET news202010/_search
{
 "query": {
   "match": {
     "textSrc": "海底捞"
   }
 }
}
```

2. 字段过滤

```
GET news202010/_search
{
 "query": {
   "match": {
     "textSrc": "海底捞"
   }
 },
 "_source": ["mediaTname"]
}
```

3. 排序

```
GET news202010/_search
{
 "query": {
   "match": {
     "textSrc": "海底捞"
   }
 },
 "sort": [
   {
     "pubtime"://字段 {
       "order": "desc"
     }
   }
 ]
}
```

4. 分页

```
GET news202010/_search
{
 "query": {
   "match": {
     "textSrc": "海底捞"
   }
 },
 "sort": [
   {
     "pubtime": {
       "order": "desc"
     }
   }
 ],
 "from": 0,
 "size": 20
}
```

5. 多条件查询

```
GET news202010/_search
{
 "query": {
   "bool": {
     "must": [
       {
         "match": {
           "mediaNameSrc": "搜狐"
         }
       },
       {
         "match": {
          li "countryName": "中国"
         }
       }
     ]
   }
 }
}
// must 相当于 mysql 中 where id = 1 and name=xxx
// should 相当于 or
```
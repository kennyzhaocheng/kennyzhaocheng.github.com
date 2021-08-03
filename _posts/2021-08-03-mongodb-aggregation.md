---
layout: post
title: "MongoDB Aggregation"
description: ""
category: "python"
tags: []
---
{% include JB/setup %}

## 实现以下功能：
1. 将日期截取年和月
2. 按年月Group by, 并在group内将words整合在一起
3. 按年月升序

-----
### Mongodb Compass
```
[{$project: {
  words:1,
  month : {$substr:['$date',0,7]}, 

}}, {$group: {
  _id:'$month',
  words: {
    $push: '$words'
  }
}}, {$sort: {
  _id: 1
}}]
```
----
### Python
```python
from pymongo import MongoClient
# Requires the PyMongo package.
# https://api.mongodb.com/python/current

client = MongoClient('mongodb://localhost:27017/?readPreference=primary&appname=MongoDB%20Compass&directConnection=true&ssl=false')
result = client['anlytics']['polls_baidutiebaposts'].aggregate([
    {
        '$project': {
            'words': 1, 
            'month': {
                '$substr': [
                    '$date', 0, 7
                ]
            }
        }
    }, {
        '$group': {
            '_id': '$month', 
            'words': {
                '$addToSet': '$words'
            }
        }
    }, {
        '$sort': {
            '_id': 1
        }
    }
])
```
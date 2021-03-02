---
title: "mongodb query example"
date: 2021-02-23T15:33:33+09:00
showDate: true
draft: false
tags: ["몽고디비", "mongdoDB", "mongo"]  
---

# MongoDB query sample

## 저장

```json
db.users.insert({username: "smith"})
WriteResult({"nInserted":1})
```

## 조회

```sh
db.users.count()
>> 2

db.users.find({username:"jones"})

# and
db.users.find({ $and: [
        { _id : ObjectId("60346898322b4f0e916382b6")},
        { username : "smith"}
    ]})
    
    
# or    
db.users.find({ $or: [
        { username : "smith"},
        { username : "jones"}
    ]})    


```

#### 수정

```sh

## update

db.users.updateOne({username :"jones"},
    {$set : {country: "Canada"}});
    
## 실수하는 부문

db.users.updateOne({username :"jones"},{country: "Canada"})
db.users.find({ username : "jones"})    
>>




```

#### 삭제

```sh
### delete

db.users.deleteOne({username : "smith"})

## collection drop
db.users.drop()
```

### 인덱스 생성과 질의

```sh
for ( i = 0; i < 20000; i++){
    db.numbers.insertOne({num: i});
}


## num == 500 찾기
db.numbers.find({num:500});

## 범위 쿼리
db.numbers.find({num:{$gt: 19995}})

>> 
    [
    {
        "_id": {"$oid": "603499ceadceb46ff7e6f178"},
        "num": 19996
    },
        {
            "_id": {"$oid": "603499ceadceb46ff7e6f179"},
            "num": 19997
        },
        {
            "_id": {"$oid": "603499ceadceb46ff7e6f17a"},
            "num": 19998
        },
        {
            "_id": {"$oid": "603499ceadceb46ff7e6f17b"},
            "num": 19999
        }
    ]
 
 
 ## 20 초과 25 미만
db.numbers.find({num:{ "$gt": 20, "$lt" : 25}});
 
## 20 이상 25 이하
db.numbers.find({num:{ "$gte": 20, "$lte" : 25}});

## index 생성
db.numbers.createIndex({num:1});


## index 타는 쿼리 추가
db.numbers.find({num: {"$gt": 199990}}).explain("executionStats");

  [
    {
      "executionStats": {
        "executionSuccess": true,
        "executionTimeMillis": "0.190",
        "planningTimeMillis": "0.169",
        "executionStages": {
          "stage": "IXSCAN",
          "nReturned": "0",
          "executionTimeMillisEstimate": "0.007",
          "indexName": "num_1",
          "direction": "forward"
        }
      },
      "ok": 1,
      "queryPlanner": {
        "plannerVersion": 1,
        "namespace": "user_activity.numbers",
        "winningPlan": {
          "stage": "IXSCAN",
          "indexName": "num_1",
          "direction": "forward"
        }
      },
      "serverInfo": {
        "host": "historydb-devel",
        "port": 27017,
        "version": "3.6.0"
      }
    }
  ]
  
## 범위 조회이기는 하나 index 타지는 않음. 0~ 199999 의 데이터중 아래에서 50개 자겨오는 경우 index 타지 않음  
db.numbers.find({num: {"$gt": 50}}).explain("executionStats");

  [
  {
    "executionStats": {
      "executionSuccess": true,
      "executionTimeMillis": "18.445",
      "planningTimeMillis": "0.179",
      "executionStages": {
        "stage": "COLLSCAN",
        "nReturned": "19949",
        "executionTimeMillisEstimate": "17.524"
      }
    },
    "ok": 1,
    "queryPlanner": {
      "plannerVersion": 1,
      "namespace": "user_activity.numbers",
      "winningPlan": {
        "stage": "COLLSCAN"
      }
    },
    "serverInfo": {
      "host": "historydb-devel",
      "port": 27017,
      "version": "3.6.0"
    }
  }
]
  
  
 ## 범위 조회시 index 타는 것을 볼수 있다. 
 db.numbers.find({num: {"$gte": 20, "$lte": 25}}).explain("executionStats");
 
  
  [
  {
    "executionStats": {
      "executionSuccess": true,
      "executionTimeMillis": "0.372",
      "planningTimeMillis": "0.341",
      "executionStages": {
        "stage": "IXSCAN",
        "nReturned": "6",
        "executionTimeMillisEstimate": "0.015",
        "indexName": "num_1",
        "direction": "forward"
      }
    },
    "ok": 1,
    "queryPlanner": {
      "plannerVersion": 1,
      "namespace": "user_activity.numbers",
      "winningPlan": {
        "stage": "IXSCAN",
        "indexName": "num_1",
        "direction": "forward"
      }
    },
    "serverInfo": {
      "host": "historydb-devel",
      "port": 27017,
      "version": "3.6.0"
    }
  }
]
  
 
```





#  rename field
```sh

//rename field 
db.brand_similarity_v1.updateMany({}, {
    $rename : {"brand": "id"}
});

// Rename a Field in an Embedded Document¶
db.students.update( { _id: 1 }, { $rename: { "name.first": "name.fname" } } )

```




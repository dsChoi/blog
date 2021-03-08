---
title: "mongodb aggregate query example"
date: 2021-02-23T15:33:33+09:00
showDate: true
draft: false
tags: ["몽고디비", "mongdoDB", "mongo", "aggregate"]  
---





**match , sort, limit SQL query**

```sql
select * from brand_similarity_v1
where brand = "tulipalee"
order by score desc
limit 10
```

**mongodb aggregate**

```sql

db.brand_similarity_v1.aggregate([
    {
        $match : {
            brand : "tulipalee"
        }
    },
    {
        $sort : {
            score: -1 // descending
        }
    },
    {
        $limit : 10
    }
])
```





Mongoldb index 생성

```sql
db.brand_similarity_v1.createIndex({key: 1})


//TTL index 
db.brand_similarity_v1.createIndex( { "ts": 1 }, { expireAfterSeconds: 3600 } )

// brand_similarity_v1 colllection 에 key column 을 오름차순 index 생성
```




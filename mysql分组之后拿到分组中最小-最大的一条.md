title: mysql分组之后拿到分组中最小/最大的一条
date: 2016-06-22 18:05:33
tags:
---

先排序到临时表之后在分组。

```
<!-- 首次充值相关信息 -->
<sql id="firstChargeInfoSQL">
      SELECT
           companyid as companyid ,
            chargeid as firstChargeId,
            addtime as firstChargeT
ime,
            currentmoney as firstChargeAmount
      FROM
      (
           SELECT
                companyid,
                chargeid,
                addtime,
                currentmoney
           FROM ${database_51p2badmin}.pb_bchargelog
           WHERE status = 2 ORDER BY addtime ASC
      ) temp
      GROUP BY companyid
 </sql >
```
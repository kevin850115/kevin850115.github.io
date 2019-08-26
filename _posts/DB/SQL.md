
### 在线率
```SQL
SELECT 出票信息.日期,出票信息.出票_总量,出票信息.在线_退改_出票_总量,CONCAT(ROUND(出票信息.在线_退改_出票_总量/出票信息.出票_总量*100, 2), '%') AS 境外航司_退款_在线率

FROM
(
SELECT

TO_CHAR (s_ttp_order_ticket.gmt_create,'yyyy-mm-dd')as 日期,

COUNT(*) as 出票_总量,

SUM(IF(s_ttp_order_ticket.`agent_id` in (4794)  , 1, 0)) AS 在线_退改_出票_总量

FROM  tbods.s_ttp_order_ticket

WHERE ds =     TO_CHAR (dateadd(GETDATE() , -1, 'dd') ,'yyyymmdd')

AND ticket_status =1

AND agent_id != 2052

AND gmt_create >dateadd(GETDATE() , -30, 'dd')

AND agent_id in(3411,4510,4735,4757,4772,4778,4779,4794,4832,4872,4913,4915,4916,4945,4965,5000,5038,5039,5061,5074,5094,5104,5105,5107,5147,5200,5225,5245,5267)

GROUP BY

TO_CHAR (s_ttp_order_ticket.gmt_create,'yyyy-mm-dd')

order by TO_CHAR (s_ttp_order_ticket.gmt_create,'yyyy-mm-dd')
) 出票信息
```

### 退票时长
```sql
SELECT

ROUND(SUM(IF(s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd')  , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`gmt_refund_succ_time`  >dateadd(GETDATE() , -7, 'dd')  , 1, 0)),2)   AS `7天_总_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`gmt_refund_succ_time`  >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , 1, 0)),2)   AS `7天_总_自愿_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd')  AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4)   , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`gmt_refund_succ_time`  >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4)   , 1, 0)),2)   AS `7天_总_非自愿_平均退票时长[小时]` ,


ROUND(SUM(IF(s_trp_apply.`seller_nick` like '%旗舰店' AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` like '%旗舰店'  AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd'), 1, 0)),2)   AS `7天_航司_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`seller_nick` like '%旗舰店' AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` like '%旗舰店'  AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , 1, 0)),2)   AS `7天_航司_自愿_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`seller_nick` like '%旗舰店' AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4)   , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` like '%旗舰店'  AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4)  , 1, 0)),2)   AS `7天_航司_非自愿_平均退票时长[小时]` ,


ROUND(SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店' AND s_trp_apply.`agent_id`  not in (4632,4703,5008,5078,5080,5122,4972)  AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd'), DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店' AND s_trp_apply.`agent_id`  not in (4632,4703,5008,5078,5080,5122,4972)  AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd'), 1, 0)),2)   AS `7天_代理人_平均退票时长[小时]` ,


ROUND(SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店' AND s_trp_apply.`agent_id`  not in (4632,4703,5008,5078,5080,5122,4972)  AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店' AND s_trp_apply.`agent_id`  not in (4632,4703,5008,5078,5080,5122,4972)   AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , 1, 0)),2)   AS `7天_代理人_自愿_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店' AND s_trp_apply.`agent_id` AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店'  AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , 1, 0)),2)   AS `7天_代理人(含自营)_自愿_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店' AND s_trp_apply.`agent_id`  not in (4632,4703,5008,5078,5080,5122,4972)   AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4)  , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店' AND s_trp_apply.`agent_id`  not in (4632,4703,5008,5078,5080,5122,4972)   AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4)  , 1, 0)),2)   AS `7天_代理人_非自愿_平均退票时长[小时]` ,


ROUND(SUM(IF(s_trp_apply.`agent_id` in (4632,4703,5008,5078,5080,5122,4972) AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd'), DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`agent_id`  in (4632,4703,5008,5078,5080,5122,4972) AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') , 1, 0)),2)   AS `7天_自营_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`agent_id` in (4632,4703,5008,5078,5080,5122,4972) AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`agent_id`  in (4632,4703,5008,5078,5080,5122,4972) AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4)  , 1, 0)),2)   AS `7天_自营_自愿_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`agent_id` in (4632,4703,5008,5078,5080,5122,4972) AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd') AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4) , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`agent_id`  in (4632,4703,5008,5078,5080,5122,4972) AND s_trp_apply.`gmt_refund_succ_time` >dateadd(GETDATE() , -7, 'dd')  AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4) , 1, 0)),2)   AS `7天_自营_非自愿_平均退票时长[小时]` ,


ROUND(SUM(DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh')) / COUNT(0) ,2)   AS `30天_总_平均退票时长[小时]` ,

ROUND(SUM(IF(GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , 1, 0)),2)   AS `30天_总_自愿_平均退票时长[小时]` ,

ROUND(SUM(IF(GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4)   , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4)   , 1, 0)),2)   AS `30天_总_非自愿_平均退票时长[小时]` ,


ROUND(SUM(IF(s_trp_apply.`seller_nick` like '%旗舰店' , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` like '%旗舰店' , 1, 0)),2)   AS `30天_航司_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`seller_nick` like '%旗舰店' AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4)  , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` like '%旗舰店' AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4)  , 1, 0)),2)   AS `30天_航司_自愿_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`seller_nick` like '%旗舰店' AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4)  , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` like '%旗舰店' AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4)  , 1, 0)),2)   AS `30天_航司_非自愿_平均退票时长[小时]` ,


ROUND(SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店'  AND s_trp_apply.`agent_id`  not in (4632,4703,5008,5078,5080,5122,4972)  , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店'  AND s_trp_apply.`agent_id`  not in (4632,4703,5008,5078,5080,5122,4972)  , 1, 0)),2)   AS `30天_代理人_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店'  AND s_trp_apply.`agent_id`  not in (4632,4703,5008,5078,5080,5122,4972)   AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店'  AND s_trp_apply.`agent_id`  not in (4632,4703,5008,5078,5080,5122,4972)   AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , 1, 0)),2)   AS `30天_代理人_自愿_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店'  AND s_trp_apply.`agent_id`  not in (4632,4703,5008,5078,5080,5122,4972)  AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4)  , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`seller_nick` not like '%旗舰店'  AND s_trp_apply.`agent_id`  not in (4632,4703,5008,5078,5080,5122,4972)  AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4)   , 1, 0)),2)   AS `30天_代理人_非自愿_平均退票时长[小时]` ,


ROUND(SUM(IF(s_trp_apply.`agent_id` in (4632,4703,5008,5078,5080,5122,4972) , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`agent_id`  in (4632,4703,5008,5078,5080,5122,4972)  , 1, 0)),2)   AS `30天_自营_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`agent_id` in (4632,4703,5008,5078,5080,5122,4972)  AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4) , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`agent_id`  in (4632,4703,5008,5078,5080,5122,4972)   AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') IN (0,2,3,4)  , 1, 0)),2)   AS `30天_自营_自愿_平均退票时长[小时]` ,

ROUND(SUM(IF(s_trp_apply.`agent_id` in (4632,4703,5008,5078,5080,5122,4972)  AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4) , DATEDIFF(s_trp_apply.gmt_refund_succ_time,s_trp_apply.gmt_create,'hh'), 0))
/ SUM(IF(s_trp_apply.`agent_id`  in (4632,4703,5008,5078,5080,5122,4972)   AND  GET_JSON_OBJECT(attributes,'$.RETURN_TYPE') NOT IN (0,2,3,4) , 1, 0)),2)   AS `30天_自营_非自愿_平均退票时长[小时]`


FROM  tbods.s_trp_apply
WHERE ds =    TO_CHAR (dateadd(GETDATE() , -1, 'dd') ,'yyyymmdd')
AND biz_type =2 --国际机票
AND apply_type =1 --退票单
AND agent_id != 2052
AND status = 40
AND gmt_refund_succ_time >dateadd(GETDATE() , -30, 'dd') ;

```

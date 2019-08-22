

### 逆向SQL
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

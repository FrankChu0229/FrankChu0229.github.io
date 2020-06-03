title: Hive HQL Summary
date: 2019-05-19 15:11:39
tags: [big data, data engineering]
categories: Data Engineering
description: Hive HQL Summary 
---

在企业中，和线上数据打交道的最简单方式可能就是使用如hive，impala等平台了；用户只需会写SQL语句即可。在hive平台上，与它对应的是HQL语句，HQL基本支持SQL的全部操作，除此之外，他还支持`array`, `struct`, `map`等数据结构，具有更强大的能力。


## 通过SQL方式在hive上进行查询等操作

| **Function** | **MySQL** | **Hive** |
|---|---|---|
| Retrieving Information (General) | `SELECT from_columns FROM table WHERE conditions;` | `SELECT from_columns FROM table WHERE conditions;` |
| Retrieving All Values | `SELECT * FROM table;` | `SELECT * FROM table;` |
| Retrieving Some Values | `SELECT * FROM table WHERE rec_name = "value";` | `SELECT * FROM table WHERE rec_name = "value";` |
| Retrieving With Multiple Criteria | `SELECT * FROM TABLE WHERE rec1= "value1" AND rec2 = "value2";` | `SELECT * FROM TABLE WHERE rec1 ="value1" AND rec2 = "value2";` |
| Retrieving Specific Columns | `SELECT column_name FROM table;` | `SELECT column_name FROM table;` |
| Retrieving Unique Output | `SELECT DISTINCT column_name FROM table;` | `SELECT DISTINCT column_name FROM table;` |
| Sorting | `SELECT col1, col2 FROM table ORDER BY col2;` | `SELECT col1, col2 FROM table ORDER BY col2;` |
| Sorting Reverse | `SELECT col1, col2 FROM table ORDER BY col2 DESC;` | `SELECT col1, col2 FROM table ORDER BY col2 DESC;` |
| Counting Rows | `SELECT COUNT(*) FROM table;` | `SELECT COUNT(*) FROM table;` |
| Grouping With Counting | `SELECT owner, COUNT(*) FROM table GROUP BY owner;` | `SELECT owner, COUNT(*) FROM table GROUP BY owner;` |
| Maximum Value | `SELECT MAX(col_name) AS label FROM table;` | `SELECT MAX(col_name) AS label FROM table;` |
| Selecting from multiple tables (Join same table using alias w/”AS”) | `SELECT pet.name, comment FROM pet, event WHERE pet.name =event.name;` | `SELECT pet.name, comment FROM pet JOIN event ON (pet.name =event.name)` |


## 处理hive表中的`array`, `struct`等表中数据

例如，从一个trade表中，select相关的订单数据，并按照spu_id进行groupby和sum。 这里使用lateral view explode(t.order_list)将array进行展开并和已有的表进行join。
这里还需注意的一点是，展开后的array是struct结构，该struct结构包含`spu_id`, `item_count`, `payment`字段，如果直接按照struct.field进行groupby和sum会出错，所以这里使用()构建了一个临时表，在对临时表进行groupby和sum操作。

```
INSERT OVERWRITE DIRECTORY '/user/XXX/XXX'
select store_id, spu_id, sum(item_count) as item_sum, sum(payment) as payment_sum from
(select t.store_id store_id, 
orderTable.orderTable.spu_id as spu_id, 
orderTable.orderTable.item_count as item_count,
orderTable.orderTable.payment as payment 
from trade as t 
lateral view explode(t.order_list) orderTable as orderTable
where t.year = 2019 and (t.month = 5 or t.month=4 or t.month=3)
and t.store_id in (${stores})) as tmp
group by store_id, spu_id;
```

```
SELECT dwd_lu_manager_response.store_id, 
    dwd_lu_manager_response.domain, 
    dwd_lu_manager_response.query, 
    dwd_lu_manager_response.normalized_query, 
    dwd_lu_manager_response.business_id,
  dwd_lu_manager_response.intent_info[0].intent as intent, 
  dwd_lu_manager_response.intent_info[0].score as score,

    dwd_insight_response.business_id,
    dwd_insight_response.fasttext_entity_masked_tokens

FROM dwd_lu_manager_response
JOIN dwd_insight_response ON dwd_lu_manager_response.business_id = dwd_insight_response.business_id
where (dwd_lu_manager_response.dt = "20200405" or dwd_lu_manager_response.dt = "20200406") and  dwd_lu_manager_response.intent_info[0].intent != "NULL" and (array_contains(fasttext_entity_masked_tokens, "{ZHUBO_NAME}") or array_contains(fasttext_entity_masked_tokens, "{YOUHUI_NAME}"))
ORDER BY dwd_lu_manager_response.domain
limit 100000
```

```
-- step 1: create json table
-- drop table if exists tmp.xxx_spu_3c;
CREATE EXTERNAL TABLE tmp.xxx_spu_3c (
    store_id string,
    store_name string,
    store_domain string,
    spu_id string,
    category_name string,
    style_name string,
    title string,
    config_pvs array<struct<prop_name: string,
                            prop_value: string>>,
    spu_pvs array<struct<pid: string,
                         vid: string,
                         pid_name:string,
                         vid_name:string>>,
    sku_pvs array<array<struct<pid: string,
                         vid: string,
                         pid_name:string,
                         vid_name:string>>>
)
ROW FORMAT SERDE 'org.apache.hive.hcatalog.data.JsonSerDe'
LOCATION '/user/xxx/xxx_spu_3c';

-- step 2: insert sessions
with store as (
    select store_id, store_name, domain
    from tb_store.dim_store_meta
    where dt = '20200420'
    and domain = '3C数码配件'
),
property as (
    select spu_id, 
        collect_list(named_struct(
            "prop_name", prop_name,
            "prop_value", prop_value
            )
        ) as config_pvs
    from tb_item.dim_item_property_configuration
    group by spu_id
)
INSERT INTO tmp.xxx_spu_3c
select
    store.store_id, store.store_name, store.domain,
    spu.spu_id, spu.category_name, spu.style_name, spu.title,
    property.config_pvs,
    spu.property_name_infos,
    spu.skus.property_name_infos
from tb_item.dim_spu spu 
join store on spu.store_id = store.store_id 
left outer join property on spu.spu_id = property.spu_id

```

```
INSERT OVERWRITE DIRECTORY "/user/xxx/scenario_open_ratio"
WITH SCENARIO_HOLDER AS (
SELECT dt, store_id, scenario.label as scenario_label,
  scenario.active as scenario_status, scenario.policy_holders
FROM dwd_policy_stats
LATERAL VIEW explode(scenario_holders) dwd_policy_stats AS scenario
WHERE (dt = '20200531' or dt = '20200530' or dt = '20200529')
)
select dt, store_id, scenario_label, scenario_status,
  policy.label as policy_label, policy.active as policy_status
FROM SCENARIO_HOLDER
LATERAL VIEW explode(policy_holders) exploded as policy
```

## Reference

- [HQL Cheat Sheet](https://hortonworks.com/blog/hive-cheat-sheet-for-sql-users/)
- [HQL Wiki](https://cwiki.apache.org/confluence/display/Hive/Home)

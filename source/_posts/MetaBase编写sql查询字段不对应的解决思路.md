---
title: MetaBase编写sql查询时查询的字段和数据库的字段不对应、查询数据库另一字段的方法和解决思路
date: 2024-04-07 14:40:23
updated: 2024-04-07 14:47:54
tags:
- Metabase
- SQL
- 数据库
categories:
- 方法记录
---

> 本文原发布于 CSDN：<https://blog.csdn.net/Xi1yang/article/details/137463543>

这种直接写个 `case` 去替换名称即可。如下所示，传入的是 `TimeType_tran`，使用的是 `TimeType`，搜索的是中文，使用的是英文。

```sql
case
    when {{TimeType_tran}} = '年' then 'year'
    when {{TimeType_tran}} = '月' then 'month'
    when {{TimeType_tran}} = '周' then 'week'
    when {{TimeType_tran}} = '日' then 'day'
end
```

需要注意的是，Metabase 里展示给用户看的字段名、筛选器名称，和数据库中真实字段名不一定完全一致。

比如页面上展示的是中文字段，数据库里实际保存的是英文字段；或者查询时传入的是一个映射后的名称，但真正查询时需要使用数据库中的字段名。

这种情况下可以在 SQL 中先完成一次字段值映射，然后再继续参与查询。

### 根据另一个字段查询并返回目标字段

假设数据库中有两个字段：

- 字段 1：真正需要拿来查询或展示的字段。
- 字段 2：用户输入或页面展示中更容易理解的字段。

如果希望通过字段 2 查询字段 1，可以先用子查询把需要的值查出来，再放到主查询中使用。

例如：

```sql
select *
from table_name
where PatientType = (
    select PatientType
    from table_name
    where PatientType_tran = {{PatientType_tran}}
    limit 1
)
```

这里的思路是：

1. 用户传入的是 `PatientType_tran`。
2. 先根据 `PatientType_tran` 查出数据库中对应的 `PatientType`。
3. 再用查出来的 `PatientType` 作为主查询条件。

### 解决思路

遇到字段不对应的问题时，可以按下面的顺序排查：

1. 先确认 Metabase 页面上使用的字段名。
2. 再确认数据库中真实字段名。
3. 如果只是中英文、别名或枚举值不一致，可以用 `case when` 做转换。
4. 如果需要通过另一个字段反查目标字段，可以使用子查询。
5. 如果字段映射关系比较复杂，建议单独维护一张映射表。

### 小结

在 Metabase 中写 SQL 时，页面字段和数据库字段不一致是很常见的问题。

简单映射可以直接用 `case when`，如果需要通过一个字段查另一个字段，可以使用子查询，或者维护一张专门的映射表，让查询逻辑更清晰。

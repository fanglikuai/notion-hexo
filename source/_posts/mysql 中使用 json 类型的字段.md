---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TLCDBYK6%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T080056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJGMEQCICIID%2BVv74%2FArAptGoDjzupJuA%2B5k5n2PDDn5F1X4Va0AiBMDtZDDyhYXPNd66%2FyowDsWrGcpkA8p%2FMPHmMH7GhVJCqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHtaGREQuekLPM6Q%2BKtwDrQD%2FU5q4sQghkeqEv%2FuN7HjBZzzEw5E%2FtYCJTkcWShOnzVdeZL4D6kiugBmWhfmOuwVYwDNktIiQ5Wo%2F6tjUBX3BzrGrVVJHcR%2F0255DTv3i1jt00IYuNtFrZwDHIltygDVMynJ0g5ZkEgv1m5qzHv59HEMGvIvQ%2Fz8Wt4tD%2BAGEeA%2BiedZtYfy%2BsSK%2BJm6V9CgheTUOoGkoKzc7TD0t5Uv9OE%2F7Etqi98xNl2FtmlMgBZs9FqmfKw%2BpB2E7NSOza1wxHG82owz5ifeunQ%2BOsY6HWPVT1NuAFFmj5TTac17UyvOemJMHIyUnYGKVPlUnXOVZ4iMIfkGZuVwOFIPsBpHT6J52MkhogLB2ykK%2FjuA4wn3hzsX%2B%2BGAqnkvpOiLbrL0H%2FXoE2mnPGmj1OSKwXAM7Faod4y%2Bf8M%2BO%2BPPJHU0z%2FgQoQ3RrjL9x5URnblTcuWmhKGyvDyU4X5zUgMcWjZDsCCMl3l4NblAi9AeEWlhhDaNbmss%2BcMtB%2FouDe4lLF0ufR7YCEnQZeUjsypu%2Bsn4fhevME84sIjXGupLYd36c1FMnP%2FeFlob%2FCQMJcnIDmZ7sztD6gIn3Bj3iE7UwIo6MA%2FTa%2Fn8T8yYiKBD0x9wypE0P%2FGqVxulleW8w8cKdxwY6pgE%2FxE%2B5hBY607kZ1Xr2ogJogLFdTA8Al5BZNehE6D5lVgl0H2mPvE6kFLN4WWRgJFU8MfcGIDqpnrjx7P6vpCUkNo0jXmXpa18YYVcIqwXzSqQyPc2cV9K%2FaYUcC92dO64JR58HT3fV2zK7wcPiTK03MwnSEkMOsXH3C68iJWzJ%2F01V%2F0YrkTkgh9DSmzO56AE8ukRivCVjMbo%2BZ3237EQ%2Fi8aT29Vw&X-Amz-Signature=2353e24c5537b1d01eb57db845c29d30d832214f12b3f60281cc5ceb3f6e4ef2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:55:00'
index_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
banner_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
---

5.7 之后支持了 json 格式


但是在实际应用中好像不怎样


# 配置&使用流程

> springboot+mybatisplus+mysql5.7

## 代码配置


java：


![imagescce2478e5401f24de6234fcc9a70b5b4.png](/images/476a1133e7aaa3e257f0f6fe9cb407b6.png)


mysql 中的表：


![imagese0bbc4d10d8ec7819433a5e83f307a52.png](/images/e2532123fe03eee4705d5db2c2ecc85d.png)


## 配置类型转换插件


```java
package org.example.studyboot.demos.web;

import com.alibaba.fastjson2.JSONObject;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

@MappedTypes(JSONObject.class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class JsonHandler extends BaseTypeHandler<JSONObject> {

    /**
     * 设置非空参数
     *
     * @param ps
     * @param i
     * @param parameter
     * @param jdbcType
     * @throws SQLException
     */
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, JSONObject parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, String.valueOf(parameter.toJSONString()));
    }

    /**
     * 根据列名，获取可以为空的结果
     *
     * @param rs
     * @param columnName
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String sqlJson = rs.getString(columnName);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * 根据列索引，获取可以为空的结果
     *
     * @param rs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String sqlJson = rs.getString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * @param cs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String sqlJson = cs.getNString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }
}
```


在yaml 中配置：


![images944ad29a7fcf96a0c51a577d6bc43317.png](/images/4d25cc1863ee3e3fa6ae7e6d4c2a6cf7.png)


xml中配置：


![imagesd6de49b9a7b17849e0d393569b93bca5.png](/images/1067c14ea63fdd81764edc7b0b6e9828.png)


# 对比MongoDb


假设有以下数据


```json
{
  "name": "John",
  "age": 25,
  "address": {
    "street": "123 Main St",
    "city": "New York"
  }
}
```


使用嵌套查询即可


```bash
db.persons.find({"address.city": "New York"})
```


可以看到，直接被秒杀了


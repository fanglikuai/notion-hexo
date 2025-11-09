---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBJPVR46%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIGYb%2BslfyaAFKs9MJYV9fsMykDmV%2BgGZqt3WmwwtvflnAiEAjSUBfFvcsggOgZvYSoJgvIsZPWhdLwAmDuEov95eKxsqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDARIeesTFxeu3icyXCrcA%2F7azl4XNJRZanx522pBD4JpvGbUMk6Z4PNP6zH%2BpSeqq25hQ41OfBs5O%2BAXuJG7%2Bofdme2JlBBMZIqgBtxjPl4nXawj%2F15aAx0iBnOL%2F5C7I9HAUf4S3HzjjmNad0MwBffAfzWWu4I4PgLZx4YfgmV7s2mmFI4s0YokitnNEu1wqTvDGt711cZTAfrQaEd0rW0LrIBHrHo%2F5slTs2c%2BVzMM3fOoRUS0BstKUAi4esj5vCjJp%2FErLMIK41X%2BHJkb5H8n9CCP4x8aEi%2BWPOjpazqW1n2w5FiYd0ahRomgKuCAzxSN9%2Bw6IyGMVbN3J6EVMxjybxkAPMcgTadwzYvhUlV%2FifVpBoFhHOl0uinHF3GpufdkIjDfWREspKOc0Mq9Z7mgxJU16c71pjpcBKAUKasjvc1hPWYOd6qNqHNKEg0LLdkkyhT5pqWmstV2Uktl4Ocn9aEgAgq4ssbVEs5SkQXDYMGNfN4B90O5rLPmynlXuDR6sLkQlUO7RG75a0q2H9F0xQ3x1lUB9Y6JDSefjOHeU%2BXoZW016ryhFkIqfq20JmBoB2V4jxq7CG8PAbUhJ8QROF3UDrlB5cHOQvVmtiPCGO9qZbnKY%2BPny1JQy0NkYUgtKAwrmwqD1gqvMOeNwcgGOqUBn4xm%2F2%2BzqqNR%2FGwS86lE%2FTVGQ75%2BlHXcbISg9YeA9BrZz6KuERupSJA0vK9%2FoOVStcictvJe1s0rtjhg1rCDAWzWwfQihr4%2F9LcEgKeGVd4hzd2oX8I6RoQ%2FGKcDAL%2Bk8NBX8qPgZt8TKY%2FA5Mm9qVdLpTDnpB8JN4CTwr%2FDm9U4XZVA5kBW9BiR9GEYtQMidhprn8f2XfHP5qqlew%2BMNajca8Xf&X-Amz-Signature=f86f518da56aad5ed0e188ad3f5b065979cbc1fa3e8e70b897a0a2469a330fe1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


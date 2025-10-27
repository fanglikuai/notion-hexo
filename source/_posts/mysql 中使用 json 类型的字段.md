---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GU6KW5W%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFW1NVokSZdlK7ecr5F0%2Fp86mjxPgUN22JGf8LmIqh%2FaAiApem3RU57p6rSjRVgrxDfZdWrBCBA1eRiiYnHN%2B6%2FLsyqIBAig%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYqs3TRotHhvlf5jDKtwD8k2bwjjNNm6qY5GCOecw%2BvG%2B5ZXpJ2ROHgwAvS2SgE0li9sI0OYxXmYJJ7KXKRliKEKDoFojmFAJQTjP%2BUIf%2F%2FWnyFV8KCS6wym%2BT%2BYZYHgIGW01491ykK0frboRjUrh7LYWk3lwZy5WkB0ks0pyT%2BRRxcMdGpRiNNu4Tt1J%2FmYevOeD%2B92wmJqaxksAHcBbtSwIlhM3EbNnjh2iZ8iJL3ELDqZdUD8Z5kFZD%2BckqIZGgWT2%2FTf6VZc8HyCM0qFeLCUqK9wMkR397181%2F7VwPtLO5z5B53xHDWRDy%2B3BpHhcXIExfWRiv5vZcY3jcY9EoPLsleKsg86FjPc1fRm%2F557tFiBM%2FewCmeWFlt6BlD8Vm3NmFv9hG7O6wFE6emktSW6M3cldj%2BuXV%2B0tYCUhimkwEWuegkQActiqFvH%2B0uN8mlSIXwf1Jmcu4BwORzJAQbrevNIt%2BKNV42Y4z9cFuylYjf%2F8IZbbxX6Dmp4BNb7z9aL0A9%2F04UosgntFOJ4SKnqtU9LlR9SMPha%2FWxhEZqkSNMsO2uExnsvd2tXZkFpV7xZVVdJyZzZRMjlsiQUw%2FvA89VLobHrf%2FQmW5Zu6FD72POxX1aXDDT5Hg4bZdDrkRWHrtg8lT2qv1L8wprD8xwY6pgEgsFNpyLdE1%2BIsxMFa5iPpMs4JGBDjLCU4g9vSGeOMWP7sfWo0z3XeAPvHWQrDDNaO8MdIKj7lLUXHVjnypqzcPvYvLOGukBkb8AJrDs8uZsb%2BoTDGYplb%2Bco3LmVp9Ltw6n0Qq5nHUvjqDNr0AiwoD%2Bcf5o4Wf9BJ67wA6S3OjAM2cd6WbGDdz%2FR8K7YZ6Xk5pZs7PZg%2BrwXjWU1kcd2Vv%2BAvrzIW&X-Amz-Signature=d20dc25bbb21f20f8eb8ad963e85fa947748b77d86db721a97c13de403269c0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLVVAZ7P%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFcaCXVzLXdlc3QtMiJHMEUCIHONxqwDGG3t39g7rUUdilER1F3QnKB8x%2FC0dzhJW0OgAiEAsc88BM0Iqa9H0uvWmGihcu0bxCq5f72V7hKegPD%2BreMq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDPJDxbQkdqMasvSUjCrcAxsKyynOp2oGGMtigU78bd5hbXrZzE%2FK8MsOuFTH3Q1077ZM3emzi7pjAv5QgQS5mdVGfkQzciX3c2tARI4P4aTYUlzyRt%2FTbN9KTzGES0BuHLWFFi7v1fkz2sV%2BsZ8MDvdErFHH3i4YCDs3TlNK%2FIkT0eM%2Fw2fdeJISkyR0N4EzN%2FlYKbd%2BxfIgJ2xJZyZPERwrLJssN%2BtfLEwu%2BqJgkjVcnJ8VGA%2Bq1C1vqiIZNNiS3PP8Glhl7ZhtENWSUj5Dd3WkX5UOyy1S2JAr8dRJSAVEPRmQ6fOUi2cIzuPpqUUpb0XAQzYIK%2FT%2Fu0%2FVRXWNS%2BnTVo9xS1UBr8JLkFdoO0KmPRVq35y6f%2FwwL4mcDjnhAbXX7tYy%2FMAe7qTw6lR%2F8eIiPfrz46iPql8DeV73zEGVkD%2FaqcbiYDMGqmuyqej%2FAe9Sv5Xe0Z3AdT558733rQrOUN3quk%2Ffi5B6WctC8NYGLdGACwv0H49doeVrtW02160JeHds4gMQVqR5jiVaNVx%2BZH8c1syslJQwTr%2Fe4Qduj0n5g1MtwbYr7FDE8HO5Pe5OxZGFB6CVt19Bt5aB7MGeIjny0CeNwywKcQEPjAfSGA0FqdXyow34XAGLVKMZ%2BPo%2BkwJkfO33WN9UMLONzcgGOqUB6GPyPT0CZkHr2sFeYpJQPApdY0IszymMeB2g8%2BPYxUK%2F0glBO2njmm6bMdZhpbppJ4boMoTEa3m6Jm0fO%2B0yBRFT%2F%2FCxx5TU2qnCHOqkJlYCg8Zeg%2BovCHUrRJIG%2Bus2KUsG26hOovFplnLHErw36FQD%2BgqaSfnEeDG8w8iYAQTJNVGEzMF1zWVW8KUOwMEycq0yG2pWtTCvo5e5v87riAfxeGIr&X-Amz-Signature=2f9cb97fc3c7450b08d7b801b103287691c00b28f98d1a5e706717872a353a93&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


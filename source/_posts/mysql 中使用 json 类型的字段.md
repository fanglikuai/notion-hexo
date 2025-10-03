---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNXAAFEP%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFNsrUSD8JpPY%2BClehlxQ6QD4i7KH3oA%2Bes%2F44ABwEyyAiA6SCSzr0FpbXpvFyOo%2BLY0MCp7viOaAoMyrqImBqb6lir%2FAwhLEAAaDDYzNzQyMzE4MzgwNSIMuwnqpzGAgq%2FOjTbzKtwDAmVghHAAD2Pf2Y6t2OIV2gdDO0jIqc5y6glgi0bwgY%2FY5SzSe3hGY7dGeCCltnfbLiDoVJLB%2F2WJ5mMyB4%2BBYNVVWesQTJ09sNNLr1kaxODW0j5GAJrzmljdHFqAl11eQ4CJlhmfCXKgLhKdaOurIAGm9spJgM7lV7RsKvgj4sQTCI1k6%2BWQeJwNx9e6Hmi5%2FY5nB8P6K1zUkKE%2BadncVEoMZg85zfBmTGXZhkorhlJN2Mj9UmjqBS2d6kZoC%2BpSfzxyDn8RgyIwhoTlDw8dyH54nF5hHKIMh0Zh5gABfZ99WxP3h2LwS8brQWkKUTwg%2Bc2YRQ0nVaN7T6B6d%2BZwEpEJIcQ2rTZepiYoyMy1bDqOzNDnYVq4i5955%2BWLk6tCBkJYhOQ9pStCBfxdtZ%2BHQ6r2lLrTnG%2BVlW0dmG%2BBybGtWYD4%2FkkFlDuYyTfz3jbjlAWHlU6C3DimMbBkYLkseSPI0Rt5v3p9etH4iVWPTRUxPWlsHgWBAIcTMhmLSBLsX9iEY7kJP5lzROXaleOQA9ZRyDfKeXY6lY37vtlyWy8%2Frnw%2FWB5Hyj%2BHthiSp%2Fi55Pufs4oSEF3X6UNXHAEKDoIl2OiqDiiMNn%2FiFmK0hoeOnbJbnu0sKuxf3tYw%2BpeAxwY6pgEDTMSHWpxLlT0UyCBmXUhoToFCyulMZ3rr3lH%2FOTw9pf8snoI6R092Pd%2BZHkapO2zHCsBI7HkcYAQPwlAgSx739EW%2FjT8bxQU3fEABLC%2BSSu%2BduaGR1TmveEcTBIwZcO%2BuiivzFhQsG%2F2oUpgbMQ%2B6YO4W2EV%2BF5Ugx0fuwsbK1g42nV1vHs1OMXkclwCfZzmJn8wkevJwIhjLFwyNgPA35ZA7iyxb&X-Amz-Signature=2069b7f08694eafe3ca34b9647131d68bed4b9c4a794fc6352741ee1679ca812&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


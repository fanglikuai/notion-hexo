---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DN4334C%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQCe8uOkl9w0eBbCW62QUilxsj%2FwoJwTGYzvTpPLb5OXRgIhAMfhbowZP3u3AX%2F9XS%2FdPQCfdgWFhrC3av4f%2BK0GGQdaKv8DCCIQABoMNjM3NDIzMTgzODA1Igy2T4yUZbyfzeSTIwkq3AOA%2FOGmqvuwJk0QIKo0iXvN%2F7eaeW9KrcjSL8XgJT5458zgpCM55gR8Lgwas%2B3C2eOoYocaAVy148011RZA6V43vUwYyphYBWmRp1plDUTe5zqvhUqlmrDeu0MxiroCF4MslgU05ipyUwmDJjuHIyhvwOQZEXJXJxS5Owc%2BfUxu9WmQ2v7qdENt1KO5p7Vt8xCI4i2fYcLPy0VJB%2Fp3qRx%2BS4ssaBJrUD4N49VhAPTpTj8NZVSWIzOUfpOs4O8pd7pvwgYUcyYvhOErZy6YSTRunBYRl24rO7AD%2FsmuZe5Myc6bi5eUrOg6mQwOKKSdKOHC4sHOKZw5bzBxyrPXvz1Ejd1cskh3OFm9DXCoZMWMKmKPqYdOIyhqltA8YnMPqzYTa6qtMJBNBbcv8yFc2LweqBSHchoYmUR6N6RFLHDlWhAtz%2BVI4uQI3IgReUeYJi%2Ba6qzqSqpHrVWemn9%2FUKPSf%2BdcTGt1RRbgMwnamgCkShn23h9Y2HiooRWBH1xnjMx%2FYO20S7PuzwFOmQoty%2FK5ApJk9wlJfMAuSJpGHiWcA4yucenW4dNKtdGH6ZrVgcLCjYK0oYvko7vaDmKQINftBLC9%2BUWQaaLXlhNzblnXQo7OR8MQ5BlA%2B%2BWXMzDp383IBjqkAa2hxSmzJWiAhMgCEsiKqxET1E9VBGgYZgmX2gI7vQWDjoa6REOul5SR6SSFxB9UidiXx8g%2FxfYT8ZPFNLUyjLvH2jTXvgqm6uxYvxxVdUB29N7JI3EUNWkQQNlRDTvMqsqACe9YmjFLh1F7j9S6261XYChLlE0KhAp270fZztwjw962bjzFqo4pICyvz6%2FDOX7zYWt3NLixATWaTCCkQWywvoFI&X-Amz-Signature=fda94e061667c55776778e81d59abc0889b44b7a860f94a7795f4326aeb5afe6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


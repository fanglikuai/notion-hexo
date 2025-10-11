---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNVB4ZH6%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQDJUVWohvOi0DjeNHoOZcUIK5PsjN%2FQXoWp8ZuOY8ZMOQIhAJSssSlF%2FCHBIvXhgjGCvPiWCyZRsLGhH5dU7A61s6pcKv8DCBYQABoMNjM3NDIzMTgzODA1IgxTK0dfcxariM1ckBIq3AMcgZd4hTFuklGUt1lw7EXXn5FZi8VPYzOwqhGrEe64M6QVypfTIwm3%2FVVgRGzSLKR8vrFIreSQ2UKfJdWBqpGvG%2BQvL7FQZWK48709RuRiU1OBQnTVbQdJNfPBJOvUQGQwZjt16ce4LX6bFKA4C7bnxfnkL%2FvmBdTqINheExuccNWREE22DPyoKO%2FcKdHmsCpnUpm8wh%2B%2BaoWjrZQyc7R3mh9b3T%2FAa6onHFlwzOIE2WEvUMH6W%2BPwODoaTCVl0ZP%2BePK7TYdwWkCtO0Ih9jkNdnzAMiagnjCOTlHsZ7K3wFIit8OtrvGDu%2BClUvotxtQ%2F7pHkYEv%2B7aZj2hblC3i7kpcJaxRD4JlQFL2%2BGXnySG4qGj3vMELMnBpGc3gd7fcy2o4veDFlR1PNNHjAAPwwWeuzecK%2FcdGUlg6XNvMqb7jdzu%2FF2KH0DDcwMZ79mf7jDvYVKgLrQROEgS0smOKJ0twH0p%2FY28ftgD1GSqHRY1l4%2FRmxS%2FSb2jvHAcwDJUqm1yn0oFlPlzxfPpE6rldqlI3b%2FAebnY9FNdwCbIupwqN07GTnc3BV%2FnJlf7T3j0%2F1D9Oc0K5vxqJ6m83xz0fUJ1At16N2ZToMoWokoSD9uvVVX3BuXBYdfZ7KhTCOpKnHBjqkAZWiOXvfE5OyMboTpzo9zuJqDivAHsJQFm3ugJtGrfsm%2BViCkB7QcSMdlZhcojSPZYZ5CTm%2FqJWwwRDtKTzInnK4QgpaL5BxIvNywi2jpK4TXixdYFGWVK5AkJ6SmAQh5cpAlFnc515Nt4RvnhzNlHGT1elL8wNVUXBAVcD8%2FF5%2FJhnZVEj%2FFPyM42wGrFuWJFOtYDrwLQTwa73QlZgzTMfTLTBw&X-Amz-Signature=8b8e0da1e46fc2e0749266de60d1bc9310978fcc4a135abdbe68c965da2dfbd6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


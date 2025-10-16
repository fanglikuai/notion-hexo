---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRHIBART%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGIOEvEByZT%2F2WbDADEh1pnrtzo%2BtuPzrUQJ5PkqHrP2AiBfkMOUiyAFGIvSxZ1XXtsiZsAJ0HFcXm8C9JAHj%2FfkhCqIBAiD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMv5y%2BGy1ru6T78bk9KtwD6Gbg%2FjykGXroOXyeINcy0mLliHYhdKx8q%2Bxt4EpS0xmYqq6Qdq5bAm9GmRGgfWySD9IJUjHAgyFcaFubmv6SnCL%2F6Zh8cgRJ7ld%2F%2Fxz%2BfX3WrdtXZ%2FAkKcISfiFLi9rMWohmI54tPk6tDTIzslOBxQSksvS3%2BA51QGa%2BWEwO8IGTAuAY3va7hIyk7cjjWWgkNnEaBQUU4Az5nxfY0Z3hNgRFQ9oqX82yD9TGFsWBvChQBdBgEuBRqW5lTycAHnN3p9YuLp1T0bVAJGbyiuhz6wBJ6tudNBh8brhLAeygy2iTsUTr8jDh0lmB3RVie%2Bnv6Oj0bv3g78mEbYzorn77B6u6uKZ8JxkqOTE2rpcLiStlmxaVOjBUpgWhPVrbqlzjXmM0FFSRwZ1E3NYHvknXFL%2FlCCHzuq3Z5WSYZWho0bIJoHkrKjxPt1T4JLM2OtUVxEPDvmw1AWQtVacf53SodNuw79eSGMm6Bnw5VswakB7q6k9xHejP004mwKtqUpThEgXHioM311rFJ9vZOillk8WpKPF9mWgsGCRL7xV0826kThIpNfuclVv%2FcSNqhXpZ3r4CqEX%2BoJNMOOHbqNZbOxKxRrcKua6YZW3jw7sYoTmeBpjBtRIndCbFVmswgp7BxwY6pgFAh79SSgED5GXoEOe06%2BO6OE5p64iSsk06PcqHNOdfeghMIoS264NPL9hAR8%2B8sJCHBkc35cU8cHZi3wYf6KZsep3mo1hEHAdQHgJ2Lt4F%2F3zLIg0jp8N8ZjqohHR4%2BPcTPraJnOw3sdONC%2F0%2B2EJ9qQZ6E4pLkougFuiI%2BqgGNtt%2B%2BuAHkdYpyk0orNcYJJi%2F1O%2FvSM4vLfWdYYlsO2L3AvKX6oud&X-Amz-Signature=7e55653ffd2a9034c655a599a6aee35d91ad91148477f764de535e1aee4bc147&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


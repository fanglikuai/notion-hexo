---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JK3W3D7%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T180048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJIMEYCIQDxZYrjGzkuN%2FANjhhH5lv2LwJt%2FdKUwlEqGvP1sV54NAIhALsV8NGKGK6P29uT1K%2BGE0hAYp0weIpA5wv5%2BZTDKtNLKv8DCDIQABoMNjM3NDIzMTgzODA1Igxzo%2FMQ3yLNHjwObwMq3APC9vrvA1L3vNaCHUAu1NxO8QIQTIIet6%2Bb4jHryrZXpMFEr90i785eFZgMEF%2FLjtSrrpdyFG6x0sGVrjmRl5ujd3sNiMcFFivM%2B%2BW4sIHmSKvrmVXY22Qn27JgniVkEvqSpl7DkKe8pVee%2FGlEDxc4LMggkVBeTy%2FevqAst9Y6c9A68nbd9Pc2JqMaPhUnCIr6aVr5r9BrHYNagyrlA7xcJDHJUXEPiJAdqZzIs%2BXocurX2H4emBCWv5NbJD1tLqtrlzhIFMF6tuqF0vvKbXaSPgpKtaUZFASJdFv6vz7djv0qtlsHceKtxPdKgdgmFu6fJKXFMzdhF2zt6BHjCeVxAjL%2FJpezrnXp%2FYI6w%2FPf6D07P%2FYAjP5oUpE4BhSShXEDUaPTMIm8aHOHdZ5vpi%2B6XUUIpjqLduqWCgx9VhO2c9vUYz%2BQ7VRH%2FprPqY%2FByKUHFoxuwKREHgy9yGyoKmmKRgTzFqyl4FURZHPslpvlgJGezW5%2BoZOjN%2FiPFQIX3Ba6RQECKesFG2k060S%2BoctVf1iXJFrJyTs9a0yv9S5iJHcQ5KTunzK7siV5EYhPZmE7xxlxzWgn4agj2Q7X%2FV6LTtzobAXZHBUxI%2F5GtvsIZFE49f%2BuzVP6IrDxmzDileTHBjqkAQSZ3bLcZ9P1HUnKwk1%2BfOh9GUi7wBoySiAQE5uicc2RuGcqu41DEnBMLTUx9AhiDtLzO5G%2FYVc7zCEwHd5jpscG7GFvmbVYEtpqEM%2FewfuJ9Uo3id9PD0f%2FyTw6QgOqV0QCEtL8QPQUsuRjSCkRcQ0JlBytpyM0sStvhUCq0ifW5Ha6nwyft1A2QuYLjTqG%2FS4FdSEegfCj4BuNtiBb5hWyWLdT&X-Amz-Signature=abccef19ee8b416cb71c6c0f7ead5464942ffdde8d55c7ba57d13012a6fc3830&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


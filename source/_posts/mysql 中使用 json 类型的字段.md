---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665764J6DM%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiapl4dVjIgfu7dvqJ27HfsFVOIQjVmdmyxiE2JiBclwIhALWek8%2BYsQtebeS1OXmOmboa8EqQsWFXtPRt0cLnlYOxKv8DCH8QABoMNjM3NDIzMTgzODA1Igyxy2bXKPMrt8Q5N6Yq3APP4tBgEgk2wHAO75GhsVBT9F8YG%2B82fmwXEAYTi%2BWiOMH18DWff9J7EmCJCBAD2gzNR0GzZsAAk%2BkftInuO8tAlleTQRZvmGXxh93QKs6kevJFGFDrKCALsvnkg%2BNN6wQk%2B%2FJE382BTBrYdAEm%2F0uRK3njFA%2F5Qlru4ig2qeZ9hk544TOuCgKMxfj%2FhE5KEbvgMVhHUgmCv7RDkRKm8nMtbpiCh9GHEN1A6nXxen6UmIkGhGtMbXL3hu%2BCZNbQU0GYjL%2F9j0DOQtHtNKry7PO15TH%2BRdeD2ISR6R%2F%2FkSiJvXXdfp5tB%2FaL6eCztp9XkglJS7GDJXKQeFL0LBMIrcgku7BQcWzET4%2BYTRgX0HLKQdvGgqROVOY6YszjwYXkKL7wmvc%2BKZCfa5pAkegslPz8zeYoxddiHEbmfXF5LIprhNXTvW5Ybf%2F6%2FePcPNgnHNpnGcauA54nqrsx17zUMm%2FihJ7yD9sOdKDWH3GO9Z9dAqQzSm8%2BfQFV0x9XLemhfJVo70IlmpmFp8U1qNk6WglVi%2FcFZL09JGT9daOlOJcOhu%2FsWCvQjjK37qYUDsZ9HtrX4yYiaJMGG3ckRN3ygE5nQJCWXBQgNQ818DRnyTZWNcUvGs5lyJrCeEESkTCetMDHBjqkAd2ohZf3YHI5AhwILAvC7vmE7PbYgTlhMWW9T%2BDUIkkw8GP6bKqeJ%2FJT9G7KFXYMgfrom9OhwSa1KQoIzQBkTJUKHE4NyObdlVepauxky6ey8eOlhfl7H96Ytt9zOBfvQAaa2quEeEZBWQPflkGcnzkLe5nGHdHyRWqDwOKE4cgCziS%2FcDl7ZWbGxoAUC9UjGJJgYJ9kgUEB6ARrq%2BaWwsDOcYcl&X-Amz-Signature=6f5dbc84f4cc07a6e6f40e4c9a88d070b02f64fe5f826e8c561810e1c3f08d56&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


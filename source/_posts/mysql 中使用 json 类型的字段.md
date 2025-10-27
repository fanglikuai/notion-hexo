---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666DEJBU65%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiXcStMOZh3n%2FxAlC7K1bH3lQLsklVBRH42lIkjRyJlgIhAIO1JJkNGCwlPo4QZFP7vsvge3BpH0MgNST8UDUZfLw8KogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwIYSVYeth1ejLwgCUq3AMAEHjlpXz6aYsTOR7m0Zx4IhJNW2VCK9e6fQ6HvzSOnt3cRQdOBF5iLlBlhf5PeBK68Q9Zilic%2F50tsqIyo1EqltJSJmnrRIy90Yj1JEscI1zgCNMX2dQorsOugdFdGzJudjckLTUl6cG8CqQfFBCfg8h6FV9O%2FON33Dc557LZroCahE9mo4T7LZivHBu8O8XAtRkiiXxocURVuxzFi5z2f5%2FDaUE%2F3ryQdDAhJ%2BqUfK4W%2BSzeTMGc60jM94cNJh14IkkqqDZAAYG4604VtJMAzfqCxl9dv2cf9l4N8F1MKCffm%2BiLAA8bU5OJyfJCekNEASr1lbZfXJyvr4hYEgS%2BiJHMj2Z9ZPH0bilxFhaXkwtGsyRSIb39fXNLj7zauMcoJIAqRRKnO4g7bpzL47OnIFCVVZWHMcWefvWh0ph0IyPJG3VEi5h3ASZ0njwrp4%2BjkR%2Bb9TLjYVuIZ0oZPpX8GYMXEYFtUN7MSdNadPQZYCk9%2BHYiNw%2Fy4Erw5ApK9NSpMwxALkr2SM9bzuOjECzoLegfAmBSoTip6ythGGHjB4HQHHYTHbhvWJF%2F0ptdhULNPjQ2%2BKHU3g9nCDORu4UFlKMdmncU59M%2FDuIj72YPCKSMM1p60J1o9kog9TC5kv3HBjqkAehZa8%2FPLFsWkrbQTfyIW30nyko%2Fl2T4Xh%2F9971qJgg9jVRxeroJtPW5JTH2cmHb%2Fih%2FRmmNUEb1jM8MPWB3PhZ3Gs%2BTxmiNfJtwH85hLg6k6lGN4J%2B9WGLszdM%2BaURnpui9FpsqAJymBAnf9%2FdSdixBBvpffhWCW8WWy6jeKR3Nm6KRmDdGkKCUZKkLD%2Fg2yNHykUtybOfaRpkiwoXzhJ25V2m2&X-Amz-Signature=bca329711939ff50433356aab1735c0e8e7b6221e0fcaa2244370839c81c19f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QW7T4ML3%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQDKQWj3bs8XF97fvoB6Ld%2FrucfbqMRto84reP9DvljEBAIhAIWF5D706ti5wC9lmdmNdVIPnEe7NKh%2FTBSml7HI976yKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxDRi0wkDt7KBNPdGMq3ANtkX4Os2q%2BSh9V%2FoNovJO9q1ZaroqT%2FHK%2FXkCLh087%2FhT%2FrO0bcjq3aYvC2upI0Ynp5e%2FPyiTgJdEaRVzOOWRbg%2BzHhkNxZfaeHnSxoV950dlUVaEQ04n9UgVDB5lBvRv79p7d6A6esPH4c0ukxoYISU7eKUjo6%2BdfZLAWBBuIkaxBWx6Ls89bGzWDJo8JZJEYej2%2FpEzwFMw9EQ%2FVtLFtMmFqas8PRaYfpmoc6745rtAYCsybUaz%2BB61X4MM6T146ErxfXZ7uiRMjtFucTrbRUwksezlFFSEgCcjPtFg0fhbburcb2FSuIscDt2eoY%2F7EdDlD367FUlspsBqXoZA7CZAmwqfXOsV9pgxTVZdGGLIZ%2Fc1V9HadvovM0jZfy7wlsf6cXvhiWwNhza5NT%2Bpwc2zahpQ8HBsD4%2Bjf60leTQ6MkPSLrmSnkwCB1KI5%2FW73VVSwvPeVXTRgdPOXQrqmB%2FuRni%2BMaFhW0qQaMg2boNhoh6wk8gZDX1xi7w9azCOHk4iuIgbdTWo4NnGCnMC6uthcXbVzByqSbXdM6rGLvjp3Nn6aMncbbBH7hRoNpxJZb40QLX2lWVYfYI%2FoyGNOGZaeaAAEFVbrxLiByT%2FMaOzKmvSOxmu1%2FzA5UjDzgcPIBjqkAaZuyjW%2BlXJAmvmqdHYpSOq6ENfjyZaGqBaozaYj6vBNqCktjyoYYIqOj4VLCzCv8RVlMcCjhx%2FpQoNQ96I5o9j67sw%2FUZfo3fySuxez6BglcrEdV%2Fa%2BtQFX9r0F00YSyjbjiuny%2BRSsu53fhNT%2FrSlrL%2BIPwtg9R5VbDP0aALfsc63yuCIx3H%2FsxG9hrLCqFaO1LQepbO4K2ckEp2XLbHepbhvm&X-Amz-Signature=3302215ca48a2289f16eae1fb7a1c56898670584004a84270b0a7cc0fbd7abb9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


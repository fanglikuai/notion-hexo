---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQX46V5D%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T150045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0K%2BLMbkLqnEdUUMIhHGtFfW4iRVnO07dYrFg7vWFGOwIhAM5bQ0iFmeJcLeHp%2FDbluR3IqazDc6qd6xx2z6okwwgHKogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyXPYwuhKmoeHTc0Ykq3AMGzo4KVUACrDyTQk9n5DmDEQqDlYAGiassKo6tmIPuiQsI7FCbgLeZePDlyaYz%2BllFnjFsxt3R4lGtPlGIT7LacUxoFCnuhEaipmRp8G%2BJbBoW4Fn0rbtrK0cDWT7zDAYqRWJrjWTfJJPe7UObmPe3UJpvG5dNlPQycA2%2BN1%2Flv%2F65DirxggJloOpCZTu3FV8wTKzFRXs%2FY2OvTfjDacxvdKkVTV5vTIRUBTjdqe2ISnuU7EyT1ixprHoSe6AO7JLQ4hLc3WOlYK7TrEaALeP9tdfYZAtBfV6ru%2B5KqQK4MhzG6Yd9COEJ0YHPoUui85tqiZvz9bfNBpaVFOPJRuBy6OpTrVsp2Gt%2FpayN%2BDiQK6sQx6z%2FmNWphVjUuVwUlQL3QBTTgp3UnVXTxAa8Q%2BJhD87x6hA1P3CX17xhxmtppJfooF3%2Ba1xj8ZjjtGeI38t3liVw0%2B%2FY2eFc1tGSA7XGqyhLufIawjYZqvdJvaN5a9e0zz8Mrin6HcLkvyZ9TsyGDtvTnRE2IWHTpGk3EgF74eStymxN6aDxdgvsthz3bIvozhu%2BFkLhtMRkPBT6wKQ8jLBVatBxfWajCGsf8pG0VwIXuqTiVVuYVLjJrGZ95V777hPzlm6xKwbw3jDi5LLIBjqkAb77p3%2BpqiQ6CLac7ZabAV5vVtplOUUw6XjOQmszdyqnkIHr5C3o8SbIea1EXtQK0M%2BjIH9NmWQxo4iJ3vMfNTmkUTgu24rsYgz5j37aSw575HFWQDXfoFhkP%2FL5maKXZvicEDCa1Kk9c2ZNUqB7Ka2tGVTW5xWHctN%2FsvBzTqrR7wEaDO6WHa2Z9O3Pgs3FrewHIHlGa67z4JwGDp2EJh8A1hBR&X-Amz-Signature=12907fbb2d23a8006bd3ff90be0d9bdc2468f0d92dad63e2458555a16f3f157e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ETG4EPW%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFAXg55j9PbKADkjwz4t4zuxscn2%2FALLJQ6TLK6RJgCwAiB4AwNO3Tlx9zSyo8%2Ba8uNjPN9me2I2cJqK4FU0k4t07CqIBAiV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMukNMZxEEK5lJsMFrKtwDjg%2FUAHcJuF%2FSA1RJHc4g0VsCfyCEyQUsgW1yvEyd1oKACAdF1kG6EsH%2FHBLxguc4fg2fje46oR6m8pxnz6vu1jo2p8WUs6pQW1MqkAJWo7OlZZ8J2vXu8Aefyj55cqLatFbD9%2BJR5Pr28htUfCrIQOnz6zHU2bO7x0sALYUocK24jDRro6Li6cja9sXQkASEmJsN%2BWINrNufovlqrn%2FWmrhU5K%2Fx7%2F0PMqfvvrOOeP1vddRlFzZJyxc7Sz%2FDMYdEIE%2Fh0r9EA%2FDZtN8HIDc1%2F5yT%2BbUTRZC6z3ipkf5ft0jTiZHaj5jUIPO8LSTK2va%2BJQgY2jFYu9SIw9jVamBDbBK73bJ1N6q0snIuCsDypXBCpKl5McVJLj4%2FqzO969F%2BXqh%2Fe%2BUqXu6JDaSry9819u4a8ds%2Fvn5DcfXA421saaaug8yjKoX7a1VxkJHRnCi6rwZb5%2FqdtQMYMQPVWvX%2FYgo2GRmBoI3Zn%2Fod6PESMGZhoY0IEAXs5y4oe7obOq4pTrHkPUVDsV%2FxkowAm56y4%2FPyhKWVVweWqXrhKQy6OHtygj82pnsIFOjTj%2FesGwesNjbw4VmOjlRhNgwah8irDYY%2BZmlu3AnwAFmzc4eT6R5McJO3UZK8HvkL4dAwi8muyAY6pgE%2BntSoPieDR6%2BSP8GDRTqdHcLLHsB8wozX%2Bv3LwCAhNVz%2BpbCfVVxFNKyLHWR7T1sULR%2F8hDsfidk27SmBxP5wWb8B%2BlyKO7oJz8yu0noaeMfPymLRo7TbjSSngXtL6a3bIHGTgIUadeI0J1uId3T1iy5HcLui%2F4pk1htgszXTYSywHZj%2BCLQtHOlu7JJXAj8%2FJghtv7NwJsqP6Yj8D%2FgwpKGuZLv2&X-Amz-Signature=7038dc484c2459dc98385ab69d16f955ae792263e6361981ef5d23b3bc2833f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


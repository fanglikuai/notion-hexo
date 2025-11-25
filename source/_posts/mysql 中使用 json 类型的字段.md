---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VCZCBOAC%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDKLWd%2FXpfOgmOT2hxKeAQg7R2IJ%2BcjMU1ZAzuQ4eRvGgIhAJlD9B6JXCcH2GkRl3vFgVvCVLOvpypNDguoKGaZJTX7Kv8DCGMQABoMNjM3NDIzMTgzODA1IgzEFLQs9dVO2tPtUU4q3APE%2F4hhz4szsxmsH5KIdZAktmL79mvq1NUbOpBt9ZOFQHhRAAeOmKrwzsD9WG9RwFu2DCmbn6CZZ5nUuaCf8v4aKQL90JDrtG9%2Fq0Kz%2FSTsrNkH4Yb1f7JebDIgy2Dqknku77hRdFvP3%2FMnF%2BlmpC0HTME0kyF4kWPzTmw%2BHQDVrbwMI6NZJFN9%2FetaLMU5wr8W2x%2BbtEYyUse8tIfEJ7W%2F89TjT8jkrv7KrOW88PYwrWwufec1MKK5rLkvCNNe7KJAEdSIJ1bAlFyMqtWowvbSGy8MFAlZvRTJhkffvNLkVbczozEwcVlMRSHKV4M6VzLicoejehZS7Kx7%2F24hCEhwMage4esM%2BeOG6xh18tSW9wTsFUgtbAZs2wP%2BSx3vZ0IZfXJZRp9Gt5z5N6qrl6XbQseOgkhm2rLk9O2o0G04s1u3rdWUpJtzPilE6aRYvE6qd6BcMAtNW%2F3PGtF5HXm1tSmymX3q1u6m4%2F6AnkClsGHljH3OVg%2Fz6rsimavcXNEZyY9uEFqaEpSTjSeLuJy%2B3AoG7OvxrOP3jT56xDu3601y3%2BsoYCxj6MqwkT0uyH3y7Zu62vT1ejNmmnIVYpWrwynApQcgNnakke89YdbdI2AIiVMIF7jktQVtOzDwmJTJBjqkAaPVhz%2FIH9j5neDtFBNG%2BS3WrJ%2BFdhYBKTxcLHae4jA93T75sVd7ht6ZAyMAHS5XC%2FNuXe%2Fm58YU0RM3FDP9Fc8eb1L0RNW7XdJnwQ9JcaiGrbU8w4QtLG7in1MofoXKgEkITX5EH91M%2FyUSicVLMUXLt3EvyX7WyegCdDazS%2BOkgSJRjtt4lRbQnfig4Is75R2VQ7NqWe4HRPQNJxx%2FGrJLM7ms&X-Amz-Signature=e3ada078c2dcf73b323086dd05821f63045dd15715ef007202dbf2872e927eeb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


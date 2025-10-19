---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2Y24KWV%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJIMEYCIQDp%2Bprxy212ttST0uSE3%2BxHKCP4BGVEd3NblYFCaT%2BNJwIhAMREuwygimf8%2B2hMldmqvQiXO3F8LGU3jZFfRsKcpbChKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzASGo9whaROlly9GIq3AOpu9ZFLs9TNJTHIbhRyRO%2FFY7kCCUZTpn5qqr8aDMSVLrLkEKcb8uprfz5ufefgy8K63ulqfXUI1YmuP1NPFqqKSQ10zEXN3mHfQTFuzwmKPE3L7IAsLMuc%2F1cAS30eFrGcHUnCfaPhE3ydmp0K0mpYBXFRtWxBbFJX7DMANWGoGO4Q1ABvMXU%2BJyb%2FiRSZKkZWGlJdehJS%2F5B0dDSQ1gdy8YvU52FxT5IvVfMYhIQyyIrsIzQUXuoGl3xygQukaxpypgTjnJomxg5UgYdbrBz453iaJXrfsUrJJonfe3t4jpjJj%2F%2FAqY5jiL35PwIOx3vSsphjSqn5K1iqRu0Ab4fho46fXhFXyamI%2BmHjHdfAHo6HyzbEd906Y%2BWv%2BQAC%2FRXobYA297JdTX%2Bxxy4zSH8KccEIiiA6F%2FBx%2BLRbO727tkHBVEFbYxPHqSh54NhOjAhY5yBBCJ6WYHZ%2B1V4wcQvpH%2BX%2BJ8XhQEvAKqJkc20GEhfRs%2FQYYTffyLzyWZRwOJK3ikg06zrT3pW%2B3Tf2ovtLrjA%2BTINvgnBIkvvqneSpFAmhN1dCUpB5Anqp83UsuLPOZQi2B0ZpS7835vPFZiDManWnxrvEX07tSEmZOeT7a3VgoKdRiq8GXf7UjDk1tTHBjqkAWxglhCfUgra36m4fPoVWTr0gB0V7pF8f%2FIALv2ZbgC0lXGez6lNcbrYsaAU5tUn2TiiKy4CAHteg7vZeEGNrd87ayAONb%2Bp6ko%2BNWzWHvF6CB1e%2FRp7Dg3AghReiAVfVOCJa5VqCoq8DswwwLAJSg%2B82N%2BUFu%2FQ8%2FxgfY%2FpXsa4nDJVvKWKICsqR4y4Q%2Fnv3tcUsHw3Jg2OZMEsHBwmAYVtFe3G&X-Amz-Signature=8d4b78d5476c4ab9375d43654867435b13edc12c0f91c324244e07d984804ca5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


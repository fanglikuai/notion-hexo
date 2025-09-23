---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NQX5CTU%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD4QBcFQNj6bMqcG8paREi4kWmt%2FgjSgNa6EQjNx8cK2AIhAKHkZJ6uQfo0rqHPx5dNAfjsuhDb%2BCc2EQfHKI8uVuKvKv8DCD8QABoMNjM3NDIzMTgzODA1IgzQhFyF8D7Pz9Bnl8Eq3ANwS7heo6%2FHtNpVnYtUjZLrthCJ60TVoQ3ZjQBRCnm8O%2FDs3PJxEhx4vTNiqyDlR3Pv9T40EP5gWg%2F9uU%2BU8Q5JZ0%2BNIm2RCbNQ4Plkg0OnWuxg4cbe5gDXBSEHKgjinBmXOjB%2FqWe0%2BBSOCw4i%2Bqem5Fu%2BWcDpUevO2f4NEYGzWL%2BVYfDtAc%2FOIY2DaNhGg4Upb5XXtEqlDfmYf35i57kmHdMuNAfU%2B5PtYZKdeP%2BMuilzVh6RzSNbigsiM69jYvooxldN4Z03xfcJZiuFXiRx8d9sA7NVQGaoR8XtLG2omaHd0yiOJZ9oVfqNMBsNUlEIipRpj3025Oi27eHzva6i01rYhLIxncpdgTJivYIcBT%2F8PFKKNjmPuVdPIePxPuoAM8ztzyDbXMD5F5Mp%2BRLqDlgPJFEKjtnsk3aVO8e7RthGUUiXbGNd1pLdvTl4QyiV9DinF7%2FmPIMndmkfdtonfPLPsPXGC0p%2FxqaG7lag4lWkqobm2ssvyrndN3pSyhW15ACKo87NpUumUsC%2BBb%2FaYHoGae2GPet2oIAg6Opo88k5YiqLjCoysPXAE2wZI1rbUawhLoSPQ9tH8b4PCBNGRQS%2FkzgdrivDbcEbkqpNMeL%2F0Q6bSilDJZGvDzCu7MjGBjqkAb8hpCoCOnzsucnKv1yt9YjttmgEsgiDFeFGatSp2Qq7G0uQXa9TNZdrUZ99%2BXz8mJEjKTnFJ4k1YysEStI41JuFxBkdbsmz17x7JCroBq4nNRu04nHzP6bHOTr34ISUv5bn1J%2FJDZgE4jzsBL6I2j6%2BkEl3GKV9WaYKChdY6t7yE1fxwBqgSLWEcm7x3bruY1yIjIjzIEnf%2B5o0MMHWbszG3vxv&X-Amz-Signature=f0a55cee3cf17eec75033dab35bf8ef995f2d2ff7754cae8c2618ad4d7919500&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


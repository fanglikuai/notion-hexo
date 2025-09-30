---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDERM4RD%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCD8sSP9AmoeY2funDgBBQt2Rx1q73A3g%2FAdMnen5aLMQIgA%2BL3RuqmDQPBponygLVTdqLWr5WxXHurbFiCrHS7eWYqiAQI5P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJpjBZWK0SBDc3eCmyrcA90gTOfclXoznIiLTBmKxRA0QIzwEYmZtTNItQTPInRYJdeXcir4g0mKmNoM9Z%2ByrFLd7B7zVji1xDXuLXdpgWhguU52DBzCCToZJyUnJbQeIbSb%2F5rsfdGNWucCgpKJkCJIs8W3HK9t8bsUyqSQmjDWbq9eR%2F6r4oaYdOrL4KQMvtb%2B9AXTOihZegkdSS8wIb%2FSb6TkqpLo6ELG4asasbpzbybta%2FukKE%2FvuEQmYrUc7wKBUhGFJVJQQ80g%2BNhSfliVJdsdjt1zMvF8T%2FIycWPR3YXDTSLaYbc91%2BoMu2Gh0NnpCT3RyKW5w8UWJuamy5myl2c5zTv2KAviuH%2BGyhMlZWskdlh65usz40dA7mT1WQpHQcL4vzFycVQCap6le0hCMCpEnyOocW3TkbygYTA%2BtSHsAdp6%2FzS7M9Vt88A0bxjZ6syUCXFRpQR3FXVRzta7CmF5LnqLjWWzPIUOI4liKX2rkhrmci3xjlvpEfMcm16yptZWSW6vdXlEaoIZexqo2oHEjyFOII0y3kzQeeMdjlNuRVXl4dy4aKRv037LUeBquE8X2H4BNpPNh1VBIDadnzmhmj1rhWKSfLxBFHQ1PZzhz%2B5YmgY49MXOirkIor0FF%2FhbHla6nk1eMOKJ7cYGOqUBchdPy8HbAqVaFfarYQpCaOt5L8PrVo4yQz%2BDTc8ofsHFxg5j%2BLN3j6TrDb4uEagS87zYukKAq2VpM%2FN456J8ZpHnkTdBf2nHIWfYMhVOJDhL5BH%2FmJCSc3lDrGXTMe6tk8XAtMIMDN09ARA9MSWv%2F7xzvKMh2m4tyxr2Y8%2BcX7D%2FOvijwfnQP6%2FNdeTWrNYFkvgZr1VXqmzGY%2B%2FRTBCgrb%2BZrM5u&X-Amz-Signature=83277e57fc83acf44bd6d7ef3090f6478a78d0abc31ab8fd1e79e59ac8638b9d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


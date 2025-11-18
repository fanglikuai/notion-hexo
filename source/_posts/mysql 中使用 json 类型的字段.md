---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YU267VTC%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T050047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClGVhzZyjT2%2BuVQn3f0AhJtMotuYza6yRw37yR%2FjtdrQIhAJQ5ritpEESVVxwTWyN911OPREbcO2uw%2FhEsEJHd6TOGKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgylonThkbN8E9FSkiIq3ANOHGDncIIunOdw4vlzgU8UJTqP6lqSl2sNbKXpBMuq4009TYzor%2BH6zMrHnyUR%2B11epbU3gOt5OmKCGe4SRuF6JxlIkBbiR48HoQAnfTNql2RYcXAo80Uf0r8VqEtFnrxCxpOgsIRtjprZVRgzVQknA5aqsDTzQG8bdJsGwJq47Q6qOPFmx8T%2BRu4GUE4olWm2nKGvHLx3C35QLclgI1qbTSNKDUdh33kDQM6cVYO6gkXUTRoLtYzcIhLUebFTTB%2BKC7igxBvXlL%2BiOP65s0LWX4k65Je%2FGWj1gbiJF76RJcACTsd%2BKlY19Ayswaq1lsnP10kFrbllryyQfGYm95McB7lgXyzRZLV3ZYiUnnRg8qJtn7hZGfmYPh9ewxcpjwwrFGpIo3ndzCNmRG8F5CUJ2broGqQCe93xzamHNWzQQdWyhqbfewQVTxhBj40fAJPpYu%2FPpdlguUbtWexJpDlnI6bpLzJfVWxxTj0YUeKZIrG3Pllm2DSTIPBWPhXLFW8Ko6cS0sMgoA0JubaZKdtyOBPHBKC9yaACT1Zm4YW2YGQJD7ClNmfOwabZXP6RPx%2BUMAri9I62udkmfLiF43z3xQ2vGz9o0SJNCfES8Xz3ByAvgFnE%2Frl1G9P6UTCA3e%2FIBjqkAROSo5hN8OM3q8kM9v6iOcX4bk9KS3j2mfT%2FtNwA%2FXv5zDMdhM6r4OGJ9fAT34rQbxW7hfOSbI0HNJCw7y78AlvF0Z1AsdRItdjZTI2L86NyweXNLm83AF38MlZW4jNMACSW8BeIVQYk5rDvF%2F6ae9PxCVY6bKe1nbkbkW63o6vsrT6xcE%2FAEosiZzpAigthEPz6Rqj9PH%2F7bFKIbVDHYlaHJqVo&X-Amz-Signature=2b196697c5a206fbdb37add1bb2bc928db2ffbea2b5c614d828965e040a10bb0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


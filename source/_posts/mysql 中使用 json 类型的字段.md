---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TEZQPJAG%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T150055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJIMEYCIQCs3kcZFPgjUKbdxyxQ6mNDyLgMJ%2BUUfR2rDzHAv3Fu%2BQIhAKWDOh%2FbabqXGFMUZSIpmPrAxCvH0TeJm53YaADkF4cMKv8DCBgQABoMNjM3NDIzMTgzODA1Igw%2BYsOVv%2FA861XBptIq3ANZA3USfFcbWH4dBm60GDT1BlGQqSWE8O22Nwmp7Sdwx5nePHJLcv5Za7DWmA4EC1qTfaTcM79h9nrqvHWc3jGb%2F01Sj00tzdfgJ4wuG9J7AZH7aaudJwpwRaaVkFnN8iHBsfxsCteqb%2F%2BmVWBTodMvjyo%2FVwssv9%2BKoC1pS6is%2B9qgsjumXH2kQFB3fEDkH9H8qqV3NeoXfHCwCiZti%2BlBPcEgDNEQE8P0yZUDfLMZSFqVNdUp%2FyKudgVgRaWaItkpXJ9FFUWxU1aUcrR%2BmOpdjuMpF6W9N0%2BOf%2FpIExq7l9b2nwltQYCvZhf7sIPEFd29LFMm%2FXfuHWkcp1cDE5ra0hvSi9FkFJ00l0jRl5HpJT20BFK9tEnhwwdysO%2FJ8xfYJlwfaWWYRuGyUPlchFiIxH7w8uV200ufno4e5lsH2BRong8A4h0LYVG4mzhRRyf4EWxzjIq3nyevhQ5RK8ET6jwXj1HsXcWw8i1H2iPVjuSRWyR1rhIzFLZhaj0gHjVXpUAM19t7e%2Fw19gn6Qma6BcWAsYRMO5yBRTLRlZt3h9DbNkFHMu3BaxbSR2foopfa0xJdAiR3SGLIu8VfpiFsJ%2FOaKQ3JX9RFkENsh51KvQ55a3yJd7vTiXgOYzCsmJPIBjqkATiX5fxtTiS8q3fs4meFRaW64nE0Fo9xabKY%2FK3ZbhFxX%2FLDb9xrwJx1yy3AMMtxGMfhn6bJQQ%2Fmos8YQbrYuNoFDGB%2BS998NxCeeQZZLy2UfINYtqWrD5sc7vH16odPgkWYovtTJAvQeQtlmI%2FjxUsoPDLhO3RpTlp5ksC%2Fxpbeh7mSrtRVZteZVVDyUJRX6naZJTchwUm0GWcuYSv%2BhXF2tSji&X-Amz-Signature=bca9040f2eb3f2863f035827bd62e3480a827cfca0f8e79e6594a8ac494b45ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


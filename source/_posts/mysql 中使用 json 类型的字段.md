---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SO764X5E%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T220036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJIMEYCIQDd846ZVj%2B8ZjwEp0MIymXJ5Adn2PLmaFNX6eA7AcuB7AIhAJ6T0zmQtJYSpCb8apgn5Eztcd72Ca2tGiyN40GoqgKHKv8DCC4QABoMNjM3NDIzMTgzODA1Igz42e2YeLPHWTWuugIq3ANASOxOgnteyO4IpgI0ImAmlcv2Sd92LK3bnZm%2FfDRzpbVwzEdqyG9z19ERoYJoyVbRkqsRqQITV%2F445ojtdm4KuCgTJQMJ57%2FKGM0eZLbGBUm33DotZrir820Ca3AuFXcFOSwN0YQ9Ygdhh1JAUonJcrK0xC6DoALn1ZV9UKyShbq2ClNjCpQchQkAp8KTKoXWC3e3nTAOhXqlU60xckS6raFPgIFX4XWlYYvS3GD%2BTWPLEQeKPoum0nzCexBJSUeUa0%2BBtMHaHpNHMcCNsdm%2FmljVZqBENFE1euZWReK0VkNwoNKE%2BiMH4TTyj3z6AL74bfcRH3ZyVwovsl6Y%2BukX%2BhuXSXPzxOPLWvnTI2w0cFvKtbf3c0818roMTnhLWQ5LLwyI4O3%2FMl%2FPfWeD4P8TyUS4E4W%2FCwMNSNN1HDD9NDx9yBh9zcDiwFd0ncmIzVQoGihl9vxYYZs35HisM6ozpRqiuSw0uWd9wQCRaWCJGhExo3yyX9r%2FK%2BV%2F1OK5B3MDvPmcZassgsACyopLu1DioxWKc5lsMcoVMQQGXqOaNWIBYaeyKCtlAEhVRXYg2S3mng94F4VNjsUaQ%2Fim6cMWz%2BSpvwilJLOmRt6DKgLkX31YHBLdUZOGx5qBhjCUxojJBjqkAcG8coDXYO5Zr1E2XKk2bKnyjbNYusN1MdQEulHDPlPkwqJNN40hdaqXqag6PT%2FI6kyLlTkK0PgHBrpiW%2BPHxve1UASqchsbdNN5q2QeSRyA1vcwj2yOo00nBJbmlh%2FMo6PDtFdozblYDHoeHn%2B2c%2BQ%2BT9sYb9ph73UUhy%2FZipm4MbEL99mCYa5LBcTX2nf2MZdnfXmWWTkLwJzCKfLEtPYTNie7&X-Amz-Signature=ac0b63ac6a15fbc9661b8893310888f0f996ea6265f8689af0a3ca47b70c945f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


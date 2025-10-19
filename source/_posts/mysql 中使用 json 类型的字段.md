---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667OHY6DEH%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQCYMD2OAoTzKnewuGnwzseyZxppqEr4K3EMrqaqnBI4jwIhAKLm0PEjhKPJIaG2GuJT2Uz%2B9CoiqLkAidj%2BK3BgTmd%2BKogECMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx9hoZHMo6%2BQqCQcDUq3AOXs9Gv1QcZhLXnSoX%2FKEBfjOAcXUJzupn3K1CRCxMnzz9Y8SecQEoMTQnpJl2Jawc0nolNVZ2SeZdrdYzMYvjWqsSAVmPI5MjD23TJ4Al%2B4RZBlgee1lSqVqAKbQ15uQDV2TzgZwsWmcNjgplMJBvECkgxpqPAWkltlxqQ3bgfyR0af%2FUHr7Uijd2MaqP2A4P7OyPi2WIe1txCYDk1jyGcKhTt0yMFNyiC0CYxz3jaL9yKFHCUaNZu2eUOewMauzebhnByB5mLeTRG2XyNHsVqRpnUYDQwwWFRbsfCTiMzMWR5F1kyIG75xsqZ%2BrjCLyetrGjNLg1qDVHVkQxvHzG8XYskEkglLIBhf2EShXQJa%2FXrEHhmNljTTblub4UIgqd6yQPJrUGKdFC240VGxlSsgloXZvh1A%2FFb3%2F0HfSApgLdZsNRJt8l5NWB9nS9qrZlBuaHrwfsNyxAQeTP6vkZrXPWjHqdcRHnomOS%2BMA2cNgvi6p4OVtcuWJiJ0B05dYKehT%2Bd7xkUTHY0iCCUJPFi7h5sqXDRSGWJbB0vc%2F5hwZM8CtY7%2FM67dhe1w%2Bl%2BrZfLFTw979MpEX8J%2Bb3gNKb9qLdUJNkcRYX7PxtToYZVJNZ%2B%2Bmrlg1wNxnBjPDC0yM%2FHBjqkAamCiVqK%2BcQUtYLRAI7j9E0nlrhjvXTbV8hZxmBZNYTtFaSa0Aj2CRqsXLcmCbltOHQ3M%2B99GenHiehz8851mbwWUyW4cKTQugBfDi4ZaJYcwt%2FRtiNNB0lkt6DJrxdWBmsH1CQPClpCBkskevbgVs%2BsuEzaCgU6r1GGHHiOKVq9xUOOh0a4NoWZ%2BKmYyPyi3eczZ0tO7nNzW69nvSx0haGB6gi7&X-Amz-Signature=594eab72499f660b2a31353ef4f953e668747e40d73d96211cc591e3aafb781b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


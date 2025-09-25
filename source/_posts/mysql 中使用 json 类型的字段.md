---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQQMJD72%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T090040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEkH%2FAddtrFmPqZnh%2FPTosW9t7b0yvQpEoYYSJRy5A6zAiAF7nkOvakAfTTonar4XBUox%2BQPvn23v8v8MoiV150xRir%2FAwhxEAAaDDYzNzQyMzE4MzgwNSIMDIg824oPSe%2FkjDqdKtwDYuwNHiTwVUBqegDzRStaeWyxjVkfh4HggBC7OboqLROsyCHKLKcK%2FXT56oHh3sJcc4oFkVmWgNiF7YyL9KFYDVTPowl0tHvzVvuKTHXR2nrV%2Fs3pZ1K%2FVpjiCxejJEEpcfxexuIeN2psD9Rh5OOVx%2FyaUWYMd2OnKHi65dWNYkqCxWGbgwcaxqxFyi%2BTCVujpghcPGBiKuDiCBaNf4IZNSdReHxsQEqviwITubHiNTPpF41%2BsXanrIiMC6jtjpWzWOUY4K1zd8FKwN9RTlsZU116PpMThiv0AhRbK4u%2F2C9LOxu4XamFtClo1odVq0ripvkpXqtap%2BjgyvQ7mbi%2BL5xLxg1ZrBAHYUVPzOpMSg8ebCWimfwYXsoiItcAbrKad48bvRWRtosJ7TsPYtgKgNAZmE7V57JlDsOhdgx2hgfiTlTyyECHKWlnxn5r5bPLdbbKMqBbjO5dpeo7RiAiqYrul75vbwR0JiBF4ySP4aIZMdDnA35oYlcUgwgugxZjEk3fd7zTBcw9AePL75Cmdmu8AwXnRkbssd1niUDpDHPrzAaamB6%2FwUHy%2FKV%2FPOGoTQOVbpovxDhYymQijyowbmqbQ9%2FJHf6o8CA%2F81G%2B9moJr3ac4toVCmwo5ekwtfLTxgY6pgEBuy%2Fo%2BK53tj5hNeqrUhOMkXMXHfdJKhlKs%2FA2GyygAVM5FUPfsRqVkmJpDZ6maeTszRz9eQvIE%2BGJCcoOLZz7R3S3VMX1p8KZ9lmbKoa8Il8m3ykWaikjtSVbp6PFoOQYL8Hd8QKb%2FtZVjmvoruqBeneHiE%2FcH7vs8Wj9aS%2Fn%2F2fEQlbs%2BwobTM1y4XQZquo0XELlwGzy8y3IJSeoTpxvq6JC4Hj3&X-Amz-Signature=402fa135303550dcb0b6f93b2cac3e62335c15f4d0b7672db0934269a32ec3be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


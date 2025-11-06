---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZCR7GQMH%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T050051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIG%2BdbWEAF8HQoZo3%2B2dOJFPj2L5MfY%2Fzsi7pxKv8EvMMAiEArdYZFhUN0ZRjQpKUuXR6%2BsJ3Oh1HtfBEwsddVj7SzxoqiAQInf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOJiQbd28TClTd83bCrcA57bVBOp2%2FvUAmzpBc4U4Gqr8rjItTayqYLKwDcDViZMZbbOdnBjc4lllDXrZE0Bm8Kh2cOzZlP3vNJbc0pE5%2BMGjWSzwMgomuJ8LYbxCrodDrYK6nPzzbzltGyMEXsi%2BAyIsDcq9Z2dz9ksYxFogU1e%2Fa6Dr7JmFLqgCNCFOG7KMr%2BpmV2gaO9WOLI%2Bkr1guNGN6O6FDvjjFbLpSCWWhYiaHlZTDPx9Qn4mYQK2E1WRnRmXeckQajFx6XwYlgNT%2BdHyiEEXjvYybj7JCyRxC47vn%2FCUdSJP0lTSNNfQXCRxLd0R6wOYb0qeJ31jILXZSYYDFi8y3hn5SFhmurBS5ioygCUMO46BEvHGv3tvkpkSkjOipGcqlNu6lqdTfuI%2Ffp%2FJ52zmk64hxAFRkxlWNmMVzyHPdWkEVf4jeKtGU%2FBCB4Uw%2FiEakl49t6AgDVaTm%2FBKhQ%2FTAVW3VoTjd9p5XdoT1GtxGVRH0QT7uAGQRhRbhH8x9URcdqiQTTSgWdOqZnEbkXO2NaCT7qflUSreKL%2FGaZq0KY6TdkbB1c%2FuYo6eu5qM4NYzWP9xiguugdBTtf2dSw1hej5r0coAO8hBukDKMpfmUoL8zlSL99AMTDBpak3LyYVq8c%2B3B7pQMNC5sMgGOqUBhNSgJFLeZWlxg65m%2BgTjUwp6QL4Jp7F%2FgDBPXIlIrw7Hy7scWFax%2B6dFBkDO8IliMK7tWYlHGq95wFCK%2BNKZbekFfv6sBZpn5orePJ5AYVvEzZul93tIomEcR2Brz3nF7C5uzm3TLsESqoUICkWSRDLMM8bU40Fn5S7pVmleDXma0vLh8pUXm2DyeFgrx%2FtswzSkVwraWKuBJT5oU4qdXaSsR3sI&X-Amz-Signature=966873ba45190cddf708935c0f34efc03109876ede54b07c9d38726ed6369ebd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


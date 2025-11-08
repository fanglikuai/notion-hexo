---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAZSAI7W%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T220047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJIMEYCIQCzpXhKLqDIVtgOCiB43tZ5X6RgNVUFURth%2Bz3ZGQQ5ZwIhAO%2FsAxzyac6Qov2IdVj0HEVAwzmOHyiGFxKd%2B0XWA%2FpbKogECN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzRXZWGbMSF4yOg2Vwq3AMNpfOagOSQ6dmWfn1oEuhDNvZm9w8mjovdBz96fY55gd7GH64oXyMJuLsznjq0X6W5AXlPRQSUrSPbja3zU8pnP%2F0T%2B7ewZSZMU1mE%2BGqoioc1Yryo1cz3dyl9rPZCDpirkrZg9ZcCdRMx2zLU4OLOORY8NBvDNpo887SQ5KLkpWKMf27U%2FDWlZvJkCFlD8GIpg6UTERLzjx04wcCtj7kbXfwIs3am0Z48M5nyJ3ZkY9uWYzNKSGRlH3McH9k7ROUyckHudThyVKLgZiyu8vr1Z0RqtLkPLVyroglAdL9k%2FPWOFVV1%2BjEAw0xDTFb3fRl7QAwUgIvEl%2Brth4YqyfCSCgZfgyxwWmF4S9Itdj9cYb4GbC8f31W6CRnTQuRl%2B3M6iv550D%2FTF%2FTGY8peH63cxvEPQA5FztOcO1Kyk8uZCXMb%2B7cdjkbAEyPQeuSvy0N7g5Dz9xkDm%2F0NZL6H5K0200mjqpfEgEh06A%2Bf6YwkxbjMN62AWPsaEulRBxEBAsnd807g9F%2FsKFl7lZ0ccdYJ0YJzr8Zm%2FtT1mUHRd%2BNR1v2qlGxoR5IeRYtGarVSQagzAFUw8%2FyDa7ZWOq6rkZELQIYUv2A%2B%2FVk2zzEgXuy9v8PbPLbyLEx8WJjOSTDq477IBjqkATqq2eteBGMSGenmQUj%2Fh8yblU3N5CpVlePSIkQ0pTw1OHLcW%2FpHfsTlVv1ldFie9zCSPgmovCMCYYI7O8ES%2B8ncnONlgR02fJnA20l00A%2BF7TzwmdYlyeCBFcXT0SOSLqkH7X%2FBuUaKIJqjVVLGTt9Jm8RAD39L%2BKRHO80HB9fVcpCShv%2BLWfwDFltsQaUlAKd4qsZHBjLxQk2r%2Fgynx7c8I2qF&X-Amz-Signature=020e8aaab98a8e60db8efbf42ab69fc5cef73d49c7bfb6c1dd2ec5be0f37c01f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


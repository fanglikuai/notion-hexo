---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QC3RW6X%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T090041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJGMEQCIB5f90S9S%2BvDq3WeOj1ZDIQTD02SbWrS7tyfqBXdUj2FAiBENPp8kKc6EVJioAQPanORQQOeV1njrs072uItze4EkiqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZwDpRU1n3UvtE%2BWxKtwDKBWnFIokcZZ63P9jMBP5Nj5ufW0%2FFTD5ckTwT%2BrDWqo4K0L%2FIflVIMxQNm42YsDIK1IKVbX3275%2FhnRxGmF9o3pK2mxEbDUNtmD%2Bz1FY8zdjpxFQ3gPJMD6oiLbm8MhM7ZduZxnctkGJs5qclNHfQVhtiBkPguzy6e%2FXmvgY8fANcnkKDR080QlswHCGNZ6EMxTxsF873gXvUzS4PqQNyzoxDV8gUFDmbKqScczWwCMDhjFShFwWJUJ4gnsUTVpY0xTq6nRfbq4x7UFujJnb9N20Bkf4F7U0Q9IjQkQk0iKEtfMOwhMgziRh0CP%2F2%2FBBa%2BPV7SCGMEQHa0rYaolW67ArvvrBCq5Loa0YdkpVdKtV2oYKV12C8Cm57NpTYfE%2FAcO9cOYNu26zG91%2Fnit7qeWDQClKxxOEvBN5%2FHuhP9fLWsgKZoHQ1j3IY3Hs7P13IDLH%2FLkfMyOGQhni4fWbLa%2B5qTUvKEngBLmxQPnMpcp%2FJk3u5%2B7gBA40od49Pql7M4zmlo5Jxk22c198DZCdUdMM5YSWQZxY7aZ3pOo33v%2B19RFuq0u8b15ehHmncU3hE5T6AjYkoBDi1eKAZQEpQWwhZCAuJCMXvSjHYFHliwdLSLNjOaxQsV9Tyi0w9ZHByAY6pgHTMzqmuYjOLZpsIRa9we5M9%2FSBemMMotD69qnbU0zwpuWcRqbU4v0nmR1cN%2BJIl4bH5kPZDF8C66c9liVjDHpZnTfefKAx8P3aP8LHvGQ%2Fzn8qsUplUwqJj5%2F9YyVmEfAd68e1mbP740K2fl6QW6AMy2rnLfR4pi5VPrDetdOYtwNM4OrQh5%2BNR%2BCFeGQmLA9mAzzIWSrTQ4o1CO0udt8SNwg4RnVS&X-Amz-Signature=6b1eaa0404d65eb2ecde536f0ff3d8e03b64b6be2fafc9612b5f9df584afa957&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


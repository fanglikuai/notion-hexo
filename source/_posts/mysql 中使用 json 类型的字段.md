---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HQCTRFY%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T130046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJGMEQCIBmTeE26sWxwMHlygtQ7Cpme1tTHnZfhlT6gm6vO3G4cAiAzHWZooD2%2BRFDsHDxM6KArqIPtXFSua9SBWLfJHGITOyr%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIM%2B1JYgXo7Sdir8z7rKtwDXmzFIbR61hciv0X5of9FDC2QynqvSw3oHBblFvfcvGk4AfZP177aeJcbLPikxMb54xxBH97ToOQbeQP3w%2Bpv00ra2SKBnICt1qoGWCzuhaUMe8tlcmEpX8LtdJYXHt0pni6tOwhZ6ZHkPW0jRkCLNTJm16z6gyYbwJB7H2MGv6MFsr7GFIULo3FPyp21hlayfob%2Bz%2FDQjQXOwz3gYY2CR04JRxtxUc8uTdENkfzSX%2BYDtC67RfZt9Aym0c%2FUE3rU5iLC2%2FS6SXc%2Fz86lWK14cRCPJJbJszE83arAhcmEv7KYzgKnndSfs30PBq8GxwxivkoJ5n0q7CarruGAhaWC51FK%2FFaog9twNx0S0MJZYpdLyJCwbQq3jC76%2BQc56k5JJFXmFIMiePsFZxN%2BMqWof9KN8nJxqFpLH33JiauG8Od8EFv4nL1B%2BqbZ2tgPIMWqm57IT5URKLDQUlJVfnBl8sztHGtnc0I0UV4zAsQQyET6stRsnjAJrkltiTy%2BMLD555tqXNGd2fq9vsNVxRcAmIaD96lW6KHQGFenh%2BugTOc1dyMhK2VAMaICpS380yA0BY5ZNsJVzajXy6VeWLo9e%2BHiCXrzLRmk1urCysqc82qNZHv9r0cAW0CwtCEwt%2FjdxwY6pgG3gy5gtkEytqhe4WdWrow%2FdjYgePIKr%2BbER%2FIdQePftZ%2Bqu9ytF9bp1eoVV%2F%2Fke27IFFAtbISW5cDRVv%2Fode8XShyc3xSiNQVBg%2F%2Bfz4AorxtadpBsuG%2Bmoztcq7zCeL19FpUewnjXvdtfJn%2BQU4AyB%2FaezFJFnbWuLemWLFMhEMuXs6TYheyf214AyX8m1F0h8MkWYAft%2B8Xrk1wxu7gBM9vOmc4E&X-Amz-Signature=edf475ee1f9d3133e61fb18eeed6c8ddabf8fcd1acfa30abbb113b86a5607e80&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


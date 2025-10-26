---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GTQFXXE%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDz7RkcmxI1zbhFiIKuvmCiBBPFLm%2Fd%2Bv0HPVMx3iZw1QIhAOq6xwo7GtHp7VyQVzr4vNbv2G33VXQ761wcsuVsWDVhKogECJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxq3O1MYdwYOgjHMYQq3AOvBk2ghuSuoiaDT1CqKHsKPTaWUwvZrqeJ4BJECFeOw3crouBSIGfkMIs3a7v8vzdvsW6GO6CumUbJWzLO3NCDKYY5T2BxmsjsvlMECNfnOqNJl4v%2BDFeHF7uV8NPEXuFfSm1Q5gsiMUOadIlS8eE4T7wrtDVedIc%2F0fXDsowXMDsi2UM0DrWCPPI9M9wP8Htl22VBnJQMuyAVjMhY7WKdKEzhF0pOUizXivu5tC%2BgESdVtHuQ1EFuU7qUuJgVjtaswwL2tHFQtpmjdfIwrCTcsiyi%2FNFu8iRuShRgSUwabj9dw5sNfYyi3ezmVChAoqWLNoS8tsE4G9eU3VS1sRaGZf5wpLS8o7PDsdkphrmXSoNj8WJOBn9qKnFzYEkmDlzSRjPCUAapAAjk28BDBWijq1zGb06Ib6O35XFV2o70x2VH7lMKmegN3N7yKOyyXptvKSNc%2FlFkaVG%2B72mFm5TIabP4HrvfeW81PPlITELHpqmD1LNJ3J3iZg2YQhomB1RT7Xnfga594d1DlLWLnQxb3kHvv0WLUPRVxsf%2FUb4xgBmaQjupAg6rI95u%2FLKg9Iln1X6dD1pjNOizSisB7IySrfeWSYy2sb%2FtfxhdYr7uUQgociUcnigoA7%2BcJDCxkfrHBjqkAb69GFkDdCj0thxvyaABol8LPO5SwO4s3TD%2B3IxcNduj7mSPsazKLUUCEEoBB7YTovkjg%2FR9AJDF12T%2Bp1B64rdm4aCmLpFkF%2FZYO6SSyXWiKHYnsMZHBK4Y0OCRr%2F15L5bQJ9gLFeKMl4rckdwKkEGC7jpmNbWij5e5jioKUtrBTtLmkypc%2FrLc8AhwpZk8wbaqDQQkiOYfy3gQ5jCBxHUssans&X-Amz-Signature=2fc38e402611465b9d08f914559a40d040d96f1b33ca6367685eafa5f21c8907&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


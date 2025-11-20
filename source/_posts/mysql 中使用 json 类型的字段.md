---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SDCWQAGD%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T010057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIF9B%2FDW1d4wGlX9UgPGf7Itj3RyvamRn4raL8zeB1KWzAiEA1D%2BbYFgSQ%2B%2FllC49pEYwFWBrgKAJEJE4j%2F9TbQM5RzEqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOBZvBgQf7r%2Bx1fp1CrcA9FV6lZyDR33tXSnTYXnevIgziVzypV0e1TX3p4XPI67GVdoN6bVU4DsgDxUNSdw2G2kcwXBNJ32JPHuUC5BtAzFAlgQoplTF6%2FAtjksX4%2FJWOs3Qr3nqbbZNUX9aL7l7ScDWBXvmP29P9PS0qSXUo%2BWl39sMDtMGvnGofh0i4c9Jm6d1P%2FU6HanTdlR6kDezLKGH3DyNpbihSSWbJQT3CQfHPeKGc9kH7DYPcsdAb3W8Wamz28Ek1prnQ7oRkZJz5bmDh6fNvuWvrtM01XOGGHutFPHs%2FC%2FD5lmX8VFlWkvPf%2BnNBk8m3RxC3wYe2hVMQjOQsF42XpQbQOH%2FIkoP%2FQNWTA2lcW6WCXYGZeS%2BQ5uz822%2F%2BregprzEAWCLJKnMdplB4dXIJtE3krjBuAwLr8Vy3PKP9QechDb6qdpJX2saTDvAf6TOw3qkvpvfO0KRLbedNhGx4i9flfn%2FHRm8aDbsP25DpzCYMB5g9Rx1%2FhtnsxJJNkT4bBXC4TiJKhB0WTO%2BugkD8pNGb3IOtpfJXpEEcQvk6cY7%2FE5p7vWFk7FZHYTtumLhOw2dUAFnfMp1bPr0d2b8xLls%2FvmEm4bqtvKeF3py%2BcrpVW8K%2BYTrlP4VgbTdnIUVV38c3zzMJy4%2BcgGOqUBHRfooC04cHyHSoeJZAjuFlyxD9r70vSyvBDX2MKEq2%2Bqr00KZeS25HlWZgnTv4UEJgNeIZ5XW1OAFtuMPwpVuM4XQuPEFaMN%2FnSmmNPusBfeSZRj1H8dDwuhZqfm6mku8llaE1HkUEFC3V7AwMxas0%2BzPdIkbwH53AmN73g6uvunc3ZuVHh8KeKYmsob6lA9S4j935WA8egj%2BVqq%2FyKlBApdEb2R&X-Amz-Signature=31c1da418258db7625ab8394fa7b23b0175748c7726224edc2cc79660af4170d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


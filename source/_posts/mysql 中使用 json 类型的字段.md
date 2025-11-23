---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WET32FYC%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T130107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJGMEQCIBk00ZUExktKcS84gkQFf2mkrDZm%2F%2FKS2L11pMH8G0JxAiBnwPWZR2hcmKoOd2BEBleACUCGk3Sl5vhytJ9WzEcTSCr%2FAwg6EAAaDDYzNzQyMzE4MzgwNSIMhx7BPb9jS39l16HYKtwD8k9tzyAjkacxk1OpwYbmhWf8t0n23hPvfPL27rRX953dG8oU8GhNgWleKkFGQBpEtM3LxFM7NdNCon7Xfy90OX1DdqpqOC3Axx9WYHq%2FnbSc5dxYEE34f0OFSZrD5bpAXP81LDmlpORoS6X4aSdm6btPe7GH2tlAkPuDDWEqDCkiR3qbrJ47JShnnce5lxnV7979cKiZq%2Fnp2tyqX9eAQLMPXWOIVXg5qeFETrswKFjrkdkg0rWjkm01oDTyFjgYkAoNJ2UND0qcLzxF%2FLsk%2BiueCLO6jIWe3BO%2FVisinQFtBA2zhPdvPQkXADB9ND8kF5PDVPhRiEAXobXu13XbRoBDn6HJF3LBcbDUlbcksL5eGW58PbySdcvqd%2BTQksYYURFu85Yu8GgskKBrXBnTW%2FK3BVdG56shR9dmCSJzsVjfyVG2bPvHtf25tG4d8catbnFXTwH9JpNKVTi11X3FrY8ASzl85TAAbD4QR8wbFqQig36rEwSSZPvsDKFMb6Gh16uKjPq7fnVpdVzVV0hcjevIBMb17FFWzENzcBsVp9X5p3XLCsCV0tW3fSDJSkkOzD81msTPEQUaC3F%2BMMO2VwVUN2zd0DHaDyuYqVm1jwNZrXv2dNtdSg3euNIwuJaLyQY6pgFFkOLAhK2ZkfEY%2BfPH3aYXTibUeK8VaTqhiOrfgG%2B1%2BNfcFmCDYoplUNSu6HxG7xXpBx6A4pLF7F%2BzY%2BIxRwBjNUHt%2FpWG4rTM4qKHkgkRwXQvomabkX4WR9M%2FUy%2FprLz%2FlIms83XeI43dF%2Fv9vOD4RntScXw5L34Dg1jxVB1%2B0m2RVogIwl6LxHFLWzvcznZv8DEpToV0YEc8I%2FAI%2FLP7s58nk17R&X-Amz-Signature=9daef01192a92871e03b73d7bc8c5d9e1435f30b3899b87ff821c9b66d09fddb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


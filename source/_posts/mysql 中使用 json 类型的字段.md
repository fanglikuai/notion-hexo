---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTBO3U5B%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T150043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDWyJh7GcywTWMocmxSuH%2BXv3GJQHhDVDSrtY2S%2BABfWAiBOT%2F0iwBCiCgadlcUhwtbKq63ruNT96aiiPDC7FnCmgir%2FAwgvEAAaDDYzNzQyMzE4MzgwNSIM53%2FKh9O4ZHR%2BD363KtwDTASjY%2Bj5Gqz1K02kknj%2FP3I3h%2BYYqLWle0Dq2Yu6f4TDx1NKBySF4RRe7H7X8%2FNX%2BAg99F11XutzQbZ1uhr76AKnn7b1bI6D1nmSs4IO18nZHAmXojc4aDVylP36RQC8UsCHHhuVGXxqxPtbZk1FNuasZ3JjiquNkdVVjlLA4cwtbfhh9prkxkoBmm55YnXLCMCvmDJGAbJsYEZVHBSKGRgueOSvBp1IvNrnk0BVEUFobwJYbxHxGSpsMqwgApkriHxr3TRDd3qhBsTooL%2FIYlM3XEHjzG8hbSpGuD8VrC1cN%2FEXStg07hQh6fWgOpNJX3SRocaKd2C2zghhsK23uw8vsOPrZWbTOrdD0fKTvKBOokHQ%2BG6OjBBxGY1%2B1sbiHGyfgKCc8oVdoph3VRLKfz%2BIhhgRY50rf3Puj%2FFUQxnXYoDVU84W%2BxBAsGGTyqNlgnS59mABv%2FWyVXxmKm0%2BQyPmho3YQ9XblbdZwtcAOzC6H4JeKEpRHWB50DwIiiwsDUCv3DmIOgQMXFg%2BjtxFLKm0NfawRFmzeNiZTpj4UAvtH4jJWexDCdDarKB8BTQVB8TQBQJdFNYuqTSfqjc2fQ4BvxW8c5f6EPZ%2BDORKDDRprlReHVeKqgGvxNYw2ov6xgY6pgFAuitGRye%2B1RjNkUJP%2Fabu2Iop%2F2NgeOSy2MbhdS%2FTNWfG3z2ZuA%2FyZ4SWcGflxx%2FMC112fcqmfTj8omROtaOyuPP0nO2T48%2BoTfgOgxL%2F3eLHBceaWGyBXUPfli1VqFHI9WvUWMO0uqVQJ3laGeqjT4porwncwoLlC%2Ff%2BKJqpDmCGcDP56PwRsnGxId5Kop90kKauhz%2BG32cEZGQcmiaXpM4NUAg1&X-Amz-Signature=c9ffe33eac4dbeb96f36ac0862ccd8c976227fa37e0cfe3bbe8be0c0f3264163&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


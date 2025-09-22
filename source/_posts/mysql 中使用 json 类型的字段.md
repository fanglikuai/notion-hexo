---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7JQT6VN%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCh%2FNN0VHopUgYkOzfddziOF8ofea75p1VWq9y2pEX9TQIhAJuOeGuXvmSU2ey5s%2B4acrZWkr71v2vqJmY8Yb5hqHHxKv8DCDEQABoMNjM3NDIzMTgzODA1IgyAGdQpYnpcnf8r4KIq3APg3MfAFpd1iBpMgy3%2BP14kJA6cL2ik%2Blf8b%2Fcvv2tmkWo4P2gjKkMNHb54NjiEgfdraRmPaSvF%2FYM%2B1upwklNMAq4riNjejw9YABuyxEK%2BpIFSyJKGWBcG0Kh9rYSNrq1Oe%2F7k9jBg3rBUdE%2F0kPrU5M8X38IrPORy4ceZlTZjlW4eENdLpaXKl3k8fVSETVkv7ZPE9ZpomrSmjZtLkccu5Ag5H2zbhSf21dyZEu1W6mOmeQ8eKezi%2F%2BkiCk89600iTqnKWyOhUobYHYAawFaU8HThUEqA8GfR2Rx9cfZoU5io5VuLQcsmmhYV5N9ROieca4d2j4Vzlf0fKESBHt7D2jfb6Wh%2FXv4qkIycTUMiu9plhNe4oKrpNEL8uWDyQ%2B7J%2B5s%2FDdMmSMmrViSMh0hTZFF0kcQSSZXzgiTMatngS93rEntEuVYMoe%2FkGsiLSWo7eT7ZvrSqyhQS4eD6tMn6cXW5bkJaeOSOcBNjyorptaXMCoGnDM77TYtD2IDUunmusXyYuCjFR0gmB0capRMm%2BMdLdTDoy9mjyypHBt99mKfxEd2Q2gasNq5C7lSSfcrU35Kn%2FWFnXRO%2BRSnAvmobmf1oTruGsHupEbpemvUSD3yDDRgHwpkcSOmJKzDo4cXGBjqkAaZCw0emJW1o6f8WZLJSe8FvOrGDQIETjSlgTHndAlDJQqJofbOURklenjYoqX607LCFu4nDDxCSnwZoT0Ob6soW2QPKvxj2j5gV2vOqIk8zSGJD9lEZMzeRUd0kCTMAQ7S1zZRdnvfU1Fn4nJserJ8dXu9EqlNxWf3ozQ%2F9Bvo5sBq4CMcl6BOB525CInkaH840lCcpXYZE9XPtX6zyOcNkOX7q&X-Amz-Signature=a6720d8e74dae1de07628e6f56b3342ba3c9f60f3224db0f27fd7aa6950d597d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


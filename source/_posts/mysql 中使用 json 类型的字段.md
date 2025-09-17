---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XBP6OEK%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIBxeUGsV07frmZ1XuY650xjMmoBBD92YSI9osoSc17W8AiAgCw4oQqvDkM4cvtCjlhwSmT6rSSvcn9ANLZeUSFvzLSqIBAip%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZp69vFpMb6GvPpgsKtwDkmjybnuMrqv%2FD%2BDywEh4JP9Fa223K6hfYO26XxJfj7E5uNpO2MSc1KLcP8cYlkDtS573t%2BKRreGeHqWPrEqBAYEQbk88NCEFBXaIEFcc88%2Byp9m503HHLgg8KUIUj2cO%2F%2FPZg65AORYG12ZN6s4onKokkSPIP5NvNuuYJW%2FrMAC7gvRHN1ZADCWtmJWmwNhJJfLvrpufRH%2FKzx40Jc%2BKd7N6V8kzqtcO0vHPLbPU78DYKWoOwufV2zQ%2F1N4rcQVyJJYXjNKCAMK0yuB0vRP7vWcwe9TBe6a3uAGb5ufxbSsCaFir6ODSIkuew8a3eauaVYIYARob6uW167qBgl5z8MY8ZPi2p5O6vtTQjXKr7ZyHgFgqvoIAMIt9IP2h287mjmOupH04LHK%2BpaO9mB2j93cuNsp4y9uddXpZdYvW3m6WUnaBDVfQC3ca%2FHbUCj5itJRqopGPhJ%2Fg2UkSAewVWJexskCTM%2F4jLYBWglbrAHpuzVpT3dIPtabuMkIAH0bElSVLnJRZ1voEYqX5FnIHtFOKxG5pE9diyOTs3ExNyfqzT9yOHSBN0mWJlc%2BHrPGAapdApxLcaSaB79Eh%2BwjaSavplgMrvCuuw5BugxMgAiLfRORuEj7nu0ZSUi8wgq2rxgY6pgEy%2BWKc9gyGJ1uP6bg9MdFoF4aNAc%2BaEHCQKvmdcb77TNbBI4NSrO6TnF7w2tDI9Av4dSnxGmJu438f4%2FwHR9U24iSneqxoe8dNLaR%2F%2BQhAJqtRkDsBcBRkjjvZPBHVc2K2aICpzym5QsUjYxYov5USX6HV5Hb1ggfMkT5gAHAzNFNTBQCDggoXO5OzJw3S08C5GOfjH5hXP14jZHmP2GunkkSY%2BcMr&X-Amz-Signature=d5dc8d58fe81c7dafcce72b73f1becf17ca38b9f3994b22a0771ae08459b86dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


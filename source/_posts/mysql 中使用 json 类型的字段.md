---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QUGMITRF%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJIMEYCIQDWmmMV0PkJKeRaPx7A1ZF0wMZrGLhlz5VicgOLlcQ%2BygIhAKCERDkCZDz3q841bIHDXjB8Em7%2BUJlM%2Bnr5oK6HzoJ0KogECNz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzkxn9KYrzC7V7Mqu8q3APMC8z%2Fhd5mNt6gQHrzvyaI7gSpDPusvemoQZ7DoODE5GaXdhYZfUIctoFRrC1PhGmQe3NUbT%2F%2BxNZpdsbwFhWgjRStwWapsPoHZP7j2QhdmUdwB6utOJT0Al%2BJe8EHhF20rEI0s2HDHpbds1A2mOJ46Q%2F8LvotNY4DtZOJQGDx61jeto6jMUCFNYLvMHvgWx2JWUSKU%2F6MbbyJg67q6cE284etCAMT1p6exOiqX2EuHFhgei1o0i7JZFGe0tjqaUgkcDRuDjv%2BMzvgDeQfeM1qx7xGiAP8KGxeMr%2FbG5itbqczoinPJufXpPxqMdXm%2FtFBHq73AmTNQbzfCTIRcYcZ%2FndbdHbqAJzRF60TI6T1cmQYusWnEvdz2KnD%2BFi94%2F5PIA%2B8zoKo%2BGb6GQvs5RweRSPZ1l9laz%2Fgv4OjdaA4PQR2UDQvui%2B6pNdFhnBlmAG1YAcnoYC0jMYl70XY2wN5Yj2Lhp7rcNq2EzsW0lxeCrjHn9BR2xQToDmZxDaLb7v6OB%2BwYhDsOr4kO2UB%2FfolxJq6wNJcB16jqhZn363YbKXR%2FvwHm5XdliOYo1ox6K2ulI7MwyitmDZ%2B%2BCcLDXLUXmRC%2FfFGab7FD9HzpfqzmDLGVRmfARyh26hutTDhg6DHBjqkASxNHpFhnqbF1Fg7z1siP0cPmMlXpBB%2FGPsH3Bi9Qn8tvxtgCboibfJ0Fu3kg97tEhM51dEp4Yncf6Ufbrt1S4sGSG%2Fr0meYQFmrGxxKJzsEKqzzEGXRzW2eqxpBWKecKEkAUX9AdBvg%2FgnujT%2Bp2mj5flHDFl5w4UaOjEY9FEjrWCyE6s9AyVHVH%2BHd6yiaQBzP2sYaxWbt4JevIITStq2eAK%2FT&X-Amz-Signature=807724446092f7261309fc03efdd2989f2abba9741073b83e743a5539cf43e6f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VBY675YZ%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDmbSX34bG%2FY0KxIUaWpi38bbRyPGMN9lEqGD2VzrXxDQIhAI8NTBPDo%2F2qYR3lhCjY2uh4QV%2BIbp75ZWkIuiI40S2bKv8DCEoQABoMNjM3NDIzMTgzODA1IgxJJEy2STtv8NRAz2cq3ANuI91DMgn11lRV%2FOWJaUzs0Rr8N%2BStxqdPyLXUWqsDotEDdUNFjvyYoSAZT4%2BhMUAXnW9Nd52H4SkaXowJReZf3MKODuTAE4SqJSW2GFPRl62EYNldmt9TBHdr7d0rm2j3XAqbAY85jZxNrG07bXmKYLunIu2seIfD9CjKAqaJPwKBwGNg24U5zxkG4Rf5OF%2B7QCC995aTaf0kwhATxYMAkJ4VuyqDbv9s1yu4JmdOw152%2BVcYlIREMLjc%2FloSp3TgyEkGh0zEnThjp6XxEQj87GEvaNx12ETPiMAl%2F%2FZnbq0B6Iqo%2BSAkaWTvk1DD7Ybzah76AD01AORXt0jQAHLJGO4Pd%2B8h4JWvWoesJjgVJuwQF2%2FovcoxA%2B7fDDwVwitR%2BVs6wexb7FoYvyFL6EmhUBHkLDKA2i%2BETeJMvCyUKZGABaIjGfnsgOhg0NmX0cfbrnetGtTFaX3sSVBwEv%2BSziSbHaYsGAbO9hIVgDuuA6RUCNjtr7Wta9qd1LjwPjFGzK%2F9JQ4EoZVAqOJwTEXRHXBzo1Bwf2Xacq%2F6dR5QjvlTcmyugNZ6mZULC5M7ZQswTTNUG0dStkDSMj9CDJUUXAnevSZdHpN4579VoRmnKM7o%2Frlqyn2Buo%2Fr0TDSmsvGBjqkASNUovOtCu9K4dkFM2t06bL552s8RRwSGnqupOoNuxZLhFZ%2FPrxt%2FfAu79%2BlMiLRj%2FVnuOuTiK1FX5%2F6dzjQMw3ngCDLRT8nHhu%2FOrW74H7C3wqykIKsyC00bTM1FXn2DvGg%2BaKgOUfGOSq%2BKc%2F%2BTCTPsim5WYfQTNbOdgIEOeA%2BJYJ5tnEzx7gDeMynNvnmKw0iKvPdrLT6d8Q4aWXGdvehv3K%2F&X-Amz-Signature=9df680e8aab1733b72a49d14ccc47a0ebc46947023c11992974cb448997660cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


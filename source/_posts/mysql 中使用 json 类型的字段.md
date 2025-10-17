---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVDNGVDZ%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD6m3zhQMsSA4FynjWqi2jJCXBBDpUPcTdTTw1WRbR47AIhANIO4xsRQBLzIHGc4J4uzrrtShV%2FPch1AULVsCoLGUMDKogECKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxOaSrq04AJDdy85VUq3AOoDTOZwbRtJv16KJNLIS0lid14yOqoji9vBzysXBD9udwLM64WZs1Vs%2FTo%2Flo2GRfjS27OfKvw%2BQVxysxsI8VbGMXlAeg9DA2YPJvO9giqjFXj2OlDCcqtl5%2Fg0V0XBDMJtS44ljnWX0URUCKTS8%2FDE08e9Ff4gFZXAKFKzDvHvWkB3rb3ETLwzATzpfejH8vbQfj%2B61DkkwcUid1j8i1AVpNEy18FGvkvycXg9QhSIjUCQJoF645YG%2BVqjp%2BG%2BTdZgh9ttmpRPst6eup9ica%2BuEk9TN9voHpQuximRwZLxeCg5JSUTaLlUHLDyzFzlDTTW08GngXXWJ6QXJHv6PYojEfvnMhrSmLGxZe1%2B7NwK2KkqFcC0QtzXzKBnBr1Sv5S5LIhdwFbqpQjoz91SNPfHNGfDWICOK7Rvn4ncPFfmLf%2BE5nUK4t%2BYN93uIWUo3Ql%2FcVtQivHxgUQB1rb4WgIkltYvARqqfvfIXzgkpIP2MnwcqnWCBrPxCkz6T%2BklRV52K%2F8R6cPTSi%2Fmxkb4Tj1jwAOGczjdLv%2Bz7AsUcu789XNQl2AFaXrdWR5vRJaXawihryDzRuq%2F4JTbFDcLTjBvZfy0NY5FyO7ar6sLGr4mtwn4UDlAiTl4p%2B39DCt88fHBjqkAfqgv5PUHe73ssob69l4xEl3ZATvzhoiCtazkFPq6aatEmDID6Uav6gFUXR4Zf1rYw0XueA2qf9bdO6FpGniRVTCjRPkQq2ITESbvGTkrVC69OgWCHsmZXdkrFKYSJthWL0U1TXbyt6TZundteHjIS9aGLHOfIgayF5ZEeCNsRa7WpPTpDEri7kMWKlyNy6WAthYWOQbwpqq8OQi3fqmjLvZdjWg&X-Amz-Signature=1f403097c3af16b1b99f57f9f547b9cb67865c53c59451411d1c5ea8a8d1f8b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


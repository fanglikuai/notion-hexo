---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662QTKHNHR%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJIMEYCIQCGl6CAQl2cpQtCti9H5maTFvFE0mN8dd4khHFkNk1I2QIhAL1F8%2Fog6CJxKYMAXVvjn2UyONy4xc19HzsFvF%2BGIdBqKogECNr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igylj7jNfsByed9KgHEq3APXRa5FlsVsEd2e2ttyYVLfdcMXLd19e66F5pXV2sG5eMk7dFf6wH39sbkk3IwT25mo9uwyeZklGuBsYl4LY82Vj%2BwVc%2BlmDcD1aynlEoiUakAYWcgM%2BobHEEBw9caVqEfVo5iy%2BaVC3ujIH7VG34TOVj6fRGgtUL8Kfnhy5hVn%2Bov7iTN%2FgSiHCd9msS472HAZdXp87ZfiGZMNeTEmz73IW0CwEc%2FRcFvYPPg5uZvW890Fqv2JDMNfcjACL%2FtYnR%2FjxoE1oP5UAyjo2j%2BlI2BWJwl9k%2FiQroxbcmwzZVH36FNcXPriYJvZ%2Bb0rlh9ZiLQjAO5gdXLqfxgfCqCtTKuCMbcRM4uq5VI0BBKgQvat01uQRR84xaikW%2B%2BNd1xdqVTh%2BgdoW3O7jLkmRmUyuxMxvaauKZFZpWgU6drhy4KL%2Fb06h1XvGXw2glo87j0wOp9cDq4NxliYt9%2BovuE8Iq1sQL2Eu5fFcI2dDjDxGvJ2zrSK8hVUUf1pAB8rK7xOBfCdQeyxJdmhKZK55s9LMD7%2BKeUenVKijOtYJBbWxY65T8Rt53MFA5sgTa2iN%2F%2B%2B8x2c32Ki1mI8QzEB%2Bt%2BRmCMkvaALXr%2Boyd48YCJqS%2FREziHjZeTl2i6Z1YdIuDCrnInIBjqkAQbVCLCv93lTa3v3e59kCmFQrNWl5hfZUJ%2B615qWJH7eZE%2BLqm8I2MBYQmr3Ht5%2FCTMS2P5D%2B%2FxSUyGbn9BVAU7lX2nmzIJ5ELS%2F4r07rlAPMhskMR0EI9hXvx%2F1XICndwndoYepvXx2URhlj4GbjMTAgCwxvuXlrDHJmyQCim3qvGk1tPV1AWpGNp565GQAA5EgBn1pfn%2F5VtF6BQoAy9FIvKuh&X-Amz-Signature=0368e46955e60cbc5dfa19f173a62781deae58375e164916f9b5b6812f08535f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


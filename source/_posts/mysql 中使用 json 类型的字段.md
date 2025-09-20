---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDIW47HF%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJIMEYCIQDtFgSuiLMUozH%2F6ptq6WgdjvPe4chgl1KRIKkU0KFqYgIhAP0BoDDH5%2BKK4KhqybXBPH3Lv1Hztuw9%2BOEWPtL7ZZJdKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwtDDTwe%2F%2F%2BNes0hzUq3AMNAAwMwjGxAfX63YEe7E%2Bqf%2FmbXvXNJIOP6wQbbh94HcF4ukgLTsJ942peP9CaveeV28VQiM6HiU0z32VblJJuZ%2Fv8qCtv0U7v8BDZEKWm%2F16sIROJ1woYUzy96mNHAYICoKXqFPdU%2F3gRlTtijDxIn7bSj1j%2B6P1KS2KCkxpPo6A2NUCofDBV7oPhk4%2F%2Bo2EwsSaWm7vlHjIc2Wo6G8b1CzbuqVYqAc1WUUpnEp%2FeCqo%2B7f%2Buouw4i5hWmdefBHUSRRHDgYGc9BwNAdzeVMyWRG8dr5a7AnIimSzZYl%2FEYbGPIbwA9nFDt4qMT6GEDldPicsazAj4fg%2FqW7CC%2Flid%2BwOAtNZJvPx9tJwvTUhhXI1Gfw%2BTztoZPY7SRRigygIKvHhNrBCyc1vdchHFk0tEFkCX5nxDFr4%2BvzcAe6Yh8ez6K8sVrAm7VoNwkfGY%2BppOxGgIALGBuAdFQzL79tXqWQcsY98aDISP1HQSfXXJ8rcIKEqdg9H82CoL7pNHEN1h5crZ7WFBQ8swUHaLODjWD42D0y580PkTsEaCVdl%2BHHek5bbQPu11vGBdxOvnXVD1SkEHtxc%2B%2Ba2SL1Oj7F0E7NV9wJJAkDB3xld62C2S0AeeUe%2BIRZ%2BxtthaojC27LfGBjqkAQVvPfHensH9%2FQiddxNdqdtKYNR%2BE7iG8D9JeYDEfNqUsL0FC3FDSwA3KWvbEnHu0t5W31W1yyAckLVH6dwEp%2BzZqcjhD6khYyZIwAkq96VuD1huKZ50YDELlOU6HCdbr%2BQQuCmzeUrUSVl5K5OZ9ABt3BRL3BjGwiyXQaPbULxeJx6%2Boj3YNJt5c82%2FAZmaEL%2FxTKRHMnXFkH9MAb80w0CIihEQ&X-Amz-Signature=107d10067d69318934ff4558fa0db225dcae3bf4c24ffcc936dd164c1962c82b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


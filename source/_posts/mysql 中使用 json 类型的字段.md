---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W2GC6QSQ%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T150102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCg0ohHdPaOzH%2BjyPG3fpKsNygxKnirRMDxNv%2BZ8tUNRAIhAN4UfnRzzFyQ0afMqedxmDm%2Fx%2BFHSxctaaVwosjGX7aMKv8DCHcQABoMNjM3NDIzMTgzODA1IgwbE0ONARHujvGqQ5cq3ANf0bQV2oKCG3NiUHEjZaU50tIhVbjAHMJquS0%2F%2FrhgHvfwFpYHpQ4adlkM2hYUUck1TK1Sb9N1ggF63cKMVHpESxLf6dAZpgSsM02K7crpxaHUaMh9nDlvk6PQbm3VbK7T%2BoyGnFBf57xP5b7A1EZcR9QA0vCCGCtJPZQ7V5MWu%2BNPbnWu%2FQrJ621NxppnKMsee2dvz4%2BKZ8C8wZyyH8mH7Lea8PTX6PMcmv5vTWqJRmViJo3H9agb1o%2BEyt5CW6nCArHwAFbeerBTuQwkJsHvR76xK4GE%2BPAO%2FGMR9MoNInRHJ%2BfkCV9tCMsTdev4KaZajOmhhYkipwSjHckQYllTxJcXEzXpOYiNgxyHxSLtXKt2qf1pdbOE41dvD0qN2iQ9iJwVu61GxNWVpmE2Re6QtzN77rF1cmBxm2gOpXUzk0qQKCv4exkYqjKGcCaXCv3%2B9bGj1DQ1CgZP91UNsmtQ%2BaCXhyaEkjF0ab7aQ3SELsajhn8DD23%2Fvo8PDUXz0HSkqx49JPYbhFOVtYXSL3hyHJ8Z09oLFKYC86ckIzKB6M6uNAqaNjjSKZwas48cFReJneM7cR3%2BSwXPRVfq9A%2BgHG%2B7DYcDU8GDycmULUF04JTtyZIThQZEgepd6TD4itXGBjqkAdSCVBGvwgGkNIL%2BezcuyMPb35%2Bs91zhCRrL63eCmqDA5W8VOi1n2OErJsXNxiil4Ab08DBVANEEu42G4PPqqFt4SwhKeBXrupqX3utEHloQqTbCQGGcCm3NPFKn%2BfbXr1zBUQLfnk6yS0ZWbXNU76aRSAEwJ1ApYmgXpKWTEgzppKav%2B2nvE6x8fW62%2FntmZ7KpltQ1DkYUM6xHXKjPMW9pvAHG&X-Amz-Signature=1265ade02f5944cc56d809388e0e493201728850526bf1e6655f0b0b10f4ac17&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


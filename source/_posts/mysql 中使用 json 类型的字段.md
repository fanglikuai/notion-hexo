---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IECUWPL%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBvRBrlZHscJ%2F4c317DOkQvkbjzlQE4h8pDnNmxkrM7lAiA9uvdho9QCn3niFd8tfWLfRCGulpT8ghRwuugLyBittyqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlG4MjjQGJO4LGNWGKtwDnCEetrXgK3u4IBEn9bkN9E0CCI5bvhIIIajjjehp1TMtCkJOEjA4o0ZnAZmZlgZk1lRELl%2FfZPpegt9tSjMs9d5TunwIbXSGUnNcXeQnZkd2G%2FhOsHa8aqKtRHs0lkMnZBvhP04I22eSnrN%2B7pDtcc1HhVGRYpgMwwVXgNBo9Ga6EnjLU1coycWm%2Bqv1dTRj0dq9LirU1vOhJdr3fMPZO8UeXD%2B7ilqy5ZrKobscu1FZEb6PNeKTuqQ9M2VBEe2YTIdVPkFhmTbj9a6MuDLbSHPxQonMCT9oO7TlwMCYjcYevm6ZkBDn9wzKUokj4ESTW03sx75135S54GcU3WYP%2FKxTRuUJt7oUgfdYH2JSlIcjg3N799K2%2B1JwP5kcK%2FMLmv1%2B8D5XNk43rdVAO9UkkzOEJ27m%2Fo0q8jZ1Ca%2FrfAlE%2FYcH77BeDqXE58So%2FxI3%2FjxvAdBvWff9zIU2KTdkFNiDfCDqooy5S2hWS6Eh5recZ5NACm0RaRPRFnjgVocDR5Vl1vy1QBDCPMSeS5hxdXTE3ijFEckv%2FwwsZvK3CHhzH8mXFihApGLuIwu%2BthdUEA0ycqW23zEqLQTt3akWAK5IvVKf5orct6PigWsNb109v90qsCmHcv4Xi3Iw7bPCxwY6pgEA4Jcjl88BcZ%2B0PUuExWXSpRC34oKZ%2BBCAMj1sc8qTLkaF7SHcwXWNfSecl5ab%2F%2B9q45fQtGEJrn%2FFu1fjSVCu97ltAd8ksRUNP%2BdN3FI%2FYgRtpLudNe%2B%2BfssiFPl%2B2LXX%2Fm1XZfzHraejlTwfMtO4bEyQlnqedoiQ15x5%2Bg29RZhKP%2B7VROPrikrz3fEkOBiBkyfDLwtyC0mLIIHUbRfb%2BECiazKq&X-Amz-Signature=6f2de8e218f58d257c530b8507bb93dd1592305dfb94fadbd43fa712e4cf8641&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


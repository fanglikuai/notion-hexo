---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MT4CVKN%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGl%2FS2ngcyLulVglnA4E2qGj6w3OjFDx%2FhSGLOxDs3l6AiEA9EZIAO74baOF4AmQu%2Fj7AYjM6bbx3K1WB8%2Bsvse%2BTjsqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOJvlaDYis%2FA6%2BMP8ircA27hY9%2FobQCy%2B%2F0V6Habl9xoY2sAFIJ6gxHf3QjfSXZIfA752GcfzZKoWZtSxkZZ5TwcmnBM5p0m9nLL369WCmYwLJpXbhPR5YlHhqexhJ2grj9DPS0BPLTRMfRVeKnQf5gbZ5W%2FJddjyR6%2Fz0IBQNUgnfkvsH8cKeP5SNMPjDVEXaL8HxSyeaseOPyVBAYLgYhSWOP2mgP8kVcu1Wji%2Fvk5QOSomygDcJkiLhdSjN4rXWkS8WMU0DnZ7Or4wMRzX5lGyhHDucz73ZbKKOG1P3Jtkvk0Kvws7zkJjvkX1XvyGP%2FhV7%2FugpzuZDaz7NhX074E8t0PBJjiSSB6rkIcPU8w4Y%2FNiOYOHP%2FSJBX%2FluSZviX53JpWVPV5Fl2HHG4fl%2FiCiFrWiKkzVqy8Y6jZMwiMds32MAK6rKNmyJvk6eK2cULsTXAZS5aaoCkHJI4%2Bphmfv4mcnTIadxDdGhboZ%2BIcEdKq8AhDdBCi0LA190xr5w1%2BU0OdkxGn%2FFpl4F99IGvksgldKILGrtz%2BUBZXVkR%2F5gRW99cVdUVd1AbrusiyRyaafwwcSC0z0d412OZaALMQAKFz6uWxJhqScGxwh7szx9IEWmfMvCO1rlEfYWtTTG%2FuA8emFmnUeGIUMOnAxscGOqUBaKoHiddeKQYvIZP%2F7pyxuyYGypDgpiZsnEYy2nicSMcrVL0Zjww715Le7NU%2FGxYk3oVqiHMZ6Batez9j%2Bdah1NvCuLrHD6cCf1btCGHbkgn0bjKbxJLUa317ep45P8QPt6%2BprI0yqrmcWFNYMMjqIM9P%2Biz4qFMFP0GKnEUoWVToUZLHIDExKDig%2BMzddMQapgu139lCi8%2FnmbjmsW1Gn5gEpLdL&X-Amz-Signature=f1c02b599ea49ef62a473e8771f4eb9257130447d79d24a0adc414bebc516d22&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQXNUKRC%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAJejGCG0KSP85kTWEpu9o07bS5sdhUPRHSKw1sP7hjKAiB3qCJlYyFUq24ERa5Edi%2BWuUDFaL8Q3DlhNzYndbK5fSr%2FAwhVEAAaDDYzNzQyMzE4MzgwNSIM8KNUBrP8RboYVuo%2BKtwDplDZZxfEHI%2B%2FNzZjuEfgE1TF9QChy5pLzXRn3GB30hlCVfdD8zZba574Ts8Zys5xMp8ndm6UyBWGmaD%2FFtEjR8IAk92yH3tJwkaetZB3hyeREr7a2eW7x60t2cJtqgNtM1E8UFVbfYC1lnWzEpriq%2F2%2B10vlfdVSc5z7DOKA3bnUtXUrC7uu%2FDVXgCvbOnfUouPZKeZqMQ6yrJ58gtoyLuVkcAJ4RtWW4ecylhCuAzoHLZCYp%2BCgl3J3t2T35F%2FNt60VxWevpXuKsHwJkdyeuHm%2Bk%2BEnnvgpHpdg43vy44SLhgIGERQ8%2FWnaMrbvqX%2Fusn4nagNatsg3h6thx0rAHIwvVRjFQg8IDfQV8nlTyzuzs25JIX7OoeXlx6AhdqzctgIblaOMAFjNMYWvx1qYuvAKtMZU7NXZCzVldpyZCTmCNPV0KtsnOiVB0y4l6JEbJlHctLEAccwwtZMamhoz82UG1GCJB5Lk7GECEWM5zJt7T6LFZ9eeVTbdYnZ7fIqV4YRSl0IIVFlOuQUCoYHhQK0IycUrXdPNud9fBmx1NgpVOoeyk3%2B1ggctt0k6EVeNrbQIFcKgVJOXhH9T08Yxanq0EG7fB8h%2FuKDeegW1xyzODFqej9MJ3lhgTwQw7PDYyAY6pgGhtt%2BRcwjHT0JoYWFySzIvdijWSJ%2FCE%2FKkXk63kC8A7Ml9OQQhwX5JL%2BX%2B9UDdY7u4KVIndU5JI%2F3J0V%2F3LIMiW9ThcArhZ15UY5wo%2Bm2kQ7I%2BJCUosIS7S4Jo%2B6RMVKMdTHYzqMJj6br%2FF6RfWTNJDKrtESRp9p121nduEANsWgmkAD%2FR3jXDVKxUms3%2B8jrbeGTcXO0M3oDWVIVg26G2phq32rWq&X-Amz-Signature=548da3e2d662a1186a052e4f7f7aa5748ba04fe983709e7af17626689a6fe3b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


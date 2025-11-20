---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UI6FGHQQ%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJHMEUCIQCbSozs9Z6IwX2Fn%2FROK2e6dFMBUp4Dwop3Fl8gMBwC1wIgL%2BBX4JC%2FTfCCvmTmQqr%2BtmWOnPE5alNtQmU8rakk7poqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAAiwrVW5OssATMhqircA37cSch2YmflXu7budJAj7epq1KYAfYKdTIMd5eRvY8PTnV55Vov9McOi%2FvwbVXjUuCZQn%2Fj1E8O8ZqKeYNeZ91sAgo9qUbA77NGfs1r5nml4PuNFxM6FE9M10M0%2BKjAJwToY0pGl7uNIoh9bBBxWSEfS23cyJxygXSczSv%2B9ginF0VfBleCxvYpByZnupYyw5%2F%2FQxvmLpUbTZTkJfPZ91dlV5UakPGvYykglpU8FxoisKSkz18XciUE%2Bsp8RHCcrJeZFhFW5gMz%2BQgBHmp6qTeJdqTA1peSEMNUoUwXGLOjV4%2Fn3kpTFx2OgvKb4QjomD%2BzUTbcdcniF%2BzmEOug3cLMe3sqkhMxg9VWDFo6FYVQUSgk3awD6WCSqLHgpQwmlfbdHtuyoDSJ0jR5hTnJFQSlmxLOZkh5JQZ7naxp4vorn66kwl0Hrh5b0OndE2iaKmvnik1lD7w5U0aLKjoSwyWzybIpjSrNj%2FJoyP0%2BYU3MFqcozv9aQN7G44Tjv5L4zLU1suEv5XB5gahjrZ5Pg%2FJKWSV9pM2gZpQdiPQkKvrjpp9HY%2Bo89chyuE8U0iWtFzISHJMxf33We0jgEVcchuBbxduD4UUdLqxYCzEp1suXIjRUoT5Pp%2FpmdTfAMOv%2B%2FcgGOqUBUTyyJy%2FSRVHp4ScrtEMzt8pr%2FAPBJ8pUgnkU%2F1Wdh8t2JFdawqqLqsKwyYC0FKZayTHXvGiso2yAtzbzlvN80PBfG5Einnv0GEJLZJ94bIuO6w9qi81fcjP%2FekxQwJFp7y8zjbZUG9xQilNqmc5By%2FMXTdHIUK3TPBYe6zaVKtpsbOx7SxDtSYYgQgo7wnZwYDI9CApB4VaJO9qkqTuN4hDRJrZx&X-Amz-Signature=671a6e09a2b8d786648934d8ecc54c7f8ea95b81fbff4e26be1da0fb4dea1f36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


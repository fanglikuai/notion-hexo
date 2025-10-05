---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TIZ44FW2%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGIzuTagkvvnO7LUb%2FCYMeY8GGj1iFfB4TILY0PaA8RSAiAwhlBrlgv2gdGxMx2tUZdaQQeUgNCXukU9e1WoG36kuCr%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMufnFe9wLQVYV5WEHKtwDVTPb4OyjpFXeyYvo632d8dlgoeKmFSTzHjrO9PjNucBGEsvjxOzxN5wcfFc9UvJU0RfHToIG681Qbth0aE5I6%2FVg7DTl3%2F7uiGR8Fr0DkMNDSrSxK00zUp%2FzmAqehsz3%2Fnks5%2BOZfQ2c08vJH%2BZ00jjBB16wxspAN2HOx5tfeVWSG7I4DvnIl6BcUe%2FM4T1XsL5ZKY%2BCcfhDJbogvBuk40MuLmzv68Ypt7HncrY9ASGQo%2B4PjPSLJwrD6EBArJ9fSUUbd%2Bf7Qsyx3kDoYQ36oeTxFO34I3br9aHCtgbw%2BS%2Bk54txTEQavGN4Kp%2F9xc68G0Yj3CpDaAn%2BH0pS4hfR4ZGBUJ6H8YlrJBlLtVEdnEGolTVlwfAHdDukUdqCup%2FBdEWXurHjCn9%2BgB8%2BjAjKwVwVsXHSgU4ULxnH%2Fz2fbXBxRXTIeAcIWujhiyEzb2k%2F5cEoVXUUyQsMU7Q0q0CEoYCiIUEWvOxt%2BgtS8B%2BYwZ6aQxIYQshgwpH0vW%2FmJdbPg0RJZZBkUWTuwuungHs4ozg%2F41fqxYC0EGxZoxhrvqEKcsi%2BVeq%2F09DAXb0U%2FyBcU%2FafKw40heQZPmWYtwrrMq9Q%2Bc%2BadmaNhkqdy08AAx6j8yAsbPYu1KRInjcwseiKxwY6pgHq%2BSp0ncrzpmSExKrrpEDI8w2t%2BjBJvF%2BPfuG2BhgClb%2F%2F6wT74spKXBP1toXhb0BOVEJK0Fj4%2BNf29LH%2FaDhEh9FxNVyhAFUs2JldZpl35Km1fyz2NJk3Pw7LDQ6hCA%2FURwdr8fjzis8CMCTSZdbnlN3j2sdVVtKWgvv74VdgeovfLHc7pqoQUw11UMZ9wc99p%2BYvNvLvyBduq4D40tp5hR5z6lqB&X-Amz-Signature=b91d61b0af24e7d066b78b80e5d33f8c4c47f091730cab4227224e68ada61270&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SHZPIW4A%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDaVUUV37E878Wlq0NbApguncuH57q7wb3K%2Fj6Gh%2BCXNgIgbOcHQKwBsF1wDAB%2FmBtTkxJGwsWjHe5qasxSTWMVh%2BcqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJb1uJsGsfK0HWYBKircA1a0OR8%2BuQ3K4ig0KhsU9NuKp6gVTViK0WsjdQ%2FT7%2FFp8Abeqbme7BYH0ldRLkGEcz3JPMyUNDWJQ7GpuKGc9REHmqVh1NsGVtUuQefdxLM5vrtOslIJL8xfVK%2B21BYuNjZMKOmkhIsMn6CJTF4rBV8%2FPDFuPyk4xRi4HcL1fWVSZBraYHaPBSMfkcEWsDxoZIS2gFDpjuCis6ZzyMrAdGytAI9OeZLkIIi4jsSqpEmAxLlbTefqnBk%2BOqHRW3xSnITdw5dgIWy%2FVYCeXEHSYrTqPO3gAEE1zJgVENY3V8f3MarZQfCNOVxp5qNnMWxkko8Xls9Uhc7efiMW7A9qWHi7kcivy5BLS%2BFuT%2FAAPcMIzbZH7489E4uy8ZCjft7qu3Km8iS1Mrs5LaxwjMV8TjCv5wMURzn%2BsfMqx7rtIpcXbABOt3ffPduxKD2zy8lxAfvKJEM8JU8g6ohOYCma%2FyJkDKcVWrSM8BkaGldLybph8HubcDSJSCxKJ5xLqsfmuojRHRsvw39sq%2BAwXBqQS2kuI6EBbRHnSgWdUgsqe6FM4uG4DOfnPSOJQkt%2BhRzlJTLQdAQul7OLW2VAbGPHTh9TjQZEi8fwIi8a55cs9c5hIdLUtH0iVIVAmPjFMOuvxMcGOqUBcGowSa44MAuIBQhltJpm%2FH8KEy8U7QPSgsy4rTwodSkMNhmYdc045IQf7jrM1Bm0itxdlVw%2BP%2F%2Ff5h4tmDbKpaGy8Rh4eSHS8ecCj7ogb01u8xWKxCY6%2BJw2AjEKY81i%2Fv78xkGwL8ymhs0SW4RNQ0NFK6Q3LAkRSLv9FVIsBH2jRRFJy2q34uR8ib0eWYX5gv8XMlt8PugD5c26E8GT2l9XpNqv&X-Amz-Signature=e9c4cb3a453cfb20bc21e6b9acc66765a37d99ef5d89b329cbe5dd81ffd7b7a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


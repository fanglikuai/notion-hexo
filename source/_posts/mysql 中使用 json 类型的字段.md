---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CVWUXG2%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T090039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBDy0jOf3nY7u2ZG%2Bs8XUxMPFpMD8L5QQauYBo5lUW8bAiBhzL2yRvVou%2FGRB0e5RgFUNMlJONKjgVkKmfGfbYcB9iqIBAip%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM4IyensJO%2FVkN%2FeWwKtwDIAmMe%2BAUEYToJAOtljAnAFCSCWyBqes3kHjZsmzCi0HvVrvntKZg7iQn7pIi0m4y4L0mEW2NNiUfYBdIYsjAxpKTYzF5VN6ncg3tMZDfSqofN0IB%2BpZHnTNBmhqABJEAH90RqPk8OEq6aLg7IgOl2aKgM2k%2FdQZxwBtn1j8SU88or82U7maYxdkZ%2BoAjXJrmZwPsZDoTDA5v0%2BUZc6IeK8OxbE3pLN8z7Xd7kiHckkkZHH%2FnT5LUgrZhyXP%2FeWYwQjWOFcRSX3t5lgzTENYBjW%2FKLwl%2BzpTr%2FREuPvIOyIdtuGantGOUr68OmuYiaeVmpTeBo8%2FuQwRfgxoK6PwXvOz8LSzWq4YR6KwUBZ3jz6arqbCD08g6NboFMAHNSZnVnoNrqtVYNxgYm8JC98cPQ5OcSQz94CL5d3yOm032cggQeC34JSbB0thVJQrUoFeITWAl%2B4Adgg98sFOXJpg8qSF4X05VQ3gzF3t0IRXqd53gzu89VaIL1bMTTqU3%2F1M2pQOVSAuIIECkiFk6zw9qSdZzR9OWurEdlBcTQIhAJ8wtS%2B7eRMcrigJz4vb7gPfVLcB1g1ROcmZbtzEGeBufOCoX4XsXyTYipwM%2FziZFt0u8chXw9KuDdbUep5Ywv7XryAY6pgG8Q7eedQ5gFPgV0OC2fHxuCilcfuPzUJyN%2Bix2TQGYaq6uExROLLR4XJVaa9gJNIEc6xN2W1qjwsNr00GXH%2BrxacKfVanmf%2FsjQMnxORWQ1sB1grb32ZkgJswhAxiWze6JPoDkSxc%2B8jTqw8gwuktvwp4YjdUGhZFaKvOn2eEQqWNvwtyHgf2jteNqKbter%2BUC0qLDOoT35XqUVb5%2B5ow0udyG3g5M&X-Amz-Signature=c59aab19c638205fd90061482a98706f69c9360f2c92cb8addea8307a6edca1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


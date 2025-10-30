---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RVUUNRA3%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T100055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQDNAZxABxkW2snBI8y6IglOLNqHxq6jWweU7JWGsxCnCwIhAOpKwOivFEIfT48j7KlZtRZeYuVYn00oHeb4ECeetevnKogECOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw0UvF3UdZVJ2nXq2Iq3AN8kOSWzCb0hc7wFonIQ6PTiecOcwjDs2kiRXJhi7dw8AREWAKspTTtkoiY0aVwzpC1cH3bPBh0GX98A49LAsBY%2BI0oHPthz5s2nJYZXtdy1yzZ8PVY4A0uTbMV87kXMNWo%2B%2F87hdaizAK9Ug9d5Q9XzA%2B4kAiBv5SXo7nL%2BU8FmKS9rqZCiKH6G1wRF8ukW8cfxiXO1Wo1CO%2FglZ2WJFQ8UKiLLxKgCZQbRE94D%2Bg6hc1MgLONybeZnouXSvwydjdVQTDsTLHbQnkmhUtvb%2FOs0RkYkpGK0XXlXwAzWfOKnlbIChUVI1FjkcpZR%2BVm2QUCHBSQkP9AvmDM20Ost7OhAfPQEqG1EfUbKBLcp0K8sIHkfwpnPoQlGN77XT%2FWYkQzNTgu%2BssP2ocaraeXFhzqyZMI0UFvFDyQBCK7gBEZ76Ps4GHKIMo7YcgFbOC2jwG9GDSecLjKy%2Fwyz23kP0BEKOSNkmzFZMQ%2BdQm4ArqdDC0YInWyde1ZGObshgWJhqq8dN2ddOqyu1dpWOZ%2B4ClPTYi5jFeIF30BxJxf0WFQSl3%2FPToO9sfQmsbvNZwmWq3DlXcF8YJWO7PssoBwv4TkFlmV5W8D3B%2BZCYeamVacwqW%2BP6CEbCjfvSVxSjDy1YzIBjqkAZ9DdZuP2dkDHssuKzIs7G8TIpQ4AQ2cXstDln18OhCNcI3EE6r1FcTjXi3nmfnIwFfLIQIvFm6V7bpRYy1BswrbUB23l2YIrMvp8w21w%2BZ4k%2BnrBwrxVxOXcmLS7lsueHxX4h9GIqQETncsipCovIEmbEUSws6qYSCYSToKy78wuNIQtJ8mGvvuYC41UWUnb0Q9Y7LcA%2Fdcs109UKLQYN6zW1On&X-Amz-Signature=4f6eef9a0e6070b308f09340ab7811bd1ed8aec200cc5437c829e79e48960573&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


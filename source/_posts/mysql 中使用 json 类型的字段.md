---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LU33QRB%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF%2BlwJ%2BZyHb7Z4n%2BB1hjMPqJQIEm75Vl0ouXGC7MUbc5AiAOWyqXmw0RkBSpkrSKl0OvL1oT3XPiLhiynir3DJXLKCr%2FAwhiEAAaDDYzNzQyMzE4MzgwNSIMjWZF7jMFuImdb8oMKtwDd24qK94ZuzVM6ilZ3l5j%2FhANTK3y9m1lF9dZFgS3kqx4ZBIG9%2FM4VFmdMG%2BrTRNf%2F1iZTZT74KF8gHht4dZgEfTKBWiQaMCI2dqILpRN5qq9WXgKLgfiZ453hkCypiSBsCyiAbaNDrZjmvaxm7FMyo0WN%2Bu9gWtwgFdMG%2BmqSCG%2F1sPYtM5HmvE2JZJDforLps9A%2BUsZ2I8TKSRwgAYUK0nTBJLTUDzPBnNhe7ESWPxfrsCNif1%2FhY4C6i4mWnIbjQBpeOqt1aquNDJFCsJT0grV%2B6uzLzjxKa4Pm4owlN5nkHJkPMFj%2FkAUhrkSzdnl77SjdtTGt4DcFHKCrKhcq6U9r5VRImzvzRwD7L2nZppqzj%2F9l4fUBnX87GSC2UrIxn8s435ae%2F%2BA5zrDnjtOn2mxNvnD02KMHK9cowfKwV9Bs9VuTwyrIeL6eGoaZijUK41%2F4J%2FG9iRzThK5lrwwzVIzh4RWMlEx1CYoCQtIlLx7I6PgN4HME%2BdRLM2xUFj9uAKJSFRpK5nlFtvkptpic%2FSulQzktJLTGco0I1PSrRJMa4xvUIgdi66FJYhkhKqf%2BzYQQb0gukOKPf2cORUyQaSg819PKukgfrB0QI8Usbo29azuq65DO89tnEUwgr3QxgY6pgHITb1Ojq%2FiIKW%2FrSOUAh9aV4KqmDu2Tjiu7mJbRtKmK%2BpzPAkpOjE7Z0OXhQv36Yc%2BiIBD8L23a7FiI3NBZLfORV98IRWizSkQVrdStUtzQyDTS1i84m8iWRj1MErTr4tT2vK8t5O5GfUJkqZ7k5K9oxEVUOHZgOVq%2FRdyV%2Fc7HvCZTlzG4YTLuyWO0Knn7wXKUnXiBskulYTfDGd6TnM%2FUW00DapD&X-Amz-Signature=efa03ecbce45b7b05e1ded82e127e9fce41edb441679a1a38138cd3d7f81f6bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


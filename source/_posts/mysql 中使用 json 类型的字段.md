---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XSAB4U2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDI9RXJvdszKaeROdc%2FdKaodqh7WdTVEcVggL4MLhIajAIhAOjyWRoVwbQO9I5W5XbYkfV1CZaLZcWJUvbTj2kHFAdwKv8DCFsQABoMNjM3NDIzMTgzODA1IgwK1gUi1d5MOyhPD3cq3AM%2B76NjHRt1I%2BUmcuImEsQ7Zak2GknV7z%2BcV1yfe7fVOJghi9xaoS09%2BfKqSHgrcfrwRc5JSRYH8CaN8a88h7Mr0%2BeOrQV%2FnoXmNLwv%2FNu7v2LjjtdfusqMv02mhANnUcdz7l6ztHQohg%2FZin%2F5IjbOA2Rv%2FYF7VN1szbtEoiBHQmMRSJ8%2BoQI%2FOCh6UduaC%2F6b0vC5iNLhVQ3iRb8067p1jOb0A%2FnfC5fGjnzGIvto%2BW6IlU0OW2faRZaAjBv9YMqNedB%2BUUdUKz97dDySovGOW1BRpGQ2XDRFDygCTMExJEpf2myCbMiO8dZrhyI5NiGkTEJ1rDfaBY9jicfDk9uDHOXDl1G36L4NxWAuhbMGvvwmJB97fPQedf4bap9AcFnu38CfNl6d7qKPycOhlQNcF95CCoqvXxzDbkFh3Hg7xuqqEVs6otnBbkxRLwA3P5Aq9xgr6%2B%2FIijp03%2FP0M4ZebGLaV2wCXA%2FYRxgNxt6I4cuS41Rtxzd0yMRokmlLW1z7CADDew9FUQlUKiaDGRPFjk1OBXZRjpWlguQkM9V9UoydbIFlZsJyPbt76dC%2BD8qeq2xXRHzN6TPmkXhiNKzytsQUvxc84XyohAYFxa2%2FCxT89VI5r9DkS%2FNClzDjk9rIBjqkAcdnXmpY2%2FR5r0zDJtICk1x0s5lHR0AsTHaguFQSjzS32IQ43OclMIbaogezN4gL4%2FhA6pyp4jrsiPTn25ZIX5lUaY4wqme6EVSLi64hKeMjVsSTK3NwTtnrfl5y94w6%2BTG403bS0oaSpUZ011f4yg8nKeHaDGQFY7x%2Fuc%2BaP%2F%2BSZEhPmo5N5WQHrZGN45hri0AjmZ2GdAOUrOvx9S6pEdnBEdmh&X-Amz-Signature=b7b4844f0a74b59cc09999505aaf1c9ad91b14b47f268e811831651686b792c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


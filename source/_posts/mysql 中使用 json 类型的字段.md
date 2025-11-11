---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLNFM7Y4%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJIMEYCIQCqvorJOwlMRh6A%2FZjrEcVe1YToZQkVbBxUe4ubV3OkWQIhAKawUggLRx72WVOyX07Dc2yaK8gf2LUm7eOgoMLLjxzNKv8DCB8QABoMNjM3NDIzMTgzODA1IgxzNzcfZf6cEVA%2FInAq3AN51eQPZ5ZlV7Ynd2JtFEgaWvxWTtZ8j771rLi1R%2FsEEf119O1zcyOYZ4SxHumkQeiF1Us6MMa%2BaiIh%2Flign3ovdOxXSFvYRvT8CdvT79ejHxfcvEhfv%2Bk7Eiq5U1U8YcB9CrlEmh9MhwXYBJKT7sD3%2BMgvyI%2FAFG48a%2FyQL%2FLAeVizoQ3YFk4RC3G58yy8MUW6eP05nO0Szp0OTwGPUXQ%2FU9DcuO7qnZSB9fbDz%2BcqzIAvhDv6OYLVHs6D%2Fp3pzmR3HbJ%2BZoRncbEYquZjjLJUyBWcTqJfSH3feUZGkWNXwAcx3baTri7TM6YvBelIlEg7x9MlqxZ9PuuYP0I5g1BH%2FCLDa3CGah%2Fh5pVczA8eBOgoDyaps6xuuHvDYgF7s1PxTj2VQGAvjGhZXWSZymvtBuahCxpwYhhyeFlPnS5vpmLkP7V1iAL3Ex6Nfdt61%2F%2F89CD58vKPRAphpdEkFx6r9dEoxWj2tMc3f%2BhJouw0qrHtgOZT2wFleQjQnRMoGstmvrx%2FcLxqPJAqJwzofND%2B%2Bo%2BpFlyzJ%2Fr5dDdPx4PJ%2BOUyZ%2FMsytepCSEaWT8%2BdDGLn%2Fgxxq0dUsfEXbzY5ZU%2BSfdWOhcAuDGPbRdF%2BpTv2c5KUAxiewc%2F5eF3BjCD8szIBjqkAdlyKnKoPGF19zpUqXzZrDYHuC762Y8YfwWFKFWoWmZr05ixFi0GZKR%2FASUcCDHYh2F11Ze2odMKkZqC8LNGqakIfn8DDDVvaD06n0DEVi9PtHuJ6rXwsREqcMuhAvVcjsZc%2FRN9GH%2FeAhYR0%2BY%2FjJ0rijhIaQkaKOak6kP3SuordpzTL%2BBrkJfZQFUHTVgS5lPedomz8tZZNH%2BqHhJpKPZswJIY&X-Amz-Signature=65e1fa4fcdc447bdd984fdf25cf8c8ab7da0b3d2718b108ce819ac30dee37cd7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


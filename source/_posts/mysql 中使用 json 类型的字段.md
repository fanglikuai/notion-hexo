---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WQ56GX2%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T130058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDD3xO1IPFgQNirO6B4Kgjg44G6r3IQREebP8418xBA%2FAIgJlyF5qEslEWvb2owFFHN%2BRSfFu4XDrMjKInQ1I3vKBkq%2FwMIdRAAGgw2Mzc0MjMxODM4MDUiDMLq70AcOlHDRtlZcyrcA9fZVZIEnclmdRh56CppcQrb0Pv5Mao%2FVNlbLCdXSnndkHICbuUTDYx%2FRB9BB3NUNftRxw7juMJ763FIOFI4BV8WeYhmogBcDZOVgSturx6bObAywLd0i%2FDr0UA1rl%2FaAQaF%2F00EFm%2Be5pvyGsAIwZERBVgSsKgiInB2jk8CAny%2Bzhf9SMURC8uk0iXbzVB8NGEOn%2Bv8tzeXp1ZUbbdKLdZPiVBQwJopjFS2jMzWyxSIdK%2F%2Fb0YOqT8nJGx99PsSCqKFe%2FQhGy%2FSRqr4PrJ%2BwFiuUAzje6WD6WPrm7phF0fslynZOK6bmCYMZe4AZcUDxZFNtZrjOmpzjlCsHFbhzbgG1LicTWkLQLy70jH9FIyFf51pWfGXfsZ8G8SQ4p%2BOIMvakGxnvybT2hL6GFywsLU4r3VA0hr9ybvL3PeYypQ5yh7azDVQHKjQOEgWWqJJW%2BngsRM5RfpQ%2FaBBhUqOhRmMmrjGtqi1XFuJQWi1SV7qXuvoiGtkLGz7Z4%2Fqvs9%2ByUAjN1AGFDlHcOEhaeFdDHS9E523BzX%2FMGsUm66nGv3e%2B%2FJjxMKtsbSQUvbqC8Ez8bj%2Fy8qstBMgHD2kD61pQCLO%2BJNnJZS4IsbBbQwupayFblQpevnN1E1pmfx%2FMNHg1MYGOqUBmWaGEgbhbw2toxpVRH%2Fm3RPAp3gR3CEFjjWbOtVmzQsvmKXaCGg%2F8R2Cw0fixh6Go5fBGIWHkEENu44bwHh5SB07s4qp0dCyc5mvsMwhk6DC5e2eq5SbOivTk8zCyZpKiuXv7DoqN%2BJVXjvJxo7ayoOso%2BUCAavQt%2BfjBniTjWrS%2F2F05AapytOvfGd6%2BmlGLBOAsSWaAcxAje7WDCtmTeTTE762&X-Amz-Signature=38a1b8ba174d9127dd0aaa56515a1a6ebbad2a567852e8b71868521a33667fc1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


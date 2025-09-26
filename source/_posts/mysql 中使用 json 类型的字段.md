---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMKIUBKA%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T010045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDichzHq%2Bc1C7aSVYIGqWa5q8CGYacXCnooYpdvt8C5XwIgeKvd0oNBaigBQJMUcHCJcJlWfHdF%2FBf7v2rAGRF2ew0qiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP065v6zw4hnXu57WircAx4ckn7tu4HUPdVjZ1np%2BEmVVWNnTKer0pHxz5GMx6xOZczxxB8IAOL%2B9zcEKgDcUZaf%2BeCm5snAZFuGxf4KjDRYaXK3Rxi8b8M9BQVfvSQhyfPCyS6Z9dg%2Bt6uxMuBRdfwrABq9nVBLeOL%2FOUVIFcVV%2FMPjVSWp0JRUptY4lnSGUrvY%2BFrFbxWFhZDZtSnrQtsdJGWqM50ymlUdGOKBaeHUeRZkY6bpqxnDyjipTzUc5b%2B4jHuoQEYjw94LKGoYwZqQSNQdWSuYU1Ci6QlcCEyUyzQrwCFWmM3D4On5%2FwY5tbgu5iIcg4HxiOyWTtOsMphBzZv9DSfIf%2Fv%2F23PC4wUOG%2Fd6WPj7aM%2BaVc%2BgPyqlDpalNyldgJ0MJJQce5zxxNFIReIVARk2inNgoPiJcEKqo2kdP9%2FYtI5hHaJF6rXyyxR9qM4xb9OMRG8kD52COZ%2B72Tod1DKdhZBhYs%2F69ohoV8VXtbLOnERsv3YpIUAl%2Fgbx%2FUakXXMLVJvEpJbB8Nh1BJMa71L0%2BkB48Vx5XOsAedtlJLNQ19s%2FJL2C6wG0dTBWlSqBfYG1kr7tMN5CxZZdqFggS9E796%2FcVVHU1Bb0euptkbp1GhVh8NpYyQ9VUOj3Zh7W6PIjjq1OMKXD18YGOqUBDj8av5P4QQHSWBCugHr%2BGDmhW5LsIbDH%2Bp3ouLWn93t%2FbymuqLJr2S3e5WimwuCOq4xNjh9jJ8Wx8Vkxj3UegJR1rlU7ExtDKsQH4NyxDdw9zpJMMGZ9w0Use4caE2aFlKMrQRN%2Fv6CcaOhYcX2ESjevbi0BuagGqQPLzFPz%2FenforcHOlr4g%2F1hYeAuqDKgGhRU2VsA2ng5iuP2Fm5aOvh4cMBL&X-Amz-Signature=2c3a79c2bae9a3924e53dce8fb111013aa34f468191edad550f98cec3ed75cc8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


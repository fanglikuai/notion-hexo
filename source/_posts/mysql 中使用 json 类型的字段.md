---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RUK7COD%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDp2u5PFDH4fIxvhfwligAqLHaMiKNL7Z6ApWTyRSTo5AiEA7Kj9XSUWGYy51DUHcP4CIb7as60EbvPrj%2BDcKbHh89gq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDBwvfM3Kpf5SXm%2B%2FSCrcA5auXnkByyc%2BKIhhkulNKONLmEkzXiCy6w0Eo6zdA4XlpIBXQjE96dBuNbPT194mmK112DJOUXraip085k07KryHEY%2BG%2FqKuy%2Fpl0hf6ycaklWV8EZv%2BXRatHkZ1pN%2FTRdglKxQG6mN7k9rouJFvVWBKslJW7mpUO%2B2XK3QU4SovakmDHz6KPy63ta5hEKhayFl712v8rmxwDGXIjzxv2FxtYAYuAfdsMS2XLWT321HSU1%2FsjFuQlC2yqk6jINq0EukleaBl6TCxV3CR%2Bu%2FQt%2B%2BiRvkz%2BjSYyit3f5D3o1uzLnQG7WKX9cOJxDWt%2FZJQbeFvU%2BTjwEnx0HJYdLf3VrgyECSa7btWMJJARjmTsS0pbOkEPh8c9BJeze1QGprqQpAozkkRcc6shOhLgck3bP1R76Q6n5jI3s7LdSHZsUafjs6YP4O4gFclIWz02hD3lUsEbaCkLbBfaODP66gp0mJwAUozIR3ztsohUYZMBD3X8VuXXMxPU4bR6HZbDIy2RcknkaFU3iForpknGMU5JnZi5pNWM%2FMrkft4ZMA8u64p4oVgSdHd33KEBMhuW4%2Bgmu0hiLTTWRmwwSeJuTkmI0SaVoeOpM3gM1Vem75aq8agAj%2F4DVNGGzSWYEIfMJrhhscGOqUBMHDem01W43O%2BNzB5aX4R93hD7GphM02LjpuiVO9WIBnaLrsllM%2FIgTfkrpnZZK4KRhuwo7IaqtWt%2BE2xNtn4w8nkH3meyiwEeg%2FcMYFw47yQ9lfHU0FlrzbaWlRBMNpkocqUJUu44oeKrJ1Dga7Ejx5CwKVMGcMkQma4J4YtB9Rnoxa7zZlGuaGaa3dcjh%2BU8XROW1iaW2%2Bna2T50Y%2FhxtrWpA5l&X-Amz-Signature=553a27e360bdbb6f5bdcaabf6a0cafaa2de4064429c14c6bc05b2e6c55244a8c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


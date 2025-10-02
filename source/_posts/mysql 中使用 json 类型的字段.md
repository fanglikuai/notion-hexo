---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665ISGLUJP%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T140113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID3X7ZpbSwj%2BjGZw8l3U0iTKGr969CkSDHzM8DXxnH3KAiEA5LbQW0tD%2BCIK80m7ko%2F%2FMtrwHmPWox%2BGgtS74NQrCIYq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDJJbu2xFSnnsIP9RDyrcAzrh4%2FynrQ%2B%2BciKneLH8nY0UGpO4zt2XgnsC2RodPDf6SHb%2BGjsffk5bXVQZoXsW85KjAMI4CD8ofM5YyvM6dfYvgX4obH1QdG2uJ0ElsXWQaZIGUH%2B%2BII34Nv8egQ%2FWHUKjP6kDBYOTwavTxKpEv7jWoviySIJDFQJjT2QoDmW3dun85pewlpF5Sd4iQtAQWaJQ4QeiDmDvshGFYDQspSyCDRnciwx2VYdGnLPC5AaQts6ZX2bmdKV6gmKPIKt7vI0%2B0BEnAAnFy6OJzGhpjvswMct7XFvmHkxYysbOeXhxzVHzRNJBQFGmztjsj6JK9%2FvfdfYynSIFix0WIa1U7zWrqNsO0mi8YlpQYjowfmEmLzFIAN%2FUpWA4YXeKcm3GzE7zfzS2DBt5W9uOuG0g5FxaDWffP4LgCJluQiWHp0HfmRfkwAo8WojB5r9ZrfDPvNeJ3gEZtpB%2FSQ1iDQDQE74BEjZe%2FJaZTIRkPxrTq25udejKZYsiAgv4dEkslZbEqO0Xdv3a1lNesyqTjZ8veKxanKQXqgqnAxvbvkp0xatMVdXX8ag3S%2FyYTVpWHiDfH1mvTkQZ0HIhYO8y5cg4hJ6%2F0ewwkqN%2FOava1GUXbhMCAqxtxoG6D7w0bWtYMMvD%2BcYGOqUBUbaGqVcms8uuIwBPRYU8ylvegGPy0%2BgwunQh3LAROWx0s63egx2Baxdci6ZiMaryfNVGVhIgpFj5Mfn0vt3wTjLqalbSs0AK9X4mVlgggGNCF7E2tYoN71c%2F1sfCKK8JwEIsHt8HYRiwDktpwZMNXxe5reNmRF0B45Ej9FQGfPhBiqcA5593E9h%2F9A1B1GCDaAlst5w3zEIR53GFoBEkDNoDwvug&X-Amz-Signature=65d3a6cc31eba70f48eddb2f3a4b39891c982fc58cd0e57368a2e08d716fb9eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WU5I6XDV%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJGMEQCIB82Hvsd0A0AdtiOoyWf%2BAXgeziz88GYXFZq5RHhocMwAiAJ0urcRrp7aF3pPQVwLZ6bDDz0ERPFDhATZWu5pCwQIiqIBAi1%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FovMEpthTw%2FqMItcKtwDfIJ%2F9Zjk41%2B26UQ%2F%2BMIslwgQkYYlwCu2YjJnaaV1tSSw8%2BINl73R%2BYlJC6LXrdfLlAuhOe66B7GL92BGmqxfyB6BAClBKmAt7V9QuRnXzNzgdduPafcKZOJnDTKUMCfgXaDwGxzc1Yfef%2Bk4OmSe8Q5L7xN5uOjIc4OM%2BrB%2FYHyjQLXws0Ip%2Bte7Kd5LtUYp8b29gitj3inckCWsetJi%2BbiVHfFSfZ%2Fh5BB4NSpH6pnVOk4crBVSsnwJ23BJNWXIh5Zt0OIzyImX0u%2B5QIiJP2rNScgCNdGx%2BWKquii7NdsiHjDDEvUIxh%2FTPov2rgay%2BmeND8oNXCyu7nKtOLGMj1XRWvxcEhADEA1xvyP8tjilArSb0gmUOsX2cNHymlmxmUaCZgPo%2FGnIIa1ZK1gYrYzUNoMtPJgZqcMSfp7%2F4W%2FrQwR85Cuz0UvyBuuIdyoQs64ERphi9Rn%2ByYdOeI%2FXPLHXTKAan76KsZCNZWRK%2BFb1q%2F8B5VDqeWyWm8zovm%2FPSAh2t0Ahf5d7IoJuIaXq7H2y6Z450h0IN8DdhEi2E5A%2FJfcZuNespvCjSBhhXB1fvJugeSAaAW1mSThiJS5%2BAiLLaltmIW7ZGbf1sVUPaZPnAmkx6AoVurK%2Bs3Iw86PMxwY6pgEegSs88mSNil%2FFbtwC31sdU5ErXCFSxhMtvw5SKaJt15%2BNx0ujf9qIKwPUfAAblnperpqwMXkmfRiMiskCtTiovKE1F6kAftuhePiWFk%2FDvE44BO9UL7rHviEd9RwUQgajE2QDwp8jCTA0ne7KrC6hIxZQMfqmOWR6U3PTQSzQFvpuP5gEbH90zCHq5%2Fuaw8DUqzJUztSyzMsEg0mktDNDxUw8KrWC&X-Amz-Signature=3492eceba6b229c12ee896a41a00318074e87144bd03c183760d9d622b19811d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


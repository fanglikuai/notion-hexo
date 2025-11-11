---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTZXPQNU%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJGMEQCID6gV2aM1XhHhl6XSZWrFfRSUnDuYZAtUZmcz1zWqYaeAiA4eJrQ97YqXvAHvyOqjdO5%2F06osWhFIhAxEA%2FdgS00eyr%2FAwgaEAAaDDYzNzQyMzE4MzgwNSIMCbpcEP7xLjhpiuSSKtwDhwW%2FK13ZTqdHNG90ZgJDFEziboMPJnYxEZD0eZhGmSPTVdZWAfa2EkLe6%2BiebDTqSQgfKW7U7Yi%2FIXSolH0lDnPYgrh39swklok1L%2FUanaVbP7RluPFQ%2FX9Ywz53EHSYYQ4M9mfZxF8MMUkpUMR8n6xdAJI%2BuKRQr5lKQsHYv4HBNYlfSak0XjHoScIGlEo2KPUttsYE9MXy9fZ10CjVUTglTr7XwWbsaywx%2Fk4iuXsT%2BfOqZ3ZhJrrPXtAacNzQ4h5S1l1ck2ievrtzjZ9D7qRsG1Tg5n5Y4kjl26TDPuhtVuEaxxiTg1DvelkpBz9j3%2FocVOIxaDY%2Fln%2BAvIBbt3THY9bvcZTMq36wD06CTddkVCm%2BQn%2BY6XcUIX1u3e5EEo5N07IkCaggb%2Bl0ywB8%2F7%2Fb8%2B2HJm1MCohvTuIEjxG2ZxOQU%2Bqfl9HCVl6icPLpXh9scowKcwHECMOi8tkrgeM7ont2BNViHGQRmStTNUGlVbKZIZsdFRE8SD4bEmu0tUcNzufHIQOok0kqy0XZ10fspgq5Gx9S8uNBFnw91rkohWxve68jgFZePlYvQHwFTYefFmwuN0n9uLB1cU%2BcXPXBlf%2F6QIjxM9YQo2E%2FViYekCoSz92edkP4t20wv%2FvLyAY6pgEvFfA9EGTeozW%2FP%2F4Ojg17tIVq1XbkSvas2bhSCrG2jCwV%2FrNxxUS0d%2FXIgdWHzHe5VoMTMA8IYTdbEEMKKTq%2BU8Mi7wzc0cLVE05rLHjTzFmnYSHxEGKK7bE7OSlRctJLz4xo3DCCx%2BkeSItF8irGV30MaPIy4TXIY7ieFFagWYQfnfepEx%2F0q1JIuF1BEkV9S%2BW5QnGqJ8HPdr2IQeEdfHvSnSKw&X-Amz-Signature=80351dd0cb1a47c5ff403e29872b2dc5f4bc96e43174baba6e19800300609457&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


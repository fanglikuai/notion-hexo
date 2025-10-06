---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XOV26MXE%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA0j5wp8aHoIjO54INl492w2EPuCkWu%2F6baM4iXreUH3AiBcxII1LFrxgOTo75%2FTwWKy35n9P%2B%2Fhi%2Blph9VfD5thMSqIBAiR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMNQfPYMfv%2Bj%2Bq0TKoKtwDAw7xGOK11FLixiZKqhuLPyC7eI7M1bs3I3qO3LQBQFnY1HjDGcFHFwQ5igKaNpVY%2FhHos0HhfyO5VH9FUa0ByWL9ZDSxdnSh3o%2FMqng1NoEvptVa0qgpZXF2dvjzVp%2Fu%2FHMRRNcyFUHr0gHdHFneWF6Hl0vc6ktXlte0wQTIxyQBAWYy2kCFqPt%2BUB%2FbZ%2FXMw36LDG%2BTKWmKJivBb8ta%2FtzNZyu93KYQe1stOMRMBygt2CAvlKNk501m46CRCyLPg9CxqjmqU3t5%2FhdnOAHoCMBThX1%2FOHwkobQ9zSdeE76LojutGJYoh83lqZpOS1LWVuNyUpwUrVZS%2B7qjO4toQf2A6dPi5gY%2FWccGpAvIWzt%2Fy4%2BlfM6HDuWivlxji1ex3gwT4Hp4t0jZlYerNr8Yedv3gzKqZx53KIDT0oGmAR3s3I9AX2UaPDwyPvnqbZv%2FPseeZ%2FgzAvXLOIACIBP3wuJQJCk1JNAgZqlgH3LlKUwjpynMALSlaDOZOK3qkif%2Fb0kN70B2Cjm2p8bfkz1fi6RxOH49sjWqbV%2B%2Fx%2BxX%2BTjquKbNIGCFnjLgl%2B%2FGA9zZ70jwu%2BSVSdu74IPo5hZNKtqHjGXv2XzSgSMtKWebvH%2FnWXjTrC%2FKoV8lS9Mw99CPxwY6pgGOIoLkNkOPXBkqc42WsrtaWsMEwILsrm70SVsmYW2KPYm7fsX9iFFlYkW3CErdVpZNJ0uSlIWfITeO2%2BjPMQNUErScAQaxeH9vNWzXYyBB8mclGJA3nU3rCUHaXym9lI3HigDr0WcU%2FfOU7SiEvxl%2Fl%2FdVRAodtxpMBWg308AWpwGZJF1n6AeVXVV4Vf68r%2BQkmtIw1phPrKaDNP44X798laQ2ymby&X-Amz-Signature=cfd4bc5017b87eb31e8a1f447bcf4e96a6f1910e9808b8cb3812aef65a5d54be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


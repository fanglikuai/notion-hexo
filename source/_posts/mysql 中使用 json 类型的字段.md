---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DZ3CB3S%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQCWspmqhWkhP0%2Be0DAHatLD7D4tFebOC4o%2FL9vlZA3JyQIhALYJCY8a%2Bx1Vh1Ff2blB12q379hQzgyFr8XJqcQZLrviKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyvZzlPdE8q4VDyOcoq3ANpXmL0yicAU42d0T6SZ7FLclvfEpADU3sXZ5%2FrIZ%2BA3k6%2FpMDS9fC6VhyxqLGnEjBXK4VOsHtI2ayfIS4mwahLRWhRWVhUdRq%2FShT1N3wpJotDBfmT%2FhGMbhkHMtVqs1YpM5QzLBvWzA7xdpsSetVqXnCPQyT4CgANtYxxXXfEBFYl5exzxIX6bgKN0p%2B4FYKZKde7noa8BAirxQ2%2Fj3JCyIxkVmIr73Rq5248S7Ub28cC%2FWvXxwuj3iy3A%2B8%2FksquhlHHdb5x5jZHQfEEYm6LgUlwaxSXmwmQoX7Z89s7CNrrhEosw3IZwqPSHc1PrugHzJVUd2F11psDGhiXB8Ek6RaBIpa5TUzkqSE4wFYHUwrd0uqk2vHC0t%2BSMROyS5hWtmqRonsAELDnuKrZI%2FlG2ys9nM2ea1smIR8UYAzWsZZng1tRLYSAsWS470GycgxcUFNuDiWjgSzGn%2BOGKgK%2BQTAtfqAN7J2xaVuw5%2Bvrft8Ofo%2Fm1XkoWZwZ3xLADppPXIzrenv3RFvw9Dz0B903USD43gY3bDCLEMXDgePsaOuvNGUxMFJHxLI9v%2FbvVOS%2BXyblh4%2BMocWoQnc8Xs4hEL9KElsIAraggKx8nzhCjaadcGFlmvSE%2FQXnuzCGhYLIBjqkAdMsc%2FwVZcMKMOneLVSG4aCGfa6zHbwnRuC2c3xb1WilLa49bkLKoZAzNP%2FXsw8yV3C662aRSy%2FcQfsZXvBlJ4OPeX9eTQcPIOjfkerM9Gi24agL35Awjm3IvfqAbFlkTfuvnMEhBUBGmQlsPDER3rtcqTMgS60usfkkWPSW%2FLSxd2651CBjhhhBxmQ4beibmlfEYZU1KVQDlK1TAIRrpB9tBvlt&X-Amz-Signature=f3c1da369ea46862bee2d5ac063ae6dd8ef534a1ac26fe39281be3aaa5208bdd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


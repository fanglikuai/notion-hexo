---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXCTNPSM%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJGMEQCIAe7rmbTa8R1dVqm7%2FUECZLDTna0z65fJNfih83gPI5KAiAazKKqgwQ59FCsN6NxNrCL4YEBvKrIyGx9%2FcezvjVpByr%2FAwgPEAAaDDYzNzQyMzE4MzgwNSIMb150dH7BArtxrGMJKtwDRnJDAd6fkeLNKIkMmdS%2B%2BkuwrK5%2BD%2BpciVQydHHtivkrXT3%2BN7xRgHFlw%2FLFIpZDKVb%2BZVOjf%2BjenGBb1GZqYdDINdqfS6OKwWyge1P%2FL7A8Ntf05nAsRl%2FbeD1sh3YfI34VEFbN0RX7iUY%2Fei5HTfIanCAM1K8VbeXIcgPCVxmh%2BqLb%2F3mjTdgSetkt9dLd5MplYLWkOeyPFsR1NOuOk6T6hmN62yZpO%2FZiS4v8dYWNxLSCljtV0Ixa3plfWJKxsl6DKOYoKEocozcqGIP0ocZc5e7%2BbZUpJOQXvZXP0vz2qVbP5d3MQlrX8rYRz1vZ5bcVbxREZY9tqbd%2FyrHpDX6OiG5ZFOMUyrn1ZkngvhCMFWxL08XCAxptv0XZzRChmuzY%2BMnXRoUpGkenzAp0%2Bd0egNTxtPuLpYaWsYWwTvy3NHG5CEzP80TuYn1BdLJay8MtrL%2FirAP0c8VAHdR918sKaycl1c%2B6KKhUDbx%2BD2FGlZLQny4bbYyon0mXUgArvuVft2sWKxvl2vuRqLCwvS7DFBwwd9zOg2596VGBPwPoHtZy%2FQJWZ3n2ODHifBh3WC%2BJMj1DLNIStgoBPQupYgqeypyKZQ%2FOwtwUdYB56acLxA1lj4orTld9f9IwscfJyAY6pgEcPXYJk9OIxuQAgp0zNZVOeogyECyuA8grntTV4AfGnFH5LCPizbycRuE2Bt0bkYB9jrWkUa5egbHJ5pAuyqKBM7Sy1eL8r4b3SCW5IuO649Vz7VDJPG0Ccx0CvpXvcTXL78rzWxoCpHTjN8VVml82G7uixWQLPf7G4SGMwiiSYiLwyMqzB1isEk2bAs72nZPC6VcBAg4vt%2BFyny4g2YstJ2V0NlXa&X-Amz-Signature=3f0959cc00fa7e0f39c463cbcab8c2247f52278681809a905f188ed3a58bf500&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


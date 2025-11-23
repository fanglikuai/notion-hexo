---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WLEZLGXR%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGQZ5ljXW%2Bdky3Ep%2FUbSuugj6i651NzmfyTAg%2FAHOP8DAiEAn1sIQZVYX%2FyWi9EOzr%2BV%2Far%2BS8E58wd0pNcYYR%2BLVFkq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDLwiF7nmbwvlYgb%2F6CrcA4QmNwhr94vesCvo8eT30Fao6rKxel8wtzngpaM%2FckGKDogdtC5HsMGxx8cUtuVMDp2tpLd0Ne%2B0wylF8UF3zbmM4ak2zl7gvOfD9pCM9djMKJxSYC%2FzqHtKGeOM5NhaszVSXYSt73Zo0UFftrLw2OFEjypTuX07GkkCW4tZyh424zJE1PNJoc5qWtl61nsYHmxzc2HghqeE%2FROGMfvIZ5exTqEggbV89AnpO09yjU9qz8rUq6wIUDXQkAIArEqQVYKr6Tksu2kaQS3%2BIS6VzqbdHfHLqTZwtrWFR0HH5OPp0TvH8xpzt9gVUlKbFFG4By%2FRWUmeqpvNagdw%2FXPAgP1eGvtzwwVtxcirVXAcILYHNwATs3xkhdNyzGPWq2CLCcB2lCRVxR%2FPLyQq53xLGZL%2F7CYzQASypecJD8v5R5XyfNBim6nNedAz3UTId%2BdXk7IIYbRDfkUHZyiUpm%2FtR%2B74iv4y2HGZvVSNgb9%2B32Y5jKz4DLeSeuH9E5j2ZvcFvgxz02a3I4Pmf5zdj6lm9O%2Fh9DFxHqQPSDrbhmynhUwvK8NFZYst%2B7Yb8ST5%2Bt1NLRHZjeQPxbi6%2FEANYYSPNOBwB1vJ8io4pKstyUOy6IpoxCKNeXuEirWZnHzPMOKXi8kGOqUBajhWuxZxWKkZWtclmIRE0Z%2FufYaJZMFjsYQFJA57HM%2F6X6%2B2oshTylucjvamjviXPIiFV6aDXlaH1q1GaWQn13%2FUNmoUrmZUp6lF8GtmALCH5FAsXIWuXDW%2F8QcNWQtPiU09Sze8QA%2FV7sSOQDiJw6QOc0kx9qwKG3ZqdF0MFqEeAyecr3WZdzETlVYqWNEp6nw80uHZCvYHkQk65gANwo7HS5N0&X-Amz-Signature=6d2ecd5b0827c2dbe7e8b82ca8e79af948e58191bc8b1182053d699b6541bbec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


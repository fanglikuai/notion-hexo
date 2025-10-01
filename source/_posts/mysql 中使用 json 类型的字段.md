---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QEIMEGLZ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQCRT%2FsQTqs%2BLmV3KR6UnifXOsLHwyg0FjhDqryhxJ3I6QIhALU3mjQPq2MfWgw9q%2FjMQCQLmlX%2BWr6x8iJF1ujX4B0FKogECP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw2dt54P70fIY4fFOwq3ANfUuctkLqBSgHDMGZ4IQOYRnolI3PsKy3rud281AV4OxEzSsoSeyG7WGsSJGDlq%2BN9chRCA0SUkzDR2POWJgFRD4lx3XQvHzklvTph5wl%2FUptSql7XE77Pira9iUb62uI82mWKociou9W40Cwhuxibo7PQfrM%2BVa%2FTUYpD9WmfKyR7mtODRtV5oIjz6C7yBLWlsap03Ws4IpLF506%2B6p%2Bn8NTNNK9PLgxk3FfJDZcJHto%2Ff02iGdvbDJnYYQzE3dEXnc1N5sPw3DziLwUaZH3wceQZdgfAyG%2BVNRv2S8ORZVAh9DrHIPtp2bBmwxkGzdbPZCnEgWP26OzmgzsWSiUFGwDtvEIb0NfCIWQa0Ee7uheUoPGG6YIIIifQM09BW7iKA6laC8fgOFXPFkoo9Jy7CdQ%2FLDe3tXqWmpHyTWOY7O18uJgl%2Ff3GpIvWzG6YugurzF0wvNW0SYNbGhAy5MCLZh%2FZJZXoQA7xaVpwQlmqb%2B59mpquihbm4Ykp%2Fr6j5VA7mAgd%2B7WlBkj6f9PUG2AQak8Nqe9EZgQ6bvqhj5dNslju2X9%2FYUca5FvSXw40pPGXZPaw%2FTmrPHpXWgaZq35dDMiUctsT50hfw5WaJCoizZkItjG1Nt0GIqzZ7DDwkvPGBjqkAT3akODE9C0XRk%2FnQ2RtwzdM78go5VDZgcJ6oq83wa93%2BWvKzWnmjR4pKPYvTvmGdJGQATPacSdNPudMzu%2FhrgIiSyFBSIrZ%2FxUuvPMuXl7mAvWTYAsZXXTGW%2BKKDDRNdk7oho2UICFLpJK3mszOjydeNm1PMRVILR6z1vltqWffg1632GrxI%2FrY6KEI8JxfS6uJPkRyrnqCMMYoGqB8zRVf43TQ&X-Amz-Signature=6241bf81e3fd0638be650fda16c99ecfe9eec9696da711a5e1c1e0693031c938&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


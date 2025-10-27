---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S7KYLXRA%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T150106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFJ775pVeEzvNCcYiXKAQPMU6tBICbDg7CH0gtvssYF2AiB9aR3p3qnuXaqWhJmWnW4O3MjfMW3rVZ54PQmqde7wjiqIBAin%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMtM8IlNkhBSsw%2FflEKtwD88ZtzNlrO5EWkk1iTy3xPDmD0OGQP3xBPfybNoPUrBiRuYwQnP9HLo2w0pI%2BjzBzf8iQPuwwgr7ZZep7dFiNeg4mgL9LlfH3u0OMmU%2Bt85KmEI1i4la1YR4KrEEoFDU9FJ2NkhlG80YoZwBr5l6kF7CixF740qUu5jIdNLcRKoRGlldesj4xJ5k3F7FjGd98zAkl%2BZKIHhtZZAOa7%2FjP04REwAG177p%2BALgiEHO%2Fw6YV6VgSZinrxk6KRITU0ClWetBzzAGHjxzMXedNxzSOVvAyKKND5Z5K6Mbd2ojeOID0vB9S%2B2NWNMMMqJ6xVdTGdxEFEhEtAyAuDqX0BUkEWMzHI1HXBmujnam8pBIQNi3F%2BAvrsE9%2B5LTAC0ee75M76Bfjn0a6fUVA2oaa2V1PdN2ejgYb%2BSe6oHjKOArFKlGzPacIKncHaRGhMYVery98y0CgEpYXf%2FjL8NNUGsLmfFaBGYlledm6P8Wf6LFtmTwX3UuOJExqusXeDpJjmS3RJTQzBsJG3YWX2SiTNp%2FYizTPHMf%2BIwlJymj4kJg1TgOA59NE%2B%2Bhxd06H2QgXZDj2zS%2F55cEbjjQ7nGAkb7tgHKqqOzG8NtkG3KjEXBMRoXB8LfTuJWU6nZRX%2B8ow4%2F79xwY6pgGdBqKTtIym2OVErcwV5GSQ82YHiwbBtH7K4KIiLwx0qq6cSrzdR1M3ZbOW25WisTRVaVIxHZSISe4kEzBrJDO0upDFZgqwGnsYAJBvlxn%2B6mlJld6dJNQ%2BxgaPdaAAl76JeiP7afgY%2FSxyooOoCCLGh4zVoMXUHro8OR0duV%2F45oq8e5vphS%2B17joqSHFEyi9%2FDQdUL6EBAr4TtDa%2Bqgl1wtLixqQ%2F&X-Amz-Signature=fa43381601fdb27fd9d7f660cd0fd9935160560738c2f74b1020b8c7b41d59cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


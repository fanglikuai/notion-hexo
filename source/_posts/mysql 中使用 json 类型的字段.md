---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XUQ3BX5X%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T040044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF0wehRpAn7OS1ipa1%2FOceWuOYZb7OWDm5anxdwHgW2xAiBycxTGcmpod8%2BfSu3150sK8pE8UnXFhlduOgfCyU77PiqIBAi1%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyxKOjNlYDp2A4w0vKtwDN4l7m9BUSQ%2Bxp7qttcARBnFYqKprKaqcIxgxMJjGfIqsgAEdlz09iQsywZ8LcR1nwbTMJcA%2BvCy2%2Fq%2FXfIiRC32FWogsIakxPqS4YrQuDGxSayKPmwNAE7KwJMjWiH4mkB5BLgHKg4fhFjUVuqmP9FMp1g0BCChoG14cWZxh8CZHom%2BbKH0g3TkuLfMM7yUeZSkKo0%2Ff6Q3MxArirbZ7GaTedZLTADMlEUTz1xzUDAhpOZU6EAZfuVD4OjaWvWlIQqfXtZVhAKxY8pLQijFQvCgbhIrJiwR3ZRagHuQB8ZiV6ZthPcaKD3VDbeTbJsnyqf7NuFM6zsl4%2FC7%2FBVVfmwAJZBEPhye6rZ37DqApGOsVh2g24KwjoFFBvNg4K90ATsQAfJLzehc%2BxH7dfKUXykL5wxQBc9r716GMG%2FYl2qT2yaAKwYFb3gfI6RlnSGEz4v6uLDBo44gSMZV3rZwNZb2qmU508hzKx1a1pZMimcnaaJfkFbP3HdT96BSiQ5n0kJqifgIK7mP145PbvCQhlsy2MUdGna%2F6cIzk2tbNZ%2BYiUDNUuY487Af92nI0LevA64JvdEevyfyH39Of1H1Ejr%2Bz7YcyA5Op0L%2FHfmnPwzEEUbIhGvDfieZH5Zgw79y1yAY6pgH8efQK9pdPoVackfSIEq4uR4bErKd1JQxgHqkpOn6qmCwb2H8k6S%2Bdj0zmA8sHWLlrZ5xd%2F5Ka0X9iPUhV0jsGMH8%2F5We29AdMUsJz2Vbpz0zUThSvXJ%2F9rrApizg6tAYrPkdWYz7%2FQdJnIokm337DXqAVhVblKsrLLO6uHLbnFulq8SU03GR8twWXKVscF%2FLcX9ceCyK2oi8HtXFS5egc8FJ%2BmDZA&X-Amz-Signature=265d54a52e58a222e94167212ded323997b995b2ce89e01ada0212cb84fe45bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


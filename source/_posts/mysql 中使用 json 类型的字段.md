---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQN5Z76S%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCRJPxZPWmoQb1tNI6sOG2NtukSvGIryYKgcehcv0r4vgIhAMgiPHcR1VzS9v%2FT6%2F3DBXqLIWEWuOHOiSAaGEsd3iSOKogECJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw1nsIWnTFIyeMbdU8q3AMNpiMP7pKpZ2wm5e9iS3KTj45LPLXmUkz66LO3hTx2AkGXoOTR%2Fn1byz14shlpswsl0M3OPsqNSLP3KQ3PwlvNU8cVLHmLcXyEG5W3JFhUmT4lBhLZS9l9IYaFqoZUlMuvJTBgJ9KbbUREZ5kCYZK0gNB3kxjv0gksk76q%2BaimSlMI4kK2VAw9CqSZgRjLKSbqSYZrHxfBHSNHMB1M0wSChrAkvApzpyFWRL%2FQkEjFE8dwdbv9bpEtR8S4BETAfLb7vlIErLIJXMP4klto7S71ci%2BSnBNpVI9O3b3%2FtNmfauaNBtHy3rME49RSvREROYb%2FVfRzzM3sj7wE55pjbA0pOSXolYL7tTkHNdpZeZezuc80tFlsh39nSvlNlEym3W4YUlDlUaM6EnQ5RYt9hmZV5dLMt16RztoIo9xeVOyCvlT7Y29lMgDP%2BxXkbwQyXXsfgRgn48IzxCkNUCsQuzRn9OLPeJyAT9xYrTr7QUglrzpueCeGwYWjSi1k1IuyUGLAqbrFPN0kJM0eCMVB6r58RJFHaVZ%2B4uXvdObBnFWRJ1GNfcCbi5gUkL5cZHG4wpxb2f16NQZlyJXxIj8NbczwBbkJUC%2BPUISY%2BOvZcLQ6EhLwoHUZGBpAPNng4DDarq%2FIBjqkAUvoPA0gJskuJulESedXdOXYoUpWVnnYTjnOuxy4yCxIbGxmXKzLBHhyOOte4TvExieya9rYpS4%2BQdGHFA28%2B4IwffaXGiL7xkxLmS6kGGk4SYuXi8CkoUcNfe5u4UlwmBzFnrcm4DiCsySlvuRiQgUMubS1HQEmld5phD5YDvw%2BkTZw05yuLChPxjwHptCNkCzEmGIB8eGYd4jqs2%2BJ0HEMlLrG&X-Amz-Signature=3ec5faa8033e0553199312b72fbe4d97ebf564a9b4e666e2e7baa5b39a0d84bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


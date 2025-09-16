---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMFBIF2P%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T062457Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJGMEQCICRfd1ApFIYf20TLqkPQcgbaJ9kaUgKpcTTY4ldsfO7sAiATj%2Bgcg%2Ftyo04qyzWwnRTkFGUoTqrK4M1w19IVx3EAFiqIBAiH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BtvShJiTePrXnSJiKtwDbV9KQox%2FRYDWI%2FNwe1r9kO71vR5K8RsBySpUFSHFiL8xmEZupLST861RErpvqfQSBXwQgTKpX%2BWE4AtJzRy6p2kpbpy76wI2R0YIBBb7%2BUBiGfeFWu6PHd7qy0ANrhZYco4s3jxdp8J7LhnlMKA2QhCOmY1lOeTtg1r7CkWuAgJ1BDkxuVppHVGHRyE%2FlSL2kv5%2FSWQ9JXVoaltMKrXCPFsBbKwekvjjH3Kays%2BaRbjGluQ3VtXi0dwJI8nRrtL4rUqR1cjsIliZ2cnb3BpYS8ieZR4426qoPXBNSYCylKfg2V9NMvi6T2tAVnP5OotyOrLliH86synAduwRWhYMwT8a2%2F3cPnsqNfOv6Ih6RbpzM4TudE5zZUZXnhyK%2Fg9ywmwhkbDGNtxWPfsaaUvUm9q%2B4na4k7I0Z669vf1fih2Agwx5dyCQR3COm3Q4P%2Flf9vZ%2FHZQesmXEnLpfoMDbHdr7rr8ndqfudLwy2Rr%2FBwNVHdY6jvS0DYznj6nXjmPreFRojYG%2BeVg2HJ%2FNVt0ydjL4Ddipep0YlCDm%2Bfr8BjNAXwSdf4Ax7Q%2Bir2I%2BcCQP86mu6VFE2z%2BGts7HC8qdY2FkSLb22HAxfEe8okcYZV482BG2ncrJQ95Yx6gwifWjxgY6pgErOQ5CR6cdTTPGpcSJZcKVIQZcN%2FzHSABFp5rOUoWzZu7GtDNLXziFfqZVeuQQnB7pxWcOpg8iiO14khqQwQC1BpJ7cVxNID9OrnlRjMGgRU5SgT78OfZXCaBA3DyHZamq45oIjKabxW6DubeVCcYSj2xUh7euu3dlcWcxKQMpK9H2qBznyhfWjMTwLYir3wlh9H3mXy4u3CfsS%2F5JaT%2BJ32u%2FR5ck&X-Amz-Signature=076e609c7f172c8c303d6f8663ef6f80bdcd46e61bcd3c8218770785d0f817bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOQWMDDG%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T020052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQCQGZYemQTgo7AUw69HhRe7TQzY8rlvV9sqzK3oN0wcbQIhAO6m7yN8FHbd%2BqfQc3eMM7OSjsdV%2F68RqerYv%2BDxgcg5KogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyJw2sl1n2cdriZ5MAq3APH9TRyVqGqk1zjER6r0MuuAh6KJTJ9gXTDFCr6GLg%2Bn5X%2FyJIzzJ%2FaRuMnSA%2FqJMhtGXiSW8Edj42tKrR3gw1k24x6K5nt3AMiayQ58mjWXXHsU3BgRLQrbybFgNEIbDsbrDOxmROaohaw%2B4wQsyqpO0PzvPKPy9CV204q0GECgKuiyLKiNg1d71NcdKV%2FdYf9bYyUWo6KEdpBYIR8qkytzhWVaY%2BdzRkoqg03REl8eIR0g84BzrGlJdgv0X5D7nauGL6YPhuQ4C2pC1TNTz%2BKjWlng7PciON9iS1j1N%2FSLi3bIUnz53GkzGS76j5WZbkw46hOeq0ugHrzYyFwyKjpcl3X%2FknNCmjS7YRICfcSgm7Dku%2BqOwF40%2FhzgRVEktNzI7iTu8MWHlUEC1E0YGtZKCgKL4ZOCbyMhryXN9FfqcsRYj%2BwsDLqcgEx1rm0M21dnS7wKkfZPX4Sp7bJfZH4%2BSg2l%2BJLWcxYd5XfdwieronHnhCSvZr6rvh%2BU7IQu7F3rRxG%2B5xuG1RpRJA5WmkIlkoHP%2F1KL4tYgPgKQY4Pjxf5Z7NI5v9gRc%2BGTpyvWXJ7Z0qzo4lMNvtuztroyD6mjZGG3QUs4Kqod9q5GrKpxRt3WoBRTSiKOz2ebzCu7sTIBjqkAdXrE%2FmHVYKF6qeXZA6DLIa%2BnkQgKNXhQofnAQNAHocvsvB2Uahf%2B6digqAlS7xbBAoGuygJzb0wIYoS0OATTRpkaYSPkehUKIA1WbiErt%2BTEHAJZY%2F6EuXUxZttAiZJScb95Yt9k1oT7m0XYpf0dYsVuBW0hk5HyMvTe7WgGdHalaD1ZXILYZal%2BZI9LkD2OlJiV4%2Fla1LaKNSm4ZFlqgsEzJcv&X-Amz-Signature=fd158f2aff0ebd0578dad8033397b1a07ec8e686aa9c834f4afb0f4d06590e20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


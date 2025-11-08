---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZEGTLEO5%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIQC%2F7G1YUS6lrhEGivs0KHxAwIN9n4tzuOqxQ44Iec%2FnVQIgeV1s9GU9UV%2FDtclH0z5wbHFL28mqKBMc0uqwaOuEbgoqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOcnihv2fjTM3m13AircAzqlGfMHvlmWWD88nYVhiOQz8e%2FluewrxkVidyaJGkS4HpUQOm7Qspml3s%2FOqyTIR%2BuQjfT75NVs951GGGFDSTXa9ZbeNrH8vO7m5Fh3iMtkbvP%2B31AVVIWnKOyhwtuOWyUwXN5g04mYyPCoETc9o%2BoYVG2sDmDqlXxcjptQPBRcGOySx%2FlZDGbW0aXh0Qe0Vq2XouZwqmNQcAumncslXii5AAkJ5a9oibi329vpVB7eZ2cfcJ7iLIZZFMiqtxiOvtQ5zT4WqyOwtCHgNpNpaHOCXKfKpc25xpHzyHgfiizsL9bkbtcq6Y%2Bc0Fvy%2BYztfEEv9lUQErUfa1ZRLiuSkDQPi%2BXW7VqcgcmGDv2nQ%2BbD98eeos7U5qjCyiZWQ%2FBwwxHS%2BVnVY3vFjqCSr0F%2FMedGm5mdRsz7%2B5AauZvGqYshDT4jU%2FrYe5cUs5UxO4dPVNLrcEs694UNbYlENoB%2F7KZ0nVmr0XkxBAhGN8LlnQ1bahFev1J%2F%2FH8uuIqWyoB6OwY66%2Fz6ZsZUBRrA92orREuTvrnio%2F8WboO15ahbTmQlm8NvgSjaE8WXZPzZmg04A8HOAM8LNvmwhShoiDCCkZcFJ2dOLku%2Bc0vHzbXfiTNBsE%2Fa3eohcjDsnlZ6MNXFvsgGOqUBcLEZMspVsT9rVnoublgbmCjZhvnOtoZ4XcR3ZlMw2BVniKEUm2%2BkO%2BUouZR3KGmRzmO749Q4Ff1Lt0SLS0L7lrVsIQjs6BVLq7iySLRudBCzH%2BLijB3j2E7GiR1QtjHDDKjwNPWUJhL2yLvk0X7rd4Sr5W7dEmPJ%2Bqxq2iMBeO%2BXCS%2F21dzYJ3a8CRyhUcp2o17pdf22th9hCv4k4gsDOO6zvz8V&X-Amz-Signature=22b476f677eedc7546aee644a938a297af8c14f5b1d1f9398d915628a9bbdee5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


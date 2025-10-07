---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYUS3WBG%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJGMEQCIGHviEQsEu3xdD56ZD4oF45UCP7W147uowOSXJf%2BecIpAiAIBLot3NxQzmLfbdcqszNaUimEmZUe5Tn738u61mjaiyqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWEL7PkCy3eNkCWN0KtwD3LVYE0S1ZgdF8W2CmjYJQA1lRqX%2BHRx6p4C9Je4iAwaS2Y875N6aoUxcMkAVnx4i%2FxxmLdeZzsdjaqwFI6btJ7XwTOIUlRTG4QGMdchkagieAerVI9vFZ%2FLBmd%2ByD7j2G3oZPeyjEE%2BwGtbXD0HmcOuIdCq8V1hy0hTI8kBUXtqhegLzSoraBUZ81hCJvkGnKvj7FAjEzIqCbIDyM33vOh2iOsZLWE0ECu0IRi4O5upmNNL1HPNBYiL%2FuWLHjIvRQ6QoVK7BYLHB3NjE4mQ%2Fo0AflpQyuTpqwtw0Y4Oe71OGbHs6kld%2Fi6b53QZfvWmAR3fVtk86ELH09bhUE8BZfc4wkD6ZLYONCnOoM762BA50%2Fl%2FmsQ5sDVHFXNhFYnbLDp4Y774uIRF8Q%2B3Bl2sJbP4is5elLCPQzel2IY1J88%2FH6lFwzOPwIAteHiKl18rRpnKxIMgeit0tUULZwz9KLybf%2FQzGxcSi5167Nz9z%2F579glMtBiunoUTbC2LgmEoreEojzBZjQnzTLBXkXcbOg9QNf6LbWyo1DQ3YrZ8WhA5x96%2F3%2F9T4eQpEwepcLnERZcKEY0%2BXM3N7i66DvXAGXeF0ofW4qkwLJYZNTIePo%2FhBi4UKXBZWaLo5Z80w9rSRxwY6pgF4cCIMIzw5ke4fjOCQLTC5hLb%2FdEDlj4gQQ9CieO6PzzuceSKfeQqR9XvbNP2IL%2FvDL1EHCj0dLi1aQR2essSy3CA0hLgYKW168wjwYyAnwjQL%2FirnPQiwhKzxwDSvWvbURiQtBbXU7yuyRHDroXbNQGPx8axUXBlqEW8vreh6WRAqJuH6Wosr9wFM1%2Fcg8fanH%2F%2FZtNcxA1H5sllRgQnk7PsKkv3f&X-Amz-Signature=73a84ceaceb3fb17100f53e811677a004538b29553ddd14a8b33cd09fb994fea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


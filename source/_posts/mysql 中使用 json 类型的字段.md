---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665MCRFLFO%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T140051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJHMEUCIBRIIQN6r%2FSqng%2BbQU7F1XWnpdNTgki7w5SmUZ0fkfLBAiEApSEbwK99x8TkDi%2BjGI53apkmrMed782EaQKTzMUzYNcqiAQI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPO6SBBIKl3luQs3WircA3%2BCRJzZ%2F8Ta21EFWFPieJV0jlR47fvQzh9iY5kxDNM5i548O0EerwizVuLrpohiU3miwebhh258u21nKi0rtDWYLov4EXMAMxL0KYY7edL4wclBCjQPS0lZ6Bmh8Dkwy7gcyVmTS%2B8LY88dNHPMPeubNW0%2BlZhJwAiXoV%2BX%2B2iM%2Fl8twynfP%2BtsJ4P%2BddugwngwCyqLW9G%2BMZKB30hzrALIaaBT76aav4wooDe2IiePHepK66FzQvvSPt67zF8ilUg022X16Tlb6c5Vpj2vFsZFYNy%2BJ4wvdiW7A%2B1au%2Fj5JJPqJyFWLcLLtzjhSLF7c%2BBzljNOuC1lq3y9WvMhcqL45r0OfOwKlCY62nAhHItjThpXwIjhRWl%2BjTD8AzX7Utkx2FtcuSfzagakRU5Krm1nCxrqF1907n9eaIrBxg6XQHN82pGL7lf%2Fn1Sg%2FpT2qaBXPQrelrj4JVcKS9LAbPBmAJ6YKijIOmr51HKbghZCQDf7NQzReiGBa3zEV7yB6uOo%2B5rTJE4uaGnyRX%2FIS%2FMB6GbC%2BrIuejKSmRX0ODDlrjIh2%2By1O80tNvwjulR08MMKCqBwh6dBZLbgKY4iIPDPOwVKOiThnxPD6IqJbq1bEa%2BtL%2Fw%2BYewxoXzTMOqU98gGOqUBx6v4OLFHWHiTwsH3mwDjKJkkFQchRzzGP5pDoixg12SJkF1ZdXaIhRPVY6tRBU9TLUn7d%2Boh6C2i9cylzrjzUIYLIIIddiLR2eNxKy8p%2BuqYnKMwJryI44IalXqWjsrE58ysPa5LShhUl6C3dhxM%2B%2Ba0NjTW8XPVJBeK8qHF0srN8CMPjxSqMSf%2FdYzWDIJE3lMWJh8uOBsqBxtB9AOBEa91IL5I&X-Amz-Signature=8c672a8fee4972db10e7f92fa76bcf92be5f5f4b6d2e426f3a84c811dad0b2fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


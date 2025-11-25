---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RHA6TGSC%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD46kQ5uY3ylUtJnEBoc9Lm0Jp3KldXtF8vp7M17nB8BgIgSaRcK6y5RIRJ7nHvLEH%2FosgLb5qy%2BaVsrrLDn2YQtv4q%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDBivjQGysjdy%2BOneqircA6thQ%2FElPG51NAZmregjhn2T6D%2Bl2gYpYFy1ri1yAYAkgLlKaMzl%2FIpWOPF6HoJNc9kb1qdASNBFdMl%2BOzJv9oK6iZebk7Jx7VyuoaBpiTHknc6hjfDSoMaLW489oMaEkefg99hZc6eZgH2qVfcLqsAJYceTVWIVOvKkhHN92wSEUF3IpzYYrfHr1NZzBpMHTLLw5dZiK7XCtHueKH4Q0BuwSOMHDZ0dc5zd5Tqi6lRGtwfabnjNWQpYeaItK8bQ1J3ZjInr1rosmCCViX9c7vr283GOhZjyEFUIAyX6ILMcQ3eVs8DlRrSEJXanDcU2milNaWkjzJ4Wgh7gyWjCztaSId3xmh5LoySBhTlSU%2FL36QNLL1W1sDmgO8W%2BJxeIviO0wNr6JlccZkoFsp%2BMnvA5yfJjg8K8pDmus2tL9HWmUo7M5mQxu8ZEa760VX4FGF649rHqJ0gEzbD0Xoer3aHkttDh7D6CN%2FEcvwEqJhMqUdrsw5lHsDFkaYwlTg9iI8BAigvLYqoxRFUdONlOAO9qNn0YrXdKzlrNR7IbWnxBsJ0izWepaPIia2uUHNS50IUsCzoRU7BRh3tSFLWTuEQuuQ0lNl7bdF2ZQtbHkjSDFK%2FJgCAeGJ4hYxuwMMmClckGOqUB82GqkG4uHppGvOFPclh%2F0KOxu0kaLMEu6n0ifnu4PJZgcOA%2BuLfBuqIbbHmvT2DB9HXfgC2AzqIdGKmaWkw9YUgifk6mIykPlioXXPI0G17Q0NFJlDVbTWaaH4wljpGEECHJAb2U8D4qdHu4jAIhHYOBtIhg4Ps7txQragsVhCMw1kl%2BTfNE%2FwQ2nT6wiSHJmiZECFWdDDBiNFL0ONBYsHQM4iT7&X-Amz-Signature=c8d80b7675ef6df74379e5d1234d47ac1496a9da749aabc8e25637e602deaeb2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


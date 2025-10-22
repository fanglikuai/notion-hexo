---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TD7I3COD%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJHMEUCIAXb9hd0Wr6KhClvz6kaD7A2VIs5D4GOZlAx3zJWo7hQAiEA4nQ0inw2Nh455Pl0eQ%2FiI1dxk%2FX4NyE5isUsnE%2B5tBcq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDCqQtq8IVPg37W%2FHzCrcA2tmUKIP9uCkofkppnjnihkTzv2alZ00VdEDK2ciUvGZLofZtbjA6oC7%2F5pYdNRFxzcI5vsgupziecbvgUytmCiKcZoMe2xaK7SQ3OCdu%2B6C8sdvTsPG7CipFeIpBPvrfr2kbwhda2i3vfhJy9C27pcKOhrLhahi%2Ffu2i1xwVwYAh8tGQRjnV24NwxWNAg5A%2FwFBCsWA8EbRAPS8i5y0v0qlwt07l5sL98ndGxvVAGGf7yULgo%2Fg%2FrAxt9k7ZKLhay3VqPvXjFX6d22xG8%2F64n4cJvbywvhnrC%2BTysCXDCu8srn0S90SwEoJd0rKH6s4arcnwMhkI%2B9868L%2BgX6uUz20zcwgTy5ZW9aaWw1IXdAyi2Kd7iLiDQ%2Fr7gbw7OwEpiqtbuEbrQc1yoYKqIdyD3Q6RztljgzzfimQXgb2I012lHCcSQU5N3QSYoOn5hXWdRheyWRZqewIDxZogy9T9hyXpIE7r9BW8sTrDdCPgEyztAIyO4arSPrs%2FqEkK0Fskg5wU8Zv241z6pHdT1nX2kSWKgqPBnPwO76V0nxMwTyLbd85zO5jTTTlKQe30FhHBfWMFxuRJoxnINqHkIbxyRC4CjzSMy%2Bp7zBBcH17VuosQL6IQmNcm1MQ%2Bz5oMPf45McGOqUBoYDcAyt8vBtpDnufydpgBmf06iDKkJ%2BwG4vDzg3IQsVK4XPOyNzE8ENSrSxAQXSnHzl0klO6S%2Ft6GiLN53WJr3uwgsVnIU3qVIF0tcRnTwxPseauWCJqSzIGlcYQXAEF1X7kocz%2FTlnPQqUv4p5l3C39%2BdpZ%2FYkuKsZfMB9O9U%2FvkhO2dfPRWVeWihLEHb2Qas3PAiYvpwtuhOCkpv84Y7wHt9Wp&X-Amz-Signature=5bd5c807f57816982c16eba63de857818a1af93b4794dc60a2eaa305473df25b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


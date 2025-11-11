---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZWK2PPR%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJGMEQCICKojMqtLg8WNCjTPf%2B6x%2BfA7XP9mXYsV9%2FJ2vGXrmETAiBjALNTPRVbWeHVoSFwOhOMGHCNtdOJWF%2BZhVc%2FKpMeSSr%2FAwgUEAAaDDYzNzQyMzE4MzgwNSIMUzQapnTiAud9%2Fk49KtwDvt9F90w4IrEPUwm0vPTmx4Y11dtHJsYJSFRCG0DAeeDwCUn0UTJI%2F5IqPRSbJYt%2BOZEBIivCa3iwEbBETBxaS4NL0KO%2BpMg2zD14ZWq4Yah3EHJGqBvcuJ84a%2B4BnlmVkNBNQxH4aVzRFEe8UvGrcp6QITMd3GRC2mIseP1g7Yi9F3ZFdzySylRoqwXiL0tvNyPYmrv7qijAxyOXfKmqwIKrxOqC2GcZCg0KF2W%2FAzQEbhyi2tjWlD505E4LEcEgU0ccLRop0yLqil21YKVtb5zu1o%2Bt%2FTCzh%2BdsCBpOxR%2FwROOq3jsN2Urxjg8QeaGA3eyuHPb%2F5TRZwzHaWk7bF4fLCHuDuIteqFvf9UdjUhSdqx%2BN9KgTLAlUtw%2BEVnJ2PMXfvx%2BpAWD%2F1EgupBmr8VYbXH46EiGx%2BxmDI0LNtgE3JDjdUH3pO6f9WYYMKSZDuhCmAuzyjdFmMBDG7ZCOYAyQXWu8TJJbPaKGhTfj9TrHrmev9J86s1GvIxhBHPMozgaCRo2x71nepJpYco7OoRWwVgot6rgZqtpZZpMfMkLcZkjnAZ02%2FrEnkk3WypAmZ3gIMia0q510w7fQC1gd5HqT7m45aM7Rby4gj9g2%2FQG0ehg3HeOUdfqjG80w8cfKyAY6pgHYknp4Sm8lHJ6yk4XWRupNFxIwLH7O596cIWHATjjVmiQbFoGXxY2m5TPlMORYFzJZDhZAZc4xl4x8apye96O7EaWiOLAZe48QIiRYn6jeHGsH7KErSnOgCvVbq%2BF1g9caRuQp9rORvIv59sIk0A6SJCd6x1Zaoy05t4YJDVZyMcKXLyKRXw2dPsn35whiedKn9MuGJwmSXaTcmpWB8iviTANjmU7r&X-Amz-Signature=cdd8687fca24146c62424a509aa59e2a2ae18a79890cbfbab3c8594b9b8c2054&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666AVW4OHE%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T070100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQDv2%2Fyek8DON%2BN6J5%2Fz1bXEKu1V7OQ5UOPXSt56VyIGmwIgCf1wWVfwc%2F%2FxAdkaYFIVJ9C9pLTOubArYt309khUoKEqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOSVhYxWJU6q1Fz5FircA53hvJm0RZ%2Fa%2FUT3JCPLtfhapYLjalOsqDKeB8FwrD6MgUmgbls3Ng5MHaAdYALm4nrI%2BNjRRzDS4q7KDYlrE1Q9hmbGJXxEdQrZvsT5tyLwTSPCeSqa%2FlNxv%2B6LnzHQ98RJ8%2B0rbuAjd5ipgc7evZWxJG8owJ83s5A0LGA7snxIHJ7sCqrcKinJ6P2O6CLHvuyjPYBBwp295zOtpGZaC7BY9YWPTYTMZlQGitp9fxAbnIqAn2UeEDFkwbKhM4Ej37iAKlWGW8xtXFLpTnY6v2W7z0HRlIhtTQlflmSIFuQqPpIak%2F%2B%2Fe0xu1YP0XElr9v7NOdEA6TMCglYZUVWVXUTYiM7Pmo9FMDTzIT7KGQCsaFzEsbVsxbkEkJB%2FciVgXOaRDjjwLwH%2B02cEFXKN7mAXYtcLmj3gOC%2BAVZm95Ok16CLzu3vXVFwU0kyPQTXvxwVze93JtXxTNCOx%2B8z37H6cVWEVtF4s1zXeJAGOhNaQtWvGAi8nDlXmyU0tFXKvrp2tQilTJW%2BlO%2F9tac86nin6Rh8mLNbvf4Bmeb2h0fvpCZGavhNrkoou1beP%2FxSvEueKCMCRtm2sL9WkA6yTsCmRRLlvKWc2jONOOFW%2FSMnr1i90W7rELFxO3gnRMIDaoscGOqUBR7ZygwP3Alq1FacTXD8%2BzRFPRP28ahA6PqBCQjiIQMAN8yKCgJ1Gk523onbhd3GaR0Qw18pNGbr9zo9kRh2BfuJi5gNsLvUyDM%2B8Hmb4860BzYBMHlIbCZRzUtxFp4cz64umEtqfZks9iFHsrruqjnQ%2FMZBFy0brQhjdsKo8KZo%2BgHK2aT5tgJrzGhnmbF8Bg5c6jWgI8wO55QX74y7aW4niBbhs&X-Amz-Signature=ecc63a1a5518f615260d0f2ae17e27c7c596f3150d4c6ddce7f925416173741c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


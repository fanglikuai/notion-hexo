---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RPZQHXSX%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQDbmyMJRRhkSGr2eCy%2B1ytgBF%2Bd5dUK86lQzxEBWdzELQIgf7sj19VPtPQkfGXP3abwJyqGRH2n60IPSg%2F%2BjAPhaGQqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLK%2BBycQEqORIhsPLyrcA0b5GmOIFTX5iYCs1VCUNZFALbVoV2QeF6YXkLbpGeXHWR8LcxwaLmfRY2ohvHSBuxTlA2kqNmKjcRMyr4%2FRUeS008D7YGKJQfXJ9YabfyMgLJsuHCZGa0mq10%2BtwwyStH0hjhA%2Fohu8XK6KmHCV5aLMT3XM%2F8SddrkOaIgTFPYZWsiuSzDvpcyIRxh%2FSoIuLaIKwM5NdeDZ6f%2BN3k9UtOuhoAgTEzcrajH45h3ZpsO0vI9mtezI6lhWtwcIRTzZq%2BjQPLsXwE01E33s3ErPHV4Z9kDextxyrGKM2srM6zRpTg7xSENsrtJm84wwFRhuGYcfmEvSBqk2IlKBfPJX7b63O475FmwiYqi68ETz3R%2BIqlncGEInk%2BDqeMcqtAjtHABZa9D%2BwtfVifB9JdV2dQu6rq0OgyNoXm86VBsrRKxLwRazNJbQq3VV4PG5h%2FtAb0nJwQ8Aa5mirgLtanLi%2FHmwDydO5m5WuUXlks%2B8wMOcrbbw4Ftxb0EN5nAAMaNgET24QhtO3B%2FRfhSjnS7hy7eGPco%2BskHJQ41KRh5e2WrNwLCyursZX6a3y2WsqnO7OFbeYvcCPdBY6doP8jRlo64zOg06gn%2BPpRzaOPo6%2FVeAZMO6aUgWY%2FOsej5MMNXq8cYGOqUB0jip%2B62j4YmLRZYszSZxTBFAjyC6SUPy9iHfdvGUFHXc%2BfOJOxAm5tLRaqe3PfSx9MYaRtd%2Fo9ZRAUPo4fOi%2Fw8NVJHB3hZuit%2Fz3V4ZKRlc8PGH0%2B96ctrsFSHDAYZNg99ezHG6D0LKe31hymz%2Bo1vjcMO2nU55vc7VdAeZuNP8MTJMsaKFJBPCSYlCmisKp2P%2BflnpdfGSCtT%2FJLM1m4D6EYqD&X-Amz-Signature=31309fe0075dff87dde4959e2c9bf33ad966d16843a5e9061702d5811458e2be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CTDWRFU%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHArOKYJgB8noV1fLGKTHpETAL%2F20H2ojYN4EWHlEAvGAiArgnehEaq6osthLAVr%2F1CqNiTAch3Au0NqB0PHGoI0%2Fyr%2FAwhoEAAaDDYzNzQyMzE4MzgwNSIMaVTfrz%2BcuTeLUA9tKtwDhk00AmQJ2XE1TdFrebWpdbkEBZzarr0dBz7Ga3oWuqd8nVACGzkYhvkwIdJZamtP6rbWr6KnJKMgJa%2B4ccaqAwb%2BY%2FhXDE7OVyYLHCtE9S%2BgNJjohQjl3nH1Inp91zWT5vNzBUxMgMqzKDQr3gAcriuP5slEzJnyWXX5dp1fpldd9f2agb4CAnISWVE89PzupNYAPkNlufaEo7ggm0l71Zkk4M05ov%2FKlFZWbFjXcgvSZQ7vFkvRWhM1Lmm4BJuPFwGEuy0%2FMdzwyBs52cAtTRAWM9VrysLSg4iSlGBhvYQA8uaXr8fwmHBzGBuKhXiZRjHOFp9CGUfes9EKj9DJibC5HCNxp85sKUdKQhvYayXnxNrSI%2B4IUMY0sbQ5T2xkS2R11xnbYP2apJwYlvSc8zuLEizoMXIEjawg1vgrvLZFn5Jza6cxV8zHFaOV1JS2EHOWi1fY0GauMzShoIaRFrHdeXCWierX%2F5fsjdrVVjktheyi5UehVNbYuuQPMgN047TlV8%2BdEAVjZFiovu%2F52PTIb%2BO61nDqRySL%2BozxzId%2Bog6BMn%2B1TcmjFwnEbzKe9xtHpKH0Xpx%2FE48nDz7gsQ32NbTTGmvIZ%2BL1H6RCAgQuhYzhkVulZs0oGF4wnujRxgY6pgHJ0igsJ49t8Gk34WDCU82isTzOhcJ28s%2BWtjBJhn2UE%2FfEITiTwHwxSGonttFfL8zwXVe8Ukz6KP4nq6pqYN8ctQffguR0%2BV%2BU17EtCyS%2BBjggACY6eF05H0dMYUeoiiT5BGz1%2BQxnw70cORcthecDlQXYmyt%2BrZa7Nw%2FITCF0dkNojj2gF6ZpU1zPET4jpvtiOjWY9X4peh5DauWP5slkvGIVMZAk&X-Amz-Signature=524d449bcdb1d16ffc5bbd1ca892f512f1d73f779bd1b1677b91e0dfd070057f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


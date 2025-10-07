---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46635APQLNF%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJGMEQCIDYwVQZKcvNRcoMHrdWb2nRPZvKPd9DIwsEuivbeQ2RnAiBwGYgPpdKeD4vi4M7qtewgL%2Bg5sriKdhmY41vgoSiOFyqIBAig%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbZEE8mmAl7UfVxi2KtwDq1FPJnXAYybmfXLbwH2ovDcnZi8qnOfHhSTEznmquNDZ5qWWsUjl82vYARzBBDTJJBqXC6u8cApwBn%2BWQuGbNNqRxJLhbLj51a05SuW6h5APZ4gN%2BMWWnzTZGE0D%2BZja46Dq3E4FgJIReMhSeYSDhExUEMjXQXmNH7%2BqlVkJ9v19VjI7bK3ApLUmwWBEwYEYVgRtAdLLHbT%2B9XGL4mvEiv0KEEoi1VAyF2llZy4MmeLTyqLoSm89rqJ08whN0aEQrEJcC%2BlukHGD4rMKuj%2BelM0OrrWkRYuBtmhLLEsRZZgerDtApA5arxSBsIW4Ky9zxxATbWVIsMSbUJVTBRjiI2qRMjs9fpE1CeJVSsoO8T16hQGtm6mqhHEe9No%2BirG4aLWIdBtbC8RWjpa%2B6bE%2FLbp%2BphP%2BSu5kyrUZ8dOVKrhdyQyIftCVB90rkta0isggjI7gI61HUjMucJzKghd9oJUPN30MGF8Bdf693JPE6XWlLYyKW7w3mwDO7Y5a9N10xJTCG%2B5zvfH2drnX2XJJpAFlVFRJBIUnhk%2BZyV8iRAGaz2HsW1v5M1DvDOvfsTV9MqH0GuKedGWyHdc%2Fw2O%2F1KDj%2FDxvDq3nsDUKtiB9e59NCNjduSHUZzIEt08w7PCSxwY6pgGhm8OAdY5FljLIh%2BCEyC5R5bJnwylCkPkrWqjILYauq5j00PBgDXYJ8%2FJiEDrUVPggPnfzy2v1Syv0c%2BnkqDmfSSNE81fIGaYkoTfwk3Bq7p%2BabZNyZNO5WG2vLhaSYJAi5WZakgfqVkfN0J%2B%2BOnn7%2F7WSibSvvqvY497H1i3v%2B%2FwXX5sxak%2BZhU6aw4cQfMK0rPQtNk3L22o3nvNKiCkmPCBD8On2&X-Amz-Signature=c6d5cd11c43f8ef52c1b204fc7582d972875d2ba8dc48e571b25e4bb01ed177b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


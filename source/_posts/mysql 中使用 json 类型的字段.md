---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S2HOQBTT%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCoVKojA0Oe9DAFX42X%2FS9uRs1A0QsjcyZ5kO43b9PPZgIgVC11VNitWbRi4QTZ03tyitghxRJJ0ZyxlKvSh8FDc3gq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDATBwefinMClWa5r%2FyrcA%2FAyVGOyr7VI38JjLdP8J%2BeaNkjhPPjHgsVE0TjrOPvGoIDRaTVRt6KSRFwajkqjcsL7Y7qBwSnQYgo%2Bsp3OLM0DlLjYKzbPU6BQyyDBrDi3E8LzLl2tLb1gJrClh2o4bAvMIMTq0R10UtZlNr0rP0KOEVFVHPkW3TlTtoDz5NMZZ%2BrvbyPoynXLtVR8nXeTQTYGp5KnlO5Uyt98bz0oAwi5lI8LFxsKMopfP10hIjf7tN0ZSaHhbFnBCZgC1QNYTZBA4WyrL%2Bm0GyLvhZZRqSt3pv%2FWRhG7a8zpBYbv9ELjF1Xn0zIcumHpD8oG0RqnUSWOxv4RKS%2FBn2lx0xjJr%2FbbJD5QbMNbApddUlzzKp3Dy5D0avujXqjRbb3NsJyRXepQaIWd8OtdIrzpXziR2T9YYmQpPD8QIFJZqb85cu9ntPFMo9RoR5LShxu6gulzuq84bVmrdEo9h7mhXiVhlkBa9N3l5lcVitybNoM%2BQQpVpxyD9DNb6c1WA54gw9ssTbVgq0TRSeqpBav%2BGCTD32mDTIVov9K%2B2DFlKpYnULTuWIux4HaIsbH3hYlPyB81VVDc11dHVTuvzjaevnalYgJfFJ6efK829VG41ZVQvcR%2FPbGkJTSTgh%2Bdk%2BsIMP6ghskGOqUBbNmI8Ud%2F9nIK7ohMKaKZgOSzV84J4ARNEztyYj7U8GAxpMcG9C0LJWnSuZ8UXCbBRvPHdKmR71o54RM7D%2FydCQruvUap4TpQ5dlhujcraui%2F5NYQ6qwHb1IfhZsQtkanI5uNdC1tTEJTy9A8eTnZn8lPhmPB1ZjW8M0EM9UnC8QbA7RHPfoYUK%2BaPwyylz3ocDUIbSvnIj4KdGSNkQt%2B6RSaWoPi&X-Amz-Signature=521a763dcef0cf4295ca96243a07a24a27038152dcca87ad683166ed9e4fb509&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


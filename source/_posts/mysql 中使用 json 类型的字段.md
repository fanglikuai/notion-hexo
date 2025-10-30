---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJATL76J%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T060036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC4aCXVzLXdlc3QtMiJHMEUCIF7SN%2F%2FTOGhQrFhSPU6f%2F%2BV14kdegzCCVbqxCyq%2Fm02cAiEAzpNoXy4SVkybzLOUlplsNos4WipbAfVOYWa2KvfwapMqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEdFSuInq1rxyCJ1AyrcA0cAxKIATFsaQVKltVb1uLyObCzckGmgHBzv7VDSc8fdR8nEAJGrqDaUbKZQv7L88IjMulT%2BdHHiyyzEluI6xODQIPg%2FLmNTyQHcM9w2E8p7QMkUGt4L%2BihHs1QTmqSu1UXjV3AQoAspjVBtjBCaMmrfNSad32AYTU957A87Xv81%2FojgcY8zCUEQ5GQaxlE2DCi9YA0njGBIi0NVK62cmBOwZLffqQUVBT%2B3q8utxcWbqUnmIva99vQPvUxaUfGcmBKhM0M2vBan9D4%2BctsyreI4aK6jO%2FBSacGZLmhl4S8KnKrjgqEeAzwaSdMwzMDc7jexgopMOqXv%2FjvLmy%2B3NpdiwGQ%2F95S6qiw1keIDhSF8mxWCFQqcc9yhSSnEB0lNXO7UfAn0vHvI9KYrHLcyl04tnkdUc6WIfleuy6ekHcxWJ8NZKVsMvPypv70saQ9tr%2Faf7KP5g7cI1X%2B6B8%2FQd3hm6MvM2HFBC4IvMgAybdPLz4JOwubOtEEr9RQgmaTpemPNR5BMtxERwuA0zpJB6buFdqnQ6zo2nBxLphGXKNWYFMxu2n7YriYKRfzgSt81Kc7VSSUeRn4LkAX558xUjhGfQNh2%2FblDgzGh67HiTGl8E8JeTbZJgF%2Fc9JggMN7vi8gGOqUBOgm0NHsYsbju21RVrJHQRE23E6xFKLjO%2BxHts2rgL1KJash%2BmTqcpEAfnCVHdwbbaz75THxFFsH3xryGp4kWqaW1hwUd3X56t3NpzeHeAmSFXsqhbUglK6iwD1MnG3MQg5bacuIv7d51u0Gb8f6G6XnZXdTCaTtn9ODudCruMTunjvDYIRuQrYlB%2FB5tjEXUtsC%2BLV6ULQpCR5uXt8Q9LG%2Fyp1Qj&X-Amz-Signature=a346ef6677bd02fb0bd1adeaaf991638606f09ae485f71c70403bc69d2c7ef20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


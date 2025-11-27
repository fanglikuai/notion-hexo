---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDKPSLII%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T010038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICzwbPfHJMbgexgiBJyFvrtdAeadPlvmP8sQYfeWjUaVAiBfg17RwMEgZvEr%2FZHq7NxAyRRfOQ9ObsugsMg7E07QiCqIBAiS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjkk42f8LP18mC5fYKtwDhtsYSe5%2BI7777XQMRQPQJ4UaTYLDV6DvuNvHnYarROuGe2uI2Elk%2F%2Bq4smb3NatSq8mRfvKwmfwk0BTFSjygyT4lGf1IRETXwmI899m5K8Y7vcFFRpnt%2BGCRdu5hz6%2FaM8p1E9KamjU1xVyKiRIgG1nakEuPPJIQkEzBdX%2F3PsU2fIE1omTtFw%2FyZtwcLvXlhBB5vM%2BELJH5%2BWOBqFUdgUMASx7AJgI301IsO4YhwFemBzG7bzW3QrfGhOTacjr2HIQFJrecYLf7VR2MzY0tXhyISYaJHytoiyjs%2FvsGHY0iCiKQk4lD9iTtqIPucjAkAouUD%2FQTelWR%2FKp3xi7GaZZjBQH8PZ9VHJM%2BW%2FdqHDeqo75%2FWS1K9VBN4tJRR5%2BcTxfxUEfPvWQHJaX1vsgWNrHMfIIGHwEiVSks7tU0C68GhwAwBgwP1ouH8Y9u1XI0ci1cEAIVxqbArKtguYHUVKgfcWrU3i3AJkYEvdHO1AlFZ8MIPfMj0HAtxciKNGbWXng03mHz%2BmK7Qi7HD4SURXij%2BVSiIA4CaLxZ0RBa1abSlIeiYCWfRZsiQMW8azcv%2B8fSpqhjd5U%2BkQ9I0vujSiJjawF9lEHwGTEl1lHqOHOfJDGHfmtTqSNar2UwkLmeyQY6pgGsaVZ0C%2B8XdwDV%2BF4qNcu1fVwd5YzAl5130NefHEeKCf57CHHo0onNv95vXyFTzBS3ueTCQBwwb%2BVfkQ%2FlBNFury6QXkmD0t5ECbWSrtcM26HJxE5M39PfyhnfMO8z54MdySKkG8sd0jLpAPBRB67oKKNsio4aULO%2BnrYZa489%2BUe%2BguPstyfTNXkQxCGT9LVO9VmTbxhDP3IK%2BGRG7LZQVGeitzG7&X-Amz-Signature=a07c07690630aa705a551c4a5a63db137b3c457acdf86cc13fd9fe836182dbd1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


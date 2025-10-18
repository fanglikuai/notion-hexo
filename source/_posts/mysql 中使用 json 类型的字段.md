---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667TBHOQJ%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T140100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJGMEQCIB%2F2mNWTj38EaVSjKbdvPqIbY18qLQirr9%2B427U7DILgAiBiihVJqBqAT7G1ZieRKfwPyYnvJgksze2Ax%2Be7fp1kdyqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXFWQ%2FQjqGh9VnFiNKtwDFEvmJ3Rxaw013suhta79DOOC1IXkaLpNId4AaYa9jxY4jiZXYWQbQLRztZGSYX7vgWX4BY5eZj%2FW65xX7sYzM8SXA6XUG8OGKyCbnFArpiMKsWmZ5CoflJ6i27t1lNGmoTsoe70Sab%2BpUc52FEaSkL7IXXWvf3AZN5Da2dHfEcta60kMWzr00vRO2bFBrbhZEeK8nxywPJsGPtJmTHeXZyPqFYztHVtay47bHuNbkXTZGvANZoAXwiwlgcHZAxZkjKL5aJkrggAZTsAGpBQE%2BDtXX8WfjLAWDdHxBhk6BJWXKMHWdTZKyqk9GypLboFXO3VSslro3ptB%2FRO0qOutV2fOtxuWLqp1Xlhc6hxNd6gUSB12Tq8uFgGhkHTr%2FcVgOZF2AVNd7arKqTO4Oj3vg1Sau4dc8TS3ER6kEmzn2XBLReYHUu%2B2aCwTjvV3EIkgNC1UvTO2P8%2FXKJfV2yb%2BkBQWbjo3YVXFvWm%2Fwn%2F5nGoTtn3BFqocHs%2BCuRiv32i1%2FFWGCHNo2UEM%2BnEBerj7Ev7nxaHh%2F5IWlPhuMdf9edXoclXJD7fI8gza29fOQNfFhGnKFZKVMeCisVeD%2B%2FtOn8lEqcOaMhKz4kCuevySaHUtqoIbzBswYP63Nv8wsofOxwY6pgGuUIJnnl43lAJDrJl%2Ff5CB2I%2FeTtdczNRFcrsKwxRmxW2qpiyluSKqDNwXcckAYBFLr5BUeRuLebj9SBG%2FNhVqscjiqr1JWk7v5cDpsI81r4f424M2p%2FzYN0Zd9v0WOUgx5AS1fjbgEUlqV2fSxTkAWPzRglwvn8XkonTTBwqX3zdPIuErwRBmojpW0aB2f%2F94XlbJwIs33g8rvTiihE4tRFimbQNQ&X-Amz-Signature=1012105e8c9dea991d932ec4521a778eac59ecd2867cc789f2b913e4e576d82c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


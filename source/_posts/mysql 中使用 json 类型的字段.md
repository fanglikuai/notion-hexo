---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZV3OIQ7%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCCH4Jm%2BnO8GG%2FBdYGcOqP157qxASYIPnS%2FioeLlMVdlAIgD%2BO5G8kq3ALZFyFWcqyzEfKVbs1YXC6NFYCI4qc1Kcgq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDHi3jmaZkzjt6C7xRSrcA13aeMft2vE91%2FyMtqd%2B6GHpLu06OKxUroVk%2FZD63UQJqCw%2FOpXPT6oPHSqh4nhqRg1DXDyg%2BaNlkhLs60%2BNgY2IVJRXIrDthdyAGCmS7oaJLMQXvxEl7cmev0aQCdvFPXlQb0lFn9pYrSDJnbbgeRwFnZy0e0RrS8n8sihX8dwGobgki7HZ%2B7h8wxB4itQL5HPU75KxX2guIId4HoBLn4BsmIdpnnFM%2BII5wkPlklhQSZsKXY6xWnjUoRYJsf4xDahGXGIOkdSfxcgU21LBxrQOUm1OLBQIt%2B2l3QhlO%2FXtiSLJK8w1rN00LUxPFjnFMLcr6ZJbLgpFISm8J0CtezKZuBTN2aS4FDdFzKEgdcIT8KAUkdnp1asH4MFDGoSnHqCBjErdRAO6iCap4YCuHSL%2F7rX4RNtdRSHXFF6eqvPdQgdpibBXwj10nPQ2NocoBvmBDbCmy0hVDd7E7CMA8l23v9KKrM%2Feutwypta8RveDrsh0Ii5pJJ3pRazjWDJvQno29CY%2BLwaSO2DTde03Xe8%2BzUTT%2BXIbJrsmoVULrEaxlFSGi4A9K5wfC%2BdnjJ5vOATgR%2B3VIluF3XnvHPw6fAkh76106E6Tt5eE1YRQN8ynRqAY7rrwcSUT3VD3MI26lckGOqUBeIvGnhrI3MkocKoKQMEHR4Izd7xi56cjKbIG3pB8QQg0hmMihBk39qzH6NVNBD5IzfZXrV6P7fy9nzeS8zj%2FUR1fWaw9fzscnJ33lflf5PhWZ6HQ8YHbvUC%2B3xIKCtdV1N4IbAbqkvTSDaobJ7tQTXnlfkFNmUKNerUE7cMZP6Y3jpXVAVwKh3F%2FjeJXTYNEB%2FYsLadA9NU2sdkmGm53ZJqi%2F0dr&X-Amz-Signature=ef1b6a7428198a425e2bd1a63e3ae0d0c7ac5485e1deabffc14e319cd05d3e77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


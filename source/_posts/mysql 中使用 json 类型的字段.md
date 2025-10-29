---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZF7GQOHL%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCujlERI2c2MR2oU%2BtByKrtQm%2Bgo0DCjqqnKUKFIJ0ekQIhAKcGMWjbbFXBXDq7VVw%2BW5JME2RhP0Z1uYfPckAdJnayKogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwtS7kKMmQT87m%2BwwAq3AP1qp2GKgdjVvUA3EZjsIfEaom4s%2BRWcWEENvlqCQQeWcdVu%2B4KK3FEIqwO8aXDfa%2F8AWI%2FNn7gKnnhXIGAF%2FR4iTQDrxo9VSeb%2Btm9hN9nY1Y6C3xFpQ2gmJYJBznLWSPISSQMj9f0uUeDTmisvZLKXxOIOCtwX7eqWLUdBZDQTRxVIL6e9nPfFwLCzVZppTjR3zBKeMyBYtHr%2BIbhAhJdYJfQ2b1Ty74Z7XUBzQOjyEPfkcIYsoDXhTeTLtV25cj4DAdK5qUzrymL7GDEyrAyO%2FoSDtGl%2BxGhFrOQiAk0ffhM9cJrA4SrQXmLwGqpP%2FTbDculooIS8%2BemQEbhevfjzs0%2FsOcTFlUQQrVcTs4xL4EID5WJ9Agetlp2fSM1tWmY0NRMTLH%2FK0sWuX8jo7YSKMYEzelQH%2BVK2K8%2B5DVJMNLWhDB%2BH8dTEnFJrM8WV5ocQeKyhTVUT%2BKYaLDq%2B0uD%2FhSHHs0g3j4mPNCN8GK1zW5LP9KcKQKv6sOz9u3xb2FWALr7fmc%2Bb1NtNcZjSYOlQq3Z3DSmEfaf8icyH5nP%2BYND1kityOiLGkjD55OZQV7UdmVSoTHPWgqhgtJ6dFhcZEJmhNFSBZy0KHfmc6gBZm9093bSAUVM3CbiLDC0yYfIBjqkAc7UKs%2FckL%2BmP%2FdUblYceMktpIFA5ZnTP%2FSOzc1N26miWpOFWQ%2ByO0jFEd2gpYiCMuLksFgZt4P7Q3lqf%2B6ME3xGP49r813PLyuWRVGMQh551GJJnvKMA7fDrZ03zMRDug2sF4GFVUP%2Bby7YHIYzKnS4ymPJXEwYmvNURBb%2B%2F%2BmDe7LuLLE18xU4twUYFQdImbds4xS66n6cpNlA%2BE4BGY0wjGE0&X-Amz-Signature=71e93488396dd51cd0a05b81cbad6541eda1eb08d03af735ebed8ce31eee36b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


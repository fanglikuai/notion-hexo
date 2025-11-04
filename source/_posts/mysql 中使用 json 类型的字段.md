---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T2Q3E57D%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCWn2mcYNNYeF8HPJBvgpcCgcMBGTz4BFo6Qha8uSJrLAIgAYCJBMz03vDjkPN8H9rR6foXOU%2BbgyvPibYPO5z3j%2Fcq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDMTti5Q6IoZNnxTy9CrcA%2Fxk2PsSqqYkgk1gilV71JSiLAAoeN2i9UcJk7XT81z5ZjbsOAXfWEwYkK2ZLTyM%2F14irw10IzOVQIJSPG8g45zxfkewDQ2wHzKkWIIRnXVWZWqldL%2FduisOj8zOCrBsr4Q8d6erCubYacUJnFE77ujMgjyhwRD1lKYLjhrDOnpiU8rYnf0i4NZTMI0v37L1V9JLYVi6oYf%2F7PE%2B%2Fc5L4ODsyThs%2Fyw%2BzXZun8mq7htkauNAZKGPJTwDBP2l7wGfrO1y6b7SAk%2F5toONUbh031%2F9cCwtCmM7kAqLSMqmkRhF6CLVhHN%2BboZZn5%2FHykTzTFmwgMSTZbupF%2F39YKBlvYW3NSU36hWtHFdMFrpXNJvAHMMz4CQVjDtWnhA4u%2Bkgr2S%2Fsn3ryi4nDXmTGk22GmWT1FOQg9tkWnneaYIflwUEk7m0MUYgs3DPPmZ4mRlj0pYUr%2Fe0M6gI5rTKyf50dhXHdE1%2B%2F%2FM%2FfO5LKXvnLJLXZgedZAYeKe7chfjBvwvMaQ8etstFKlXCE0jF7kKL2J4eyskKOUBFtPqR1xS7MbbUig%2B7MXdCKQP1sGFSg7xf5PImEMZGKJ5et%2F4GKhlyXAToLo57ocyA6DNhatD7x5Fn%2BsZ7QPO07FXF74SyMOyvp8gGOqUBGAQ09aZoRVgja%2FAeP4YdmMRJmlwysm1ntCyXATrAqxzzlvFEkzqu%2FleqCyOJI7M4Ur2Mwka9yXo9Gy2WZiE4hRROc9jTIXKiRU35T2TqALCtQJNO4%2Bhg1qQr9nwE5uk6DCiVIeMVlN%2FSLBrwrupJhMGADYpGP4bSoXKxrBXq0sHfdo3DlLkQoh%2FhHMiDHengr%2F1dKAppEgzWmxdvYV%2BY9MGjHEA4&X-Amz-Signature=d8123219d4f211dd71f30d334a6322ee63c9d222c0ab6ed8f493e2537089d089&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


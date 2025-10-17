---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LDOW2R4%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDOFc95UxvBPloGCIqNeevGSGdbM7EShG%2F8r2j7mcbvPAIgFKrk67QqG%2Fn1DnHeLpe8lydrCZtxiSv6W8MsTxsmXvgqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDt6mMbmIvqE5QcEcCrcA0DcPDGfsl%2FO09D2XEk3dwtr6Wog4xpeFYgHtPzcrBexlaLE1ZeE%2BD8AuV%2FsA6ZfNDESnoBuRMeQNj9NI7dP8sasGBq%2FAcZ5iZZx3dxo8rRwzwJwSti99C3Va%2F3A7SD9B1k%2B73U6ZIF02vEvveq6768oGx6qZ7JUPioT%2F5QaBtw0N1YK5REtqJ0IHZ63CQ7iDlTIL2mhOTMkpp2PjhdwGEycF3XfOv1Qp%2BPtHsRPqD6Lzf%2B0hkg8NtpA0YuMKeeUQbtOZbMb6J2uyclAIo%2Fdl1kwrBIfY6e5KT01CjhaP4JfLRgFo6WrvP8%2BoQoUt1aMjXw5qe5fNEJkvdhjK4O0TNpc5RbENqRN7bvmrMuuTRrQUeIPkG48yiasyFVsX%2FVmcpEvjtcJaupEoycnkiYshkjqTEGvs6G%2BMFZMn3dxsVJ35YGS5gqu49e8YYwD4A6klwYeQ2jrG%2FThVZtr1Hm7sn5IByznVnNEKM8bbH27fMDfWEaxO8jgydrVgxoyfewcg8IWYU81xoMMSpVmMxsO6sxNSncJtqSuqGuLlHWoM1IS5nKPh2JY6u3PCf4gU6fltiSvOfYWjs29%2FD8SzJO8go4VWVXTgoEWWQNiUwQ942nuBKLpw4E1jgd845a0MPzCxscGOqUBUYoaXJGTr1fmNJaF8%2BPBqnVfYrN1EawFu71s3vT0YIKfBUg9%2BUykGN6MukezO0ApqzDqgN4LC5rf9BbuK7SgQ56e30y%2Ff366SOMRDXUlaIGy3OMKgABRPAY7BVIP%2BMYrVTBsHRobchSzCKqlpsLVQX9Slzk3NkOM0ekDbew9XZTevJMAnXeVnly5c3SsHeFVhFkSPHVNNdzpI0J6jEaL3n8YcmDg&X-Amz-Signature=1001f193877d89816f13bce629df0974868dbaf7920be8fce6bec8cb94f294ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


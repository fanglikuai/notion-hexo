---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XX3SHS5J%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIQCQbiRr33xUOySsyirHcossQOhHY%2Fvk%2FxIBPr4PTpc9eQIgI3LsWRlqK3u7pndKTwrRcpAnhQAQcpppaghqAIyjUhgq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDGaDToC9qBMgjBFolircA3PRCDZP7Rz970NZrLuGu%2FezNtKHXt5FVyKX%2BIiahyKtF4pm2hh2ylkDgoGkE3pj3OAaljCojy%2BeUyGpMU04obwr%2BeJqbzWU1MAn%2BTVs%2FbzNhlLlkSHwuMNKoX9nnhRZUD09LR%2BufY2G2yimWXLBTnEAuyE%2B4qFk%2B1CjGoij0drlIjub7vh93uxouu81oU6NJaaBPmXeOBcu7Av3hUQm174Q847JaN%2BPXXxnHTMavI3mFYpEc329Wn4Jz49NxWak9gZ5%2BWRWL8lYXOblIvSpWzSBS3%2B964py9drPe9WPoUjAqBPStYLDWFjusFA6I7XGMJfxFomYL2Aer0uI%2Fmmuan3xPrlS3FAEh4PWLBdFD%2BV47YM2w1OvUCyJ%2FkoLj%2FjJKZMXLFwU5bnF4uyqJS5MJ3gck16hI4TkktKE4jeZd1%2F7oHVYJRkQVL0Dcukfu2Q8D8OxW9pfiGN6tqxg79MsBbtH0cT1zAu6gGuNRJwWNuRtEHeztOmRpH1ad3fHZ2gRpq1vA8HZ6KVUHy%2BZM%2BtZbnlhN3M0AOa4qIxAtTglhSG9KoyiXPc43F5itPSBi%2FpzcmjR%2Bt4nVbczwowH4PBVcJ1ghDJKeGJcviX4q7PNpJbtseDAJFNuTWmk00GeMNmrjskGOqUBj9d6WzRc0PTWPGesXPpz3I9%2BXEqzN%2FNLBttrEz2VIWzCqWcYv%2BfuFDitDQOVliVT85a9o8sOLhb%2BjcxwEYQya8o4levKnXdiAfL69ZGaKNqC0YvE8r1fcICSkLc7uTw%2BHaS8LnWKY5UZzHjOYMwplcBb6atY8XYRndPoVydPTzZatX5jvhPwuygsZg%2BMU1%2Bej0llr3aLtlwlKI9J0ck8SvOaYtmU&X-Amz-Signature=aabc901ada4dc7b7af862be952a572d398730552a571e4bd96638f1143f7a652&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


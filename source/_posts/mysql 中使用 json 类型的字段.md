---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RCVEHHJ3%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T080042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEVupvX47xv7A4x9zkHTDi%2FRkJgc9bI2nV2IgnIVUVp%2BAiEArWXRJLkRNz3lTgQOQpaTHHNAW3J%2B4w%2BQfixqfKXnzNMq%2FwMIbhAAGgw2Mzc0MjMxODM4MDUiDMRF2%2Bxp7O4yljYgiyrcAzFHo1u9hc1MDmf9B8TBHxiHmAX0oTF2aGfSRHf8R7oZPub0swVliuv5ioCdAhrP%2Bz8cl7OiyT2hY%2B%2FRs3Fx1AMpbNhEl8SMoLIISBAAhLek4gfnRhzDi3vgHdj3scFRLKG8w7fXBfUpQKoJG4V2nUxRVvBFWFkfawiLVuqKHsRN3YHLdmXTN9vbxIB93TQHiJYit4tCtuw8q0mPVcFM35cPC7sUIiJRkPTTCoWa7s3NrLHZb8OpgGH4jom2FFt0NJFBwAdsFNj8yzF1lA2APpGEsd7tY%2FoxD93mTzMEHdUhNHC87ulwABDRaP8uE4pGqRO8rrOfxPB3dKeiocN3pTZxiJC6yoGx31KJ9%2FNtQc%2BUpEHOCwEGy%2BlhdE459sOWQPHpOdqZ7tI6hFP%2FJqGTQ%2FAyGXxDy7%2Fr6dySOaNPzU7Y7ecd%2FPxuBtIqxpZCYUyHzAYO8blxwtcSIAtnNa5PIX6lka1uLzPOq5532om5YiCkphKwKDBNV%2FMJw6xUIMInGMVW4YULHjpvHkbbuuS8mMXgPrkq4cmQK6km0mQ7ash6iH%2F6%2BQUdzwA4mtNLTXGdBkfuVpFgV78THKgVPmuJYAcLIi7UlD2j2sISnBJX%2FktGeeRx11PAlu1IsSrBMMeH08YGOqUB941VmpyoolnihT2SWFnWxNzOLy4yUfHd2o%2BnrjFRl%2F6QJ9YAm6EGDClb5tXOIKkxhlQiN5sM7r8uIqBnidaKWZXez5JbtR70FXGp%2Fh1Tyx9z4TGZlrzVt5viHADH6DWGY7jXxEcudegs50M3iRrEfisDriy%2BcXMCmnXpjqn6sGX4Rsa3X3EOwwUCuZdj4Gm6rmlKFeHKOZg20aUro%2BMxpfRJAqdr&X-Amz-Signature=0e7a594c86545a7d5ed45a72f7633a12b2006105f2e7c71c74fda6fad8f8a182&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


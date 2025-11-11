---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662MEB2Q5H%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T070054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJIMEYCIQDFTJyJSN7wliUT2fUqNL7zYrV5IyNs5AzZ8hJjiNL1hwIhAMffrRdvitLmjQp869Mnmtxs6KyxnqijeafO000bS45nKv8DCBcQABoMNjM3NDIzMTgzODA1Igz4B%2B1%2BLnzJPYhqNKYq3ANmcBFAv%2FpGSjWS0M%2F5%2Bo8qPZrJOexq9MOtL0zD9G5MMDR9FopHscFgTyYogeH%2Bpo7Mq5P9CUe0P6K%2Fqf%2BCf8gxYREWVJqppo8LbV4I3hYwhDZKRqVg00KS7swyOkhHR34nmx2dOhjPP8tCRWEerxAQNsGkB%2BeP9%2FliMrSl8M3%2FdM8nVVH1K7M1kjor%2BRWP5qbU%2Fk0LhDwXNoeAZ7zOoTe%2Bni66PVLgbRFTXbp65u83YmZFemR7qFzxjg8saB%2BBMtizguYdfP%2FBIml2d2Du8lf7tPJSUAnowOXfj8Jlmdqge2uoUw01DcPXoU561ECCGEOVKoRo7IRUgfqh4pvDgDopDYQitgIfk8jTP2L8G6HyDrZzoQSEQhJUlp847lmMRfIzpUIDitTwohFHtg0IjgMgQnM9j%2BWqu1t78ZFq35No4kgRt9e9TUWtXgg7C96nXNdNIm6U2NMd0ztuxYKZpg4jfR48VHQsGo9Z77lgg1tqoAY6BRduRgzRM865xcxh7eK6s3zpdYquBLhTJkg1Rf6bQ1cMsmXHEQuUGExiQemkDezvgxvb4s9x0bV%2BDgDWbvnvC8UXb5kj0RzuzCyivTkDWFL6wamEZz8IGtjDzPxlZwRYtL%2FnnV%2FUYFdEVTDBpcvIBjqkAeN0be19ZCLK4POSMC1q7tBLK5oU191CF8EvyNcBmPgzkbhRSB6d%2BCJnrcuvh7cqtCgx2%2FmB71EYlCc1dHOxe%2BoACHUqA6GhhzzGpykuNtuneAXS2wIBhUOoDHKmn8pw5PZ1HTHtu3vNYjuTHGQ%2Bh7YYNsCqH6h0%2BOZFeMUm1EWfxYInVUDlgc1aGFCA5T%2FI4EoWu4A56OyXBWokpYAqza6P90%2F%2F&X-Amz-Signature=219c3ca675c09ac2dc8f3c0da5f8490892942b687c178b083b0d282a2b6a4128&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZE5DCK7B%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T070049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQCa514yzjJuF17DHCLGZTYnB87no6ZUh9n8lCiqNdsukAIgTnkOE27zqbDSCv2zZtWgUzqkKyANJlOX1UUO%2BqNZjVYqiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAB%2FAqSRUEQDlUagzircAyjmidyCnv9g2HeOcAyaBYsmzUdHck2%2FH0SehEVcwyT28BVxKcHsHl3IsWKqa6ofZH3Q%2FyMrmgy%2FqvB%2B35H8frzICPz8vtr%2Bh4fWJue8tM%2BdCc5SnNqpixL8MJaxjZTOdtEF%2F7duc49bLQfEF3nzojJDX%2BvuF89c52Y31Ysn9lh3%2B2uBRbvQTmtUAJI%2BLW6WuFYZP6GvQDdR2e28tvm3DXQwF4Sn%2FnKr9aSTXJcs3bum3IpWzWSDE2EzinUTNz1nPnRoi6Ojdx4X%2B4WXGqDJAWxi6mMBEq7MS2pYtrxrxvjRcz0czZfhQovMK7%2Bn5W0qbGVCsQ0fJZX%2FHhQJrOLNNZoELRlWs2qNM7txYAtdpY7pFw%2BKd1h2SB5Q8Nd7Zxd7F4bk4Ea2ztmgs%2BX8q%2FMWsAOt3gdPpFL7mCIhcT%2FE%2BUPkdnk4HFq4dYUB7Hq%2BU2Kr6kLqv3QzTHi5KkI18F4TpUKbPOejMe1lc%2FE1KKYGTS2KINFy4YOpSy2ADRi09gCWz5uPFFw1%2FJbn8LJWVY6%2FpLlJt6V49sZlyRXsF7o4xGtSA9Wt427xOj%2FbCw8SvcvlyOqYRH8LCNp4RFQGTOl1Dd5XfzSXJbEuzfPef5SBGpYMzTzJ0aWJpQx4br6MMLjBs8YGOqUBwdE%2Fnf3j4mnlKNzgLMmJyvMxgcgbb2D%2B1MC2IdaUV8EeNeEV8lbdbi0BRltI%2FT8LZnvUfUMfhgtZ9Rjril5qR7xmkl16KvZKrojd0wj1ss3EdUzJ2ZjIzCRvOneSK5ZKyfusCUNS1MytNzRXxIdtFNPeQkTu2825iGgqU51ZouDUD2zGxc%2F2SBRjNmN7Rz0xFvjrZib5Y8yYOxn1eESDNRmau92C&X-Amz-Signature=2e4f4dc46907f93d194b628bca2a588191257cb42e32959fd1142434b54a2dde&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


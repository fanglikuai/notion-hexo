---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XRWQACQN%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIESfE9JGoGOgrLpYtME8gVjVSpCSqNmaswFbS8B4MOpsAiEArGvxgi3x9GWaxwBw3h4zXxaKiqn3iH8osc1qE4pk4%2FYqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM3EkFO5a2pOqQRWVCrcA8YqnF9anthRjrTYAQJ8jHNhg4OqkcbK4N2IV7a3bH2t66Mm1DeHkKlkXPDOWUwMMikeAaEilJ2s1%2BTthe2jIOQotNtQ5tMgIpLdICb6Bwa39750bFpLYGpKyGlFnWzWL820eQp%2F%2F4ypRd613w4v%2B9FEsQ11T6pvA81HVd3AFW3IzkfbR6bH6Cq0O5YPVlkmfsQvUckZh5XwyidM3h2QIoqtTxmQxcf%2BHuHSHWjk6hSZwGhQvZ0P8lbt8GMvm74Otk3GmwY5nUNIBO06gKICriu1iRsHo8XSbRehTg%2Fd2IaAasrbtkPpiLAVKsYA0SGpJT6zdey%2BFPVgTQLGMwGr47MpGwvQOueR%2BaWEsm7j7mbItTinSR%2BLDiwXMmkBlfK0GjI%2FM7ebq046uwqtgOwC%2BxpwOYEKkykLCsaQtpqf8koWcpz7p%2FayHpeB34f%2BXjDXCZ1trKBIodcq%2FqahilaDNj39sxSlGDeq%2BbLw8b95O5r6sakGi1GYYlyyB%2F6ikmYb5RiB5X8c74bTjssoYbx3Hexe3tjQA5ByxmMLFlzo0y0VB8PViFfUvSa3duZ8ARiWanjBumfhvyCqtuoTgvZPzshVwGdQoPZPkFaK6ueXwOuzueBSnSared3xbcpQMK%2F31ccGOqUBlXI36RZvt4ny6QkmBY%2BmeizgaVNWUGzC51Ih6qRppy3MEjlYFTDdHPZ33cXFQuVoVXZaakHmV9Gv3KCqvL3kR6rqDWHNvf0K5pX14C%2FBXQlQJ6Sd2UWN%2FUBKHcScd4YRvnMJo6IaHP4L5Du3KKqOVL5kJjSSULNLG3nBRLiK0zDBBbixym764bco7Ay0Zxwo%2BnI5%2FtjmOXXFgaT5gYbvTojol%2B96&X-Amz-Signature=b95d53783c4ca94bc46ff2550760d0c71a75656336e375a40e2dadd79b2fdfbc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


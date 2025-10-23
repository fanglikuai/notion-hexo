---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SF26PZXA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH4OL3QL64xRQD4RZCqKZwq4V5jw8SxSvtLdcQhPmIkXAiAla%2Bz2wLcWQxuXYLZKB4Wg%2BtOKM0Jcok69ZQ2LUlxQdSr%2FAwg5EAAaDDYzNzQyMzE4MzgwNSIMOMBHd0txP%2FJAHX%2BcKtwD45owEDE%2B%2Bq5lHpSBZ3p7RCWHFKIqx%2FcH2Koq5xcZyjdEq4n76T9AGE714SBKg1iN3kmwWpntWPvw3YLq5oVdV98yx%2FStOGKaH6htwMCaunnQHl8Eq4La5V42JtazlPzha6jXebf4N9OdRp0tY094NG0JIbG%2Fl%2B5E2do3or%2B%2Fd%2FAX7TcuxVFfULPnCy6wcN9%2Fl2z%2B8zpQ%2F6vNvfwpvdoyhw1DRZF%2BqBWXcS2tbxOBck1BnsH0GiK9m3yOZPxGtCi8JqMqW8ZZeHnZCQCKB66Ddeq3jsQN6SlgNRFd8NRKSd9DE5WJQYCwUX5vcz9XAexUTKs8B56rs%2BULLhgnAPZa77mu57as5NRLk1yuDDCdoK%2F87AKZfSYBQv6BfHZNErMk8XBFGLmxo13D2rpXx7dkZmDMBCT4BKxYGm9LZO0pwOepoZmNa4QKkU4bZUG47wQYVe7AQK3ds4I7R377ujcKn7ISN3N7SYZUcVC0vie2VO71z%2BQRTrQjmaf0NV5euu49qo30fjp73cIV5uC5LY9wbKZytNr5aIiWtRpzsShXjoBpYou6%2BtWopNL1WD%2BUawXNlTUiNI%2BP3U%2Fec4NV5R36WAmtfOG%2Beqe%2B0lNo5gikAG6X3fiLgUEALZLKJREw5trlxwY6pgGGBWDf7IxBjmLic81Oqy7ZLIU8y%2BouYrG0ueg%2Fcjapwsk16aoHVKJUmWUSgk05VxBpy1gJPhKWxA%2F0xaoe4HICxYublamvXqNdNM8sFesh9Kp147AildDlrM6Cwe%2BNhBGb7Qc%2FnDB224RSyFIC8H7yTmfKzGQTQ%2FkLLQ7BGt6HfMoKHdns4dSqwpn%2FelL5MPGGyxVhVgrAg5XbgZbFkimviNnJ7%2FMh&X-Amz-Signature=0db7c106f1031878e80fc997b2a9b89cfb30d632254dfc101fd1689bad782612&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


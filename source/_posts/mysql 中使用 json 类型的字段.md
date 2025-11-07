---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662H4T5T7E%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T090059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSTmXwilsqV1d9Cgl3gKJO3eKVIab9stTWwIkNE0YqxgIhANQ9FTMEEZVkrR%2F94ueiv7OHkmBGjH5lPFmLZxDko%2Fo2KogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw%2BbzV2HJ8lmtSK6Wwq3ANoMukOErDlSgFvt%2Br%2FaryCd7NnR9Tg9ThtBW6WpJKi%2BgU6FD%2BMPHUOXP0vHU28hEI%2FiveC2O7UMcIriNgBzUPw6l%2FthwINZ0Gf1c9Jm80aKlSnHo9xcw5eBcVD%2F%2FLf7Jy8cyaPxIEIZAi%2F%2Fu2rlzALZyoG%2FD7EgcEZKnJJfyU4pjfnHgps0870cNETOYD7HqMAmjyUEDWQZ1%2BwmN2KtMSxXaxMLYemdV%2FdyLPTP%2BCUY3WHFB%2BJxYqIiM2Ojg2g7PHzArBBNjV0jLXFsY0q%2BYfPMKZpD3kB6LjQaM8Ek2Gb7pzybHr4CLraWr67BTbLo9rXWoFZgCDyYK06VrCF42kFFWC4S5hfqThIYb1RBd812leC4xzmFSmTiiIWAQ6hqIDWC7n4OM3Dv%2FfV%2BtSelknlEQlcJc7PZ%2F6X0qx5rml6Fninn3xPtDIb0LQar88%2BuM%2B6SfUcyMI4LA0%2BUIQCXjrvhxHu2dnxSQD1eFDdmJuWcDqBroinA1CpckBN949dnZMGNKUXOFCFO8UFRxOC4%2B7OKdpjfXXPGoo9BpqlNgJlNGiRf3pWiePQBiOx3nyyvcjLqFZUHdtysnRilgMcnQKgTDWO5JdQ2ZqY1CcVV3yeCDwpZLb9qpZCtgF1oDCY%2B7XIBjqkAXuc6KuDGWuggrh2zoYst7%2FzpN9YkdONCbwBHSwrGhKNj6Q42AISlxCbr3j%2FjyTylElrWUghHS9tqaTIJDHgoBlWyF1zytPx1KpXRQ4nwUTVhc0%2BFEBzN3GiNGYkxaI6CO5ABJzFqLba6HSc3ubLhnsU7k2SJ2S0CR7uMEhwUG8IXgjMTs1I3rwK6uVBgoXJht2JOcyKrfS7TYO1ZU%2Fri5y3lwie&X-Amz-Signature=45827a502281ddfc3069595e4c68483b37725bfe0d3c53d35e41052838bc550e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


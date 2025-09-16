---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFYUMYAX%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJHMEUCIBBO%2FFv3zMk77UHdzkRZeW6M56xe%2FAatPCKZsOnSUbE0AiEA75fDY9k5T%2FeaQ77a6sDrHJAyQXgxpAlV9Yqzljm3IJcqiAQIlf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDALCzlI0MsHvNWKtkSrcA623FaqXZzG5PS2zDBoYyyS9DHrPwh4ZcOT8vm0TU5cDYWNwGUu04v0rIqsPmsu8QXJhxUhyjaX0HrZD6vNiafJokM5KPjXfZFsD26TbMIXACtLbyl4naixlJA5MSDRxYRWLYObfBN5aPl0zSH7Ly%2BVsl5clkuPsF2s5LEgRFLuYy4BJ3pOqDm3M%2Bau%2BgxikqsKvfWSoQM4mQl90ct6w8P9zerqmjXJj0OOsbevkZQ2b27AowqbjdkOuMA4jsiRMMgEAUR4ifv4k%2BmC7ycwcJa0a7DTxGWELQADActXfGHkF1pT44Na4hK79ijBgvXzKKce4rorhHTEqhuoyws9d88V5EgfijSVKGxk7PZLW8Yj3lOOeSdNPOUyvcFcUDjUJFyi%2BntIkabjv5V%2FSYlgG0SzHtiPQzBAfj%2BlT1Ik1HQFL1aLQhjvcH5FDvQeGUaV8T3s4gcJ1xfuCUU36c8ZDRMK%2FDZvFHj0xhXP6XeWblsABIRlieDeOrnrVkXPKkKAfW2e5tuko2tLtgWH3Y8HB%2BOuoK6ml7XR73LZr1z77PzP2H8ppWvGyT0U20r26golcntYh6yQNmZFM%2BKKZ2xoRmjUircV3peC%2BxNhqq0snJBPdVzTBp9ChgXLmFzfDMP2Cp8YGOqUBgF6qu%2FHKBEmuR1Q8IqKGxt3k7vwXa80ca1mlkBVjs2hV66O4Eyb1ysRuUjjvE9R0DWnOXutdbMuqCg8E0ZXiz%2BEdYJ34G0qyC9tWpmww2mT78jLOvJBgETmVJ52yHDJcNJgJRz6%2Bsibs39q09BT8VUjo1zO3hmW4HbmPJdvnVScHX%2FWmz7pxW%2F4pmJSSE21TtFS66snvW0ZDrhtKqwR%2Bovah9MU1&X-Amz-Signature=e10c747819d4e97f1203960e77d4c8d6f867f6eaa74c188dfdc8fbaef9fe1032&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


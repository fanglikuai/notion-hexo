---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJIKXPXA%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T040051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDoXnZD2fPApahhX%2B5PkaXMjRHndyEzf5hweNlYtZYbIwIgbBGnv%2F0B3itudylFIg3C5OqV1icpHzoUm6nimi6jExMq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDBoTnOc6t1XvD8QbCCrcA4Oiffi5nYiUfi9cmaMDwycZaydXQA655bJk0BsdpvdCI5XPJrwylK78aERlEUj%2FUb%2BQgkT2rRwgOGJW7vi8fSoru3Ll8Xbv354DcFZrt9OBuyyaGfb8ksBzb7cBq6sDzKeN3J1idGU%2Fz%2B6IoTt3sXsA%2BnThyhfZIN5%2BCBqoGP74YWGbWn9A3NwTq9gJ8ysnDNsH%2BkR9ygBaLsZsB3vhNp39Yomkwoyc2SUgXiMiUTc8%2B2FO%2Fpl%2FUdOzjJ2lOhcwZXIyV6dox6ok%2FmQbTfo3s%2B58Z9ubhNUhScYa5kJEbrhRVoLfNLAT0kzWB5%2BiUNm5FpU83lJGa8H%2BTuwtxahz%2FkP7h6NlBT%2BkfvwPyweIGhwGmLhxDWq7RCIJLlbjjoeaxpqhjqsJ0koAFBht9D6ghizjUl%2BN8czcympSKmpLNk6vwaAS4s2%2FsjqBJR9qCEYoRC48ufCjlnSAp7ndIFdqTm6aV3CX3DFurgQQTUnf1TGFa3LkqEkFbz59yZOi4woCa0FtW0pNBJttmVPXRtk0n4vTGE9I5QtfsggAXGQZnEDlAeayrMJerimL%2FG%2BbFvNXwJUNPUBxSJ1mpUri4afGH%2B8g9gM4bKzyTjfgCWSa5UPs%2F%2FQsGStOK1JpNjEmMNDe98YGOqUBbGEqOjqStAy3ceV36i%2Fo7GZWYc0LJ3FxMS4ARxW8UtJR5pjWJ5INCYmjnSGZo%2BAxzbhedfpjan12UyrzYPSRdgxmKGhgMQWbH%2BGvxj8SDEJ1Mbu5KzMumBeydT6Lp3stX%2FxOfb4zsfdKsqduC5C3TJsZ10QSqs%2FFxORrzGe%2FQXiPcLWNQ9peTlTWtRlO%2FAG5WuyN95%2FiP6VF71OOanIYM8xW36UT&X-Amz-Signature=ca2a7bb8bb724b216ba6c3f473c75de275b05c1c6b845d4485b5e0a5d395d3b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


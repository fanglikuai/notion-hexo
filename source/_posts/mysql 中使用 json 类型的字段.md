---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMA3SS3L%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T090134Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF7V80cxeTePp3HhOi4PkBrQe9TN3QPex6P1MDHTLOFcAiEAh%2Bn4IJe8dyQQY9kxwzruoqc9Q4yfFh14jLChX0sFvFsq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDJ25Hu6TjU5%2FaYX3KCrcA5i12u%2Bm7XqF6PKvpyMG244I6RJistR7huAb2G6QDdEYawCtboi%2BQ6mf2I9RSvNTR3axKJa2vSR6FLNqCbYQb7tSfD2C5cybuOq2LDzBnnKmmXH1qRfUXEgimLXyKlho14ZulfHOg3dc4uDcZhYGJqrCRbTD9XATeRmfRCdGSzFIuCj7GLmKQZNmsXjlmz%2FZeXIH%2B4DOkaZwVPiBUanWDuiRPe%2F4a6KTfX%2F4CXrpoAGHq%2FueaMURNfcRIqTiok5Z%2FpCaIg5cgg2pBqmPrPezmdj%2Fm7mQt36f7NAiSZskEIa6ZO%2Fsjl8G2ego2F8fz1YYPFCcIt%2FZ0JmXd4tLNxvHO6KmNqDGzuuoHHSZkX21DzU4zUt8TwIHpGuea4Gv6jPm4afshdaE%2FFm9Wp0GGy8CrFJ11vGDVDvHAC8Bq9M8fewVwixab6DmX%2FaR2mMS1Np0Y2%2F9xlfS%2FD6Xz9%2FIcdLBoqxlX7lqJ%2FA%2BwM5Qe7HoZql2oNR5b%2BfTm0ai6S8atKnO5vzKBB00Y4FAgxAkanRIEiq6Cf8U1JiSnkKBzi0qRhPFfX2I5T%2BfXI6y8VVfz9tliaKsO7IS5ATJ8H4RCd2Py%2BvJeaD1pZ3XoD%2FKbiSuqh93jAUPypaTXQJKVmqUMPjv7McGOqUB7qXWW0osjEu9cslS5xU5FWLxB8wOI1r%2FkbAI3T3OWhQHSI0tu2%2FsoMIQa47FY0n1WsZbYYtAnEWfCkAlT4n0LoKORYflkXqzuUMFOGnFvxwxvfExF0tTkeA4UON3Vwmwj1JKXFK8nHk%2FylrV2fEfDLBk1y4Db1hX3wqf1ZW2scgLdW%2F9PkwpxR%2BsU6bPLj4dJLbUnVXy%2BQxIw4GlCzZPZ2%2F5wzlC&X-Amz-Signature=da776c1d589be1f509a602da2f1f1055f94e1d584858e78c5f397aec4ef801b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


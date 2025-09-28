---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PKKCW7C%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIGOob%2BJto1HWuYxxBYyLwI09xrsTZapFfBCe8FDvMYoyAiAPC2wC1vpYjhkutiCf1sicYI%2B0rA6ytsHGXviRzCLlTyqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdMSzAkMc9XPv8U41KtwDmfJwn4jkiI4UuQo1Q%2FPCaoeC3MvDV%2F%2FY6M7RLL5GVop%2FaNeLRh7uieCAwudAFMjB%2BblBoUlI6fZhYKpsYipPX%2BEdPK%2FkdDz5AsBqV4n%2FG85aldHHc%2B2ksqEVf8PpazRENEiaXrTd344uf27J6uMN%2Fm9hWUYBvB2L3tKZeVdwlC7pykD7vf0Bp2mbstsUvZ6v4szis49sVdF65E0yRmMJYJkB1qSYyBxj%2Bw4JpbZ%2FUcqSlrwj3yKIyOWwHx0WSvAHjgWpeko2%2FiGppliO68KjCAmvKTcHB5uY%2Bn15r8nvIwt3ivEAMw26QfcXT1QJLM4VBy6Hr48X8pACQCDZKVzw1tpw48r2H8idQoFbZjV9%2BfwvrMoD%2FO0io%2FN8eWJDW6oKn2mEmpDRYx9gFRUwCneCnJFLOpnK8Dbr364b16QsBf17SXxB6bIergC2KbNngiFGdRyBcg2y%2B4%2Bs1UAxBpa%2FPa7frmK%2Fdz7Vx7UZQrRNFQKKY%2BBkIEn%2F2eRvCg8A48md%2Fmtr91ToW0RAN%2F9cNMt3xZyB0VjO0KaNbI22lZuLvnHqr5zC4rJGyQPD2OMMzkgZOZmxHZgsjDDo2Lr4OH71CVQrHwfINzTiXznBqnQWIxwZvB%2FfU1NiCy2oElQw67%2FkxgY6pgFPaonAmPNue0YL6rsRdeMVdiK2G7jstvkY0CJvGriu4TpqWu1ne%2FJjmMiB9ZgwI1ZAd%2FZ9Zu2F6KoWV9HZogJUq5Mz5J4dA1peDT%2BRVeI8D7SWzVu583YYHyUJTpwPPZDAKpg1blo%2FVgK3gQrGIXrDkdmz0udOGQoSvS90y3b0OuCnIWI0uEW07BjNALRptxtSkqdpOgIpseslIjw%2FCFQFI8t1gg74&X-Amz-Signature=6db4720a538d4b11a6d7e8326716da3517e75e61a921751da36e311f00f5c130&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVKOV62V%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJGMEQCIHlC9XBvuYVocBvrpTBp3q%2F%2BQ%2Bp%2FKEuCSikG2JtbAW2LAiBaPVvaAFtBSzIaS7D1IgCHNy4AOJpDbsAJat46fIHqxCqIBAj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAK9t4cVQBTGh4nf4KtwD%2Bx6dZ3GR%2BnM9HR7scwNYKhY9iJ0RVuy6XpNEUTouimEp%2FrfFT8%2FXSmuuP5LlBtb8xaRgwlkI3p0hJPGz8eeg%2BYMtm4KWeHkSIn9c4thkDZgb0AuxiW8UouSDBIpPinsQ48fBI6ph%2BsgBiyKbqyudnsLwGqgeJzVixIeQ8l3jopQqjZk2Es62DsS%2BuxvkjhzQGqnKAOkEF5aJffzJNC2clnCAWUXBQkhCYWiKpz3rpE1SJIdA9xDOKrHJRfqk1eYeanvBfkenLoaP7NDnEX7GErn%2F98ZbbpgjnsMpfPJjtaxwiU2UnSoo5J6Rv2KLunhPKuq99xzrO%2FcR5N2ZVIOqFlOdsMwkI3KnWpyaskLnF7%2FEoj8OlrxLE3Q1FFT7v7VjLsD%2B0J5SO33%2BX7fFOJwyCKpaB6weS8YznoXoXeIP664zXhj9Av1V821y2FMZKb%2BoVN1JcQlhoRca7wUgMqjfO0fTq5pV5ZbGRed%2FyOlxPYro93mx88heIjQ0YihQtuTASCqTBc%2FrIcrjo2sXb4TUhQP8R21iHWGivRLr4TyeSGfJia3gaBQ2aG2SxQA4Wbi9r6GgzNmsq0uNMLyQCOfGVUlstunkPJiChUi%2F6lBhB6FWZrcSVdlUxAaVUOcwgqn%2ByAY6pgFfBzd79MD8Md%2BbVE%2F8BummXldFDOImjO%2FFp7O47lNyZZcgzxgynOzYyBjg3k8EsCFZshaPT60Y8FP8hNvGgmc5BveXmK6bQ413JVsJ9JWeBG%2F9bfk4DP3%2FAkbBMjS6hHmOGzrwbzaPjLXa3Fx9ZJivZLVL%2BrihkFLwkhPsLYOiUObWfnSlgosTk5V1NgjK6eAAxkJaj1EtBa91IqjRUDM23Ii29m26&X-Amz-Signature=57652adcafa29bba9441a5f68806e53ed738ecbbbea923e96bf06b8a28554adc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


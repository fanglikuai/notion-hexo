---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNYGCL55%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T100038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7GErKPnw8nvs3zWVAxZ%2B6KsEYjtRgv66GJRDSOBA%2FAwIgIkZQebgmteZkWBDK0jHRsEES1mFbpkemiz7ZM1av8EAqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKOV1TZxBbPsv%2FDhQCrcA5aOMZ%2BwTT9qxKRjsGcpASIbadr08n3LxwxXsf%2BW%2FoeRHAK4WEE4vZj6Ij5WbWW2OeJxJCBIaIeMbtTfTKo9nvikmwl6dkT74FCWnJ5yXCQ%2BJ%2Fnf2DPUypjfSvwYIbzn7xD9tWNEU5fOZHhgi4lcbPHwAhjs%2BzwYXJAhd%2FfcmdsPCCHzbpwigRuJM%2FHG7jOPhMxcBWfrn1WL3yHGWp4R1gKOZIZkzeZZTH3MY5Czpw7XTGxBKkLPGaluttrrCyU7oxtdR8Z9Dr69cR4dR0Ja2aWUz5%2FyYb5DnhZO5yOh%2FyIZ%2F7HBh3G5ItJf%2BBQQI6Ck0Gp1LNEWcEMUB%2FS3iB1rDo7i9NwmKqI%2FmjSxNn9EMCnK7xtn955S5WmPaMCAXdvCTWo8N3871hcnlEVovcKtfRDMiHucrxxvMzWPToIZyeVHDzFUyIyxrVmt5fe44mnN4SgGggYXLF7leT4PLWUAqjg8BEGvzS7Wvre8IOi90gEViaFIQRdO8gdu9hcxDG0p897om5BhG3qN5tImWUlGEOiX9O43m%2BZMin%2FOL%2FHCc5uwbMfWLYZmK7RrR8ococOzSw3%2F%2Bn5m9mjtZbJPgWw98pdH%2FrKOR20EJjWbcXLNiGYcy0KrLdil2plsDw1iMMb%2FvcYGOqUBZmQgQXAhvBeYhsJ0Bk%2FR2AzURv836R7rH7ieLzF7GMP4ESMP%2BAz9cTUdbxX8rHpJxVsmznOqrpDHrBHHqp%2BXJBsIinBoQiJCbyqvyRO2Ni6iejhwYBgCruhCcjiUv4h2JLxeouP70S%2FYZl9MxU6arFqorkDNy0bM6kCJRJpQJv4U%2FobAXCfqev%2BHzbmxD17qWo8LvvDsuSQAjZ4tLSTpsMGP87iW&X-Amz-Signature=5cb5f580e1693ebaadad9f9b50fb9917d93f3b7c3d290abb5522ef72d54390ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


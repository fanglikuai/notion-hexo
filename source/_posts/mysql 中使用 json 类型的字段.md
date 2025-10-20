---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZOCLPEK%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T010056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIQCxeRPUmbkty5RIZ8qn1pJpvbZPnKV%2FGuhHJ%2FBOLPdSVAIgA30eFYkmJIsK4C1gzojsVQRAUCncsNEJ7oXMXX43r3IqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNGoKYgQM9HLCU6kFyrcA2rYd0GbGlSk5xHPxhwgeMaxtSDT63Y9ken9mLDyyURlha%2Fs40EwXk2WYnBUj1GRsvfd7xp7APKz%2Be1llAZHMXq%2FAU3lbIR%2B6ZWsHkP0tY6diDo3bT0fHu08nUf4Xe0%2FBOLpiCxvmISuIEyursfFhgLaNAeCM4ZUxid4%2BVvlhYjehoE3Pk2L3rJgAz1ZoGvw3U1ZTDfKC7cJQ0kOJAbVAr69n10gkTJUyfvbYkK17bsAQTMVHijzsKCoC%2Fa1IrVrRlnKVBZ2jAYMtKfY%2FpjF%2FazFFprZniQbNSMzacFe%2FhwoOx6Hh%2B0Cxxq15TanvyFj00jeNQ4GZsGLJg42wRtxn5UBKRPh6K68now5x4XD6188zNRuhTt%2BtQiEEYrnxVZtbzrGuJlEEIFnyoF%2Bg0O148uOwi9Pt9ubr%2BzXIcj5cIJBeoZSJVitRKf4NqnmeGHXUkr8ra%2FJ4Tlp98858EN9ip8N%2F1KOdhrs7y3uQ5VYBUquc7k5sP%2BJbO1PqOMrF1Dip%2BIOGg6IREDJY2faRqsNzqk3LVsZFSy1XO9Qy%2FAIsl1SiD%2F2azi5AQU0VSwbUiE8YJr%2Fwn%2BGV91ISBa5Jjx7TYCkUQW8X4gzsCSWACG7pdMi6FVbj0oDNJ1WjHVnMKz41ccGOqUBg6hOJlDTP3CXzAtzQaHud5qp8aVy6cEP3i9yJDfjsNhUzdlDDxuuw0z%2FdOFOwPlL2u5uBWZfHKFPIeJP3XlmeJbIWmXG%2FVYl7Bn2B3BJ5VWc87dcHEYEGsqYVJC7KixEMQCLy84QWA4fKib3%2Fbn4uAOwA2NDt0%2FuvBvo%2FplEAhcNaw5q%2BlW3cpOs5bNB%2FBepkaG1YyYSI4%2FqqC1vIOmz8t4dBFlT&X-Amz-Signature=1c37dd70eea9e037727108b6dbe434120f737d2951e10089218cd7bef645a720&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


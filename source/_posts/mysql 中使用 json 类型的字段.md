---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SWCZGCHI%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE9aO68e4KR635XSJQ7Bvf8ABJIuNFF2MWIePVjvteW7AiBv2NUZX9GkrcWbe38FmqgmMQbg0AWU8JgrZ3Kj2vXbtSr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMCTuGtOvBjSQS20lKKtwDCVLNGdB5pvFMYfDUqDbM%2FubD7urnCeh3%2Fpt%2Bj4z7DwBZS00lk1KMYjKt7mYWP1Bvr4gAoI4MPRqyikEKLEpMwlHeFBYVqaAHRgw8MvveC8rYMV05sem6QgaepZzoRPnTkjuTzoZggX%2BkKabIoQtm%2Byd3VTbNulZb1GtNOZKckeYUGiYy2kUUI6n%2F2Qdahdk%2FLXuIHrULbUOgfHGQLDkl7hCCTIuQGdFUAcNWS%2BRkUepHZrrG05GuzdGlQ2xPs0dEPjQKXfayx9Kc4vso37xCn%2FKEBb8a9qpIT0j6iwBYzEwGe0Kbk1PDbTcwGIXfvFjXgSgmFES3AOH%2BIIYuHQJ3oLMz97eAw8Qm1DKhUNbYIWWyPmbbD8XfNzmRTHem58%2Fx2chDwltknLdqWNj%2B3fja9oukbvGRzxJPCe0FeI4YO%2F1LiCvRzXtjrudcrOctRy03DpkVEmYfCh5ll59CwA0K4T2BsCHFBh%2B4DhlavPDirmQTwNQB%2Bx83VoP4cPlgICt52%2F1WE4wLQ3tj%2B22xz2gNhZkm7Y5qOGWRfYKyLFy9s9Dl2P3rtLBkNym8De%2BLZPfMah5sOvS%2FmUNqDzaZq4TcO7pM6EKlWSbJcgNW22LbLh8kPq%2FUdTJi1VZ7xygwytfyxwY6pgHBdvgObfqxEartDORaNVWZWSszTBY%2B8IpCE81b6zP90XWp5CUG2MzQvDe5PZ2iiR0U80f5HdkIVRcYY0Q1ASYZrNdRdf%2BA0DTOZKmWXn9CyjbZwcYDU3HYEAhsKQz%2BYUOdrqSsxywzQh5a%2FufmCjOQK4aSTY5eHq6XyM220gIbLvjlnkAjKPm7HnwzeTDr0LAelUmbXvH4hzn2DeZc83pH2tdXyPIW&X-Amz-Signature=c3a38fea7b7619e168d67385b68c0e766226d508e74b52ab2782626ba4169427&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


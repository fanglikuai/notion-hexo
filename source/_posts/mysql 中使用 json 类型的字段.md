---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SIIDKU7Q%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIHC0gg%2B3R%2BXLd3VCskeS7DJRZ%2FXNt1z8fnRBVvpm52zuAiEA8sys6zj8B0YSI3qlj6RBVDp%2FfI6qiaqFLPB5FktOYZUq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDCzdGcZPqoVaXylWuSrcAy89AQ4%2FJWd8xc6cRp8UyrpsxBVmftKFzXzjNlbbxEhNRcXQxsaKVzByz6ofT8sVhbIUkVbY%2BXtVBbMHJ94KMhnlw4V1WiBrSv%2FUHJgZ4RW0ckE1FCzXCaADndxQItavTNftKWa20M7xFH0qFygSzbfa9HL3q9VIgPDgLvBuw9WhoiflsCVYwXGJFC6PY%2Bb04WK7lwReWDhJex5sTSrZFePm4%2BWJtpLJYtA0AjrHaRVgUoaIZGcLg2GxTVB%2Fy1CwRyGf7cozLf4MsKEuADY4ko57B6G6pIhiT55TZcOzcPgBmuI1n34GvfZPA5CZ76PgyW9ohQGgm4Zn9A8rvgeyY3Ml4qNi1f%2B2Hb7TUllWGKDTX4lX9x4y4CFZyW7VW8H3JZ3CLX%2B2s4%2BEZE5Dm2y7Qv4E5%2BSRTJbNZgtzbAl1G1fGuLcpVlEjDGZvisZO9xXOd53q4lEgX%2Bp2XTxzvk8yy5lPHIQ%2FDMcx17ZZ5jsyVNqIaE05OKXPCRw%2Bhfn%2B52GV7mxQtQ%2FL3BAN19kqgAsPTvtsDgx3v7S0%2Bt%2FRHUIdC6MDmUT0WlA%2BYaT1WjGd5lBDcbw3Ge73FM5fdTBqa91PxxLImrspKT3jB8kpLEqPhNBus1v3jA4gD6jW2NBDMJHxmsgGOqUBwYlIbF8sWF4blHIy05OPvq8PMAHtK3W28%2FaVQRsNmdR4CKJTTRXXQJ9hcdBN482sCDL0OIxTwvUoLzD4BJnO5CsbGIZgGlJyxsU7NWrjVbO200BWfn4FLPGzjmPp%2BogGTcWEkxKZ%2FXVxpCIsV%2FUy24ZqufYwNTVVYqEfx%2FPwmusvSQ2TF7nrzCeYERJofIHFzqylhmXz8Jcth6hgb%2B1XjtVNz%2FE%2F&X-Amz-Signature=05fd2a30da2ec2e4204a19e6d248c5b3e1f47ced489a525bcb581790cc62db58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


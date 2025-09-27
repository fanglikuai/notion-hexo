---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YKS7H2J%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBAaCXVzLXdlc3QtMiJHMEUCIQD8%2B5DaO6qs8gRrPhu9607vYfbOT3TeaLKQRbsqFER5FAIgTvTb2eOnoPCs4ZBdowZ0uI5aXYZRGqG4%2ByG40m4iSTIqiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBs9RdtWB%2BDIyICc2CrcA49H3pFeLCGJrDeNC0cJgXetAm4GklFGK8chYu09Q%2FmFh1nHHWAkbHF4H1ezMQjPtdU8obMpWlEzfIcikhdMv8bQ3rGukBYqs%2Ffs74rznv%2FQvarBjTuGoUq%2FiBfLl55w1YlrcHrHMwA8zq%2BzNmkoqyKdTKdM%2BGGSzsBXWmo85zCzH2fD0Tqv%2FIUHIdoaqvnCu497LHeNKVZ1c%2BGNrgF%2BM2XXNDORXWyDTVdLPkH0Mxa2aK9RppZ5Zq%2F6PIcLmbORJ8PyHpif5Lj9%2B1Qw7%2FbEwNSaes5E72znE883nXW%2BiUGEoPiSR%2BXzsgSHKAO6y2yddJYyGWS5fs5vmB8INTBNgWmktFHt1DyUWGHRLayGoYkZ92X4h2l6%2BRFWHKqMgU2DhdV4evs6UK9zfX%2Bky06kIoXuqe%2BcpRESb5M8AU9avVw%2FEvhR4KCoKkoJQv9GF8tkccJ2ElK9aY5xs6UacL8Yh%2FeJjjgrQ9Z2P1Br04WdgcRtsjwV%2B5X6rSrFXy7fbYdSiZ4K%2BiUBuDLp11uZxZnwr0jjSvPQ%2F3HvCxdJslPKlsAB0kAR2uTcpamkfckjcDU2HVUTLvbkYbsKgiYwzTULSsynJTM%2FqqC1gihWOwx%2Fyj8CVvTmMOLzTG2nvH6GMLfR3MYGOqUB7gcPu3GF%2BmwYSSezVlQ0KxEBBjcbrFfDBId0%2By%2FVvd7csM6feE5FLcbIchdbzIArl5M4idlpJSK8Qg19Ah0vjZpJyi0UZRFE0RAWGUR8VvYvBCb3TqRfXQe7wEzWfQ3pJWxp5SqLj6VRHQCL9TuS41gd86DmT6oi%2F%2FOV4RZoCfZdBT77SsRLbkuAi9jxrSp%2BKxX%2BtVSMV5g8Wa5s%2BlTCU0d3bViH&X-Amz-Signature=8c388e1627cb8db09b76bedeab9d405ade26906ee52641eaf7fe45e39417e5a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


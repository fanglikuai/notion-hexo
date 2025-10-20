---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7F3O6ZN%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIEuBGhWa2i61YpMYmHTQLv6PGnUSH6BLXZYPNGOsWfudAiEA38XgOPkd8N36Oe8Bmkzyl38a0P3oXJmAlykmjHCtqqMqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCl86LnEFxYGE2bj0CrcA%2Fp6BysZ3L%2FDMOqB7jJ%2F8U2pcrvA2UeYOlgeftiKtAUmCMgwiW4bIsrU%2BYOMZ9QFI3skhqE%2BIK0ZXQmJu5l%2Fgx%2Bhib4Nlp%2FiQnH9D1DRa0bLJ2SFOt0MjhxyTeWc6v6%2BXWiNpxOAWRVoGMF6VZj0N%2Bh8XVMPzIx%2FzN55GeZnBbMp3gPzYBQDFR7CT7Oa%2FQ745Q1Z%2BWUaz3WON4vKHtMOPElD9LZO93uj9MQpsn%2FUOHbOTgsyoudyXyLWt53I8byYOf%2Frpphudnb6YSjtom9V7WVoi22AVeVCvO172mRRYBosZQwNGoSWYOuu2%2BpA390DrCFgkI0rliKWe32fhTjzyUQALL%2FpbgMF6nDG%2FL1sweCNMNlFqFiEvxzVKsUC2eX5Xcwj2P47Qafi3a7IpzyrnR%2FZmIM75hklIvQykOOgsx3%2FSywojYEUxarVqdV215ImIDaRDb37lBflYUeUV6nutQ8oPPOlxeb%2BaVWP%2FRo5cAwkaOVgSyHf9H8a3dqR3%2FgkovQsTD4t8FFI8u%2Fc99BLQk%2FPrEHko30fXEKk2i2sw8Rkt70KUqSQ%2BJvvLiS503TUIlLvpWxzbH%2FZxMq3xTKwS%2BsIuAZxYjVpxZqs6mdy1osqzdHL3a2A0EVPzb%2BYMLn11ccGOqUBOzQfyqrTlAbI9Au6uZpf18neq6xBDIVG8JMZOwNMY1HJsbgroozMbsGGEOMOliFvE8MG9EzEpY0o%2Fz62HSY2NDUlos%2F%2F7jpq2%2F5lVNVbnJ7V7nO7e%2BfjQNZ5vUdhWqprQtvPah0CUUu0lfJYbQSslG%2FsLHYPmhwMTkKQxm1Omk1G2Do5IDumeTlNh509sDPQbrG76FOz9TlZqshiseTzXTvPHN9Y&X-Amz-Signature=4a764ef137e31c5fa8d279f6295b5069e367ea3abfdad1558a9a21a511597cdc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


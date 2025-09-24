---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWERFRW2%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICX6t4b0%2FFDbdh%2BmVpCsMdwJPs2EIi0YbN32j6LJVnYPAiBxWO2MpHX5iIAPR7Jip6aSfo0yrQR%2BiNwtndCZ%2Fc45nCr%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMI62%2BJ6cox4jng8vxKtwDslWrWtQQdvHQrjvmSxcDMC62L7JWrGEi0%2Fwo%2BLmepUHhgNiA1o%2BBBQP1Yi3sVlhznKtGZB9oavPZpUzCsjxW8zJGdp0p%2F6uX6zOUSpL2HVnAY1t%2Ffl%2FuVn7IkOVP9JUOStIZIbIijG6gYU0MT3X2kjZryXOUQGKjIFqZIMxyXVB46SbSzgJm%2BgXSjzWScXmMItibZ2KU7hxhFWGKDpljEErsIN%2F2Rs6D1Ptmp%2BTDbFG62hLR%2FM5XL4th1FWQ%2BsZqOTW%2F7KaimBeoXnHHC5NVoe2l%2BG6943cL8e66qhir6%2BjOURwNWjS4VSJyCkmGV3uKIfJNJKRbV%2FyslzJorQ%2BM9K6CEG7Qz%2BWmpTNqZyNJFRLmyfJn2WIOnlYBWX6P2Mr9QkqR4UGegPHWkh67Zo3g5vm1mspDG%2FMGS5GT4TVxDdwtrlQ%2BzZxLjVwBtPvsL9BLcD71GhsaHq8VprjZTLoH457xFwo63eW%2FhLjSYevg7bGV3SRnNgEtAgYXXw4uwIxnBBiv19W9HGIu2%2F2miZWdzr6%2BJE5nNw%2BAsOffFXfUGas%2BWxa824y5KwU%2BTP5b%2FFeyE1K0sl7wqLmrqvLZr%2BK3yQbS5X682VIq4SgQJ620H91FOj6L%2BK6BnriEFV4w2PjPxgY6pgERkknxieXicX5f1k0n%2BAQ3wmmE%2BNUKiFZwRRdRwiWjF6PlfTCQdBW3KWoZrFXDhC0quh4AhfXQUCpvCcFujvhqJx3usyW2%2FWOBwd9o2qLJNVH0hv8lIrxB8KdkR%2BCwlz%2F%2BDZclRomN%2BiJD%2Fg7fTz5qKn5NF%2FhRHLHCwnZN08WS30NN1UdaSlWnB5%2FIfwruUzIIXPSUhOqaFHk%2B8CdclmX8xDrtcrbp&X-Amz-Signature=5e888cd628fbc26582da1a5c034a5081976f8bc1614a852c734b7cd56e8918bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


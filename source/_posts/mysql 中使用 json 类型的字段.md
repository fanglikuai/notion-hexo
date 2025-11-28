---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFKE345C%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T120106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAKQojJJR%2Fe9%2FUCJoukUItxPBhTP5iew19a0lH2B94Y8AiAZeWxdpKEAGQM0XRXqRgFZ6C0LUFOFVFA5WsoMKTwPqSqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAf8m8tmG9BoT18euKtwDaDIX4rr8i2rRiTNk9QVTkU7ZSKuZmLe02oOPxcgOEeFmkosN3VhNg%2Fk4%2FpBJ1O3Xfi6ng7Wh023Q3zs3En%2FT3mbx4sOous6duYUykqdzNgYTEBjuRlTum2JZWajCyChZ8E%2F7mftsjf0%2BQuuGYGgUSneoMWKQQTsn3QNP8FvebPR1XDYh5%2BbTfzVWaBZBjNT41stb6BcRENnYrflKAvpwuMqbKOD9hKsQFYlR3eylLHiKIaZrBANyFS0Ng%2Byfo%2BvpuqzIk2tuo0YvXFjyeJ514R8ox63%2B7N0lsku7kh%2Bx2cclie7vJjLVxBBPnYRhK%2FoMkDRD5iuKLEBvN4VL7lMtBz5PDPF4uIBpBtuiZtN1QeIt2pBdsaKLDVRAoYNcAGOAjoxPAcKeOaHdsEafUTfxvn2taasulWlwMq9sZrxUvLrzw6ILLer6dkweyYS7GbT%2FPVn6x6y8znhomppCHkc0gJyneURqWj%2BpX6M5q2mzU7cwwNRnwbSmmChZR7FAC0gckiHHv5LdYGwnMuUFHyJFGRP93YYgXT9tlwFNFg8uFs6JfqTE1y%2BCpivJke6wdCDKywK5Jy2z9OJ4VML329rFyJ5gLCLLamlnHFG9ch59cvQEQEsVxBPpFJFAygowoNalyQY6pgG32B5NULyygB7n64vECyq8Km0gXH3t8S63YgJn2drYcr%2BNMAbdSMjz7s6Z9qnmH2XmdiutK3roBSZ8UdVSeVl1SzMBoEmQKCU4VOT8Wc05yQXMTVJv85QPSmRfoM6mt%2F82suPo%2BxSLzS%2B4uF2KKPcPnl9iAv70snETjPYziFVZTClAVXUmCUDE5o1NAd1HL3ea9PRgrCJtjgO%2B4dz7ZZXPksxTtjH4&X-Amz-Signature=a34eb74da8ace9f6e7378d10186d7afa4a5cfbe3bba64df2a8593ac66beba948&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


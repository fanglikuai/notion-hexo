---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666F5XSYEB%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T110050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJGMEQCIATSk00gEeuXl0GM6QQQdOzh4DKV9BAek2SsvKQzP9a%2BAiBu%2B0yjHrpWbBERLM5nTO5lMPzyJo%2Fyc4K2lPgvFCVtgyqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMX80nrFXwPVZco0eNKtwDtW4548qAnKqexKkoff1xW7Rjm84Vr0%2B02l6KMr6XRMsnbN7NFVLPdLdO7WWVM2uaW8ghlNXPaYp5p0%2BhkLzOcKNwRtyheRJLiqWs1VFo%2Bm94PXxehhFMkKP3JfjTZ48TyewGM0nQocCchZVVjXQIm6ZHFCx6FxpgYqAby9rLFSQNE9W8npNsB8CdSxAKiEebmIwkeEL02Nlb%2FSeMYRK1uatoqzAqAR13XG5z6EYSsf0zs2t%2FsFAo2KLpwCaE94ehZFvsB0AgoIk1LFh%2FRrrHheV%2B9FKLcyJTsgzqKH%2BGTtX5U05kwwATzIhxr5FPTkS79WpelSeJTW4VuproyKHF1uCEUid2cahw8PDvJw12H9wyZXLqng2w4eznYQ%2BHCFdqry0TGrUJCwpb8x2sEBuVZ4Ttcs3Ok9YJyw5pJm2iV5ZI2snkpPt1QbJUHSqOWduXsZdODmEgNFaDA0BF9RFBcjYSe3JH8IJysxba2fj9uxnn1hvPWhVzTWA%2FigLy%2FdjbLB6WSfhKb1EuLISVhDJhDfIyIcc6aV6Cgsj126KMD1oVnavuyBMZ%2FKXeyjtlVBFhGF2G6Dar5vV%2FZUz7Apw4SSSHW%2FgI6UG20tLXbPnjG5BaZDy4aOCNRnolGU8wrY68yAY6pgH3%2By5ff9S7ECtzke0%2B7DfCLfzTbAtNHrwTnvFTlYcik8x31NAPrazxHgawfs8Y95RzuArMDv3fF63qEX40oamQK1b4DyM50rzv3AQyXLgbBnRAUBFJkMvhzH709%2Bn1wF%2B%2BXarJ7YKSd2yVzG5B7fa9fDZo1JCpAlhjjdCDeMmr8alpe08XKYfrxOla0OjB9Lqo5dC%2FGkj%2F%2FmxgWzk9B0LIAU917n7L&X-Amz-Signature=2db0633cda9c70d693eb7a3b137bf35c916c352fb04895ffe8b15ebfadd9086d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


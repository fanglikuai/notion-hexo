---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZP5RGLCK%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGZ1bwyAFfgAYm1mFNXhtHGC7ZPIr3XOZFip870f7ZYDAiEAnaTJyt%2B9q60Lj458ZqiGKsgvuz%2B6ZvTe%2FZUFi1jOE2Mq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDK8E6Wt0mSJc2F0V0SrcAzqca5gyOCVccC2k8ga3heMse5Zue6Z1xKFnZ5As6xvTiO9xoYHKVQN0JWkTCxepZ%2FLvLeld%2Ff9w%2BcuCj%2FSlIKsZZOP3nA78bmjsZ7EO4BFUHZmgnYgsdiGTdohNksP6mmZMVQtEdwYX2tjtNOGF%2BjCpdHeAyleoTgp9t13iEoFll0OQNTW9vCN2JK2nVSgiB3P4nw1mz6I1OkEnXD3SEsKwX7yJ%2B8gosiUvDaoLnre8TlHKePMq9wUafEVbdUbf0zO7Bp8YFeDGPx5ne5Ch1WVhTsIE5qpFYd6nfG1D21w%2BEv%2Fm8tLf67fAYl3nwtHwpHH69ATc7%2B7CJ407tkYnMap1ztiMIUvLY%2BTbg1l21CUHG4XA4wG4Amf0RA9TJgWg8Yqk3H5cSMLGUfJsqglpjcprNVnNX5fv6muauMgeoLD7oPDVsRTwYRgGJYXPCrJWYx2nw%2BBJ2BnTUlFRbC5zNsd788lme0E2Nvr72HAJp5GWgXVIFKA3l4%2BZ0ufjODgDaYW0xTfAYBDegxLxu8x1%2BjDhUe7kOTpiokKMgdl2VHFPBCrdGkrpqnwVXv%2Bd0heXD1na1SmQdUk1Xf0xABT0utL%2B157pv7DpNazx79Ei5zSsZP%2BABoJgKNK9jZ6cMKz488cGOqUB4McnxWis3NegkNNn2K2Px9hIK7qmOsTTwkPhJ89jA9cKDCWtnd%2FFuIKSXUrwlFLEra%2B3LYMC6Z6nVBiMgjkZ9jdH6U5RxhQ2tC3miLGT8kdVCicFfp9w4hXGzNk0xpdqK6x3HLX06j9nD8phIDOy3nDDxeEOUaO%2Fhnr6z2MFfrlpyF0Ll%2FGTm05Fza%2B0dl10Kz7JXOcwMLe7Lv%2Fup2d%2Byfvrp4wk&X-Amz-Signature=5555118a19363615b63f6348f7f36e9810b74223a3742621213e67ceffad9d08&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


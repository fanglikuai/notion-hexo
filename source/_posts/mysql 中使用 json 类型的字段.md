---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SU2GOCTV%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFqhl83%2FjoO%2BJ%2BVWhgURuqCcUUYMcaHsooh%2BIX9PkQnFAiEAsDdipTvNeM2KSL%2FMmPuI0PrrJzIbehqKlsPq0NEqsQQq%2FwMITBAAGgw2Mzc0MjMxODM4MDUiDEVzYSNpkn0OEkwlyyrcAz%2FhwOkBwZdNeIgYCg311qtM%2BSDZOV0cZw7nzAQt13MmHC8wYTIff2WaXBsT%2BEgD5ygWlmpzr2ZotiTCSuqlJF5UXILaF0BSrVS5qX%2BuWINVEZ8gqxs7CuH82xaKvfbSkY5EgWUaJLjLOjy1y1wWEkZOH%2BrYBAZHGVq%2Fr0rpU09LfX1GaGZsQJwTTrANlmLSCyxyVJLbaWXcED%2BTfnYDu4ebOibuvlclVefry1ttHDBFTn2pepPxd%2FBv%2FgwylipdN6JoOfDtBM3yQ%2BSu6kTruU2tegUjWQ10Z70DdkacB4eBLvHg3TlJ5D4kluvRtvFAYMGxG7Yx5kfYXWqrRzhqNbxWQDvoIgOO%2BhS1QmA%2FBh7JodM1ODGcF%2FsDdNULXAkEtSsmct%2FhLrHG%2Fjpa6SKlulVxNZ1uDgrwLVkuPY4KcpI%2F%2BQUVUZlzC4lL51EOmqI%2B%2F4sw5%2BdXBsZltCr4VPKXWIu8fGKmOsaQ3jeZrRhOMdm4vDkniIx0oS22dFv7qJdHK5w1SaNy%2FyDMeNGIu45tG12Roh0d4%2FUR7sfqbESA37%2F%2BIA8Vm3SjWlUl3T61ckU8tirKgvoeV5FV3jp3PhSSmlMHYyfwLP20znQCSnA8q7f1CJuMZTS%2BmX2X2PGNMPu4gMcGOqUBqeLahfVivbpolElVQ3T45cf0e%2Bs0Kg7%2BMrS2hAdxaI1lHH296KcY8oCj7o9lmufHrFQoHmncBZ63X1La3ViJ5QoKxacvGAX1YhVjOLwFuebX3TqWl4e3Z9iiRqEVvU2vB16RYedJtoqI8u67RmC1gIWh%2B%2BXD8dvRvewxnpiuqDDyTI1qrC6998JXnL1P3vgOm0a1Y8Swi3xa3cUuBA3dU9FJfuaw&X-Amz-Signature=33060c7a7bbe1339078928ee640b1c0d7a7fd4943d6ec5e3eb25b96c798cabb3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


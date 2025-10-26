---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VL7QZFT%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBHZiYz%2FAfqPFxZA%2B2YFjocH4MC3NsO5RnSywO1LwCy4AiBgMXFspU5qVeUPnOn0tEDaZN2k9POMCqx2I5aRzXv7DSqIBAiN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2L%2BE%2FpNYyAPQ%2FXEJKtwDKo7tklnLSsmRxsJfY5jO8TBtqH%2FRRKIXq1Sm8A%2BavYznXLKXd5iddtlf0O3P1SlRp%2F4KwMuc%2FelPHM6G7%2BvhL5o1vu1%2FE%2FfsPCTEn1Y9h%2BdJ50XwefY76mD8OZKi9xQML96eJ3aO2qsaybQw%2FLJZ0OqmUKMhXOdBeFbq%2Bnqp9E42kgNCEqWQwi%2FDgmEI8r3BfbISWAwQW%2BApJWQkpendAKdeHvGl98Jph47gbFnh%2BvwzKMUPJQF8di6aV6gd9KVOncL1V3HyG8d0h9f2RjaYo0yRpgnoigBbGQ%2F5LVoUGX04myoFWTcJy3CivdDIDYyMpe86W15zCMJ%2F8mC1fu6VHPq8RVqoG0NjcDocH3tfCXHkuB8whkmUVH1sKme0l%2FyA1Wi9rHpPVZty6UjBqLBuvy6KCVOaJszSy6qsmtxB8VLjvI3aaplLm14i6StXgSCZHF%2FXixnmBul%2FQZ8K8G4mpWCA%2BgipElkVEcYUsDN7o3akjsVEIu5PfYuZHXAGDgLAFgLovxw7chWvQzp64dlPKOu%2FG2mPz4%2BYN5mjMoC7mHHrGdR206%2FzKuLf8IWxfO4A%2BSZ00IcWsqiboTkU0xskpXVMQ9g8rKrmSN4jJQfHMUzlITOxt9ggmsdKJM8wnJ%2F4xwY6pgFDa9%2F4Ak%2BFm8hlGBffXYpNWvTASwCY6AC3Eahtr%2B4ouq5FBguFPNcGRO6Qt6kzclWlOqUILBInb%2FMySfOHaexASKy3QiXJC3xZ1sEL6lvT0GhtbRnaDKj24KeTk7k5gIJQ8UpCAnIuEtk1Pa8QYujFn%2FCKdrMtX1SQ4flrV%2BQQgtypoTOyky426n1B9Yb%2B9mUAFWk4MbtXTJy9iZGshFE4GKdEC9%2BG&X-Amz-Signature=ef4017b7ac30a9b03418e7dfe49e1d0d4e9ee08212cf1ceb51a63611006bd11b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


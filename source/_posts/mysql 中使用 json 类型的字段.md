---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646O6XEJ5%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJGMEQCIBzisrua47pFuS9kSuOU0byGeGD9GYmRd20MRMtJsISiAiBceDLbRaM7VU735lTxlcVIocJmsFmbcpjwCHnDV0IGAyqIBAjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM3tUvf3QysK15CnDCKtwDIjKg6Wo%2FDs%2FYSmHb9zpx2tgF04wlM8EGJDiUEkL4xOVoSdw2khiMR%2B%2FA3%2BLlh19rutL9yYHMCs%2BQ2oQ%2Ffj4ur5DWk8dawgtmFH%2FKk8qdVQXuZQUKPxb%2FYCV446Ihq8WuvO8fq90D8VysY7LA%2Bwo03WWEVjJeEA6m3xXWLMNo00RgS8mTwByGnOENJ%2B3k9NpfuvQsj3Z0%2BeAYx1K6qS8%2Bo04PaFpW7gBX8sDRvmr5bV3XZl5whGBQdJF3Bew2SrMaPgJvmkHA2TwRRFsml2d3eA2fA2nMfC4MhRgUA9PYBMqlwNBDVcfTCEK5wlZd71tTCd7EZAW8kjD2SqD6j0vk89Zd3fb1NRa1FM6b9GloJJQSRspw5qJskx4%2BF%2Fu9R6wht8GRfMSaCAlDIJGCGVf9jAquFFXFvU0kgoEiXGzbEhmKxDTU8zEsq5fxpNBbQHHGYilRqI5BSZraOYwJ3Rd4uArPnY7OhMOD19cARW4zaNG5imU1XwFBiZQOVQm85u8U%2FRLYXyFPokTOrdwgFqoy7nFd0grz98zCu77TXxmYNk23vGl8SG6DM942m2qYN6gp1G4wQq6yU%2FEDuTFxiXlwGXTdKYMIeRFxdbSzfZAv3jFqDJK1QdBe%2BbG2VxswuP6%2ByAY6pgFHkwgQbxoN4NAK9fctXcqnrtuhBY0dpXfojtvD4vBITB3WgkIpp9M2EVaTZRQakqDqwUK1nAYPh8JQYDjYA3aPugg%2Bdwsj3PwoZUI0IvFES%2BX5daHLyEtKQGkivKHRzu25AfnwyRJIfdaYIBRS4O0zTbS5sbhaKNQUPv61O5vXbSymY1td5EuQN9CvSjjz55tBPzse%2Bv%2BeMnmN%2BjUQIU3a7iuOOnMm&X-Amz-Signature=2c6d4d0d5e56579c85830cb5a60c26ac6f1faa2bb52ee636d66945a42c942975&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


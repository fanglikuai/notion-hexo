---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666U3GXYHX%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJGMEQCICHOh%2FymvsD9FVx1wL8wsEbJ%2FHXH%2BTGvuD8qn6I3G6vUAiBu%2BmtIFRDGggiSvC8nHRWDaMRtZs6mFfUA5R8gC2BFViqIBAic%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMGJYXiHjuEmB3Mb04KtwDzQSANnLiPo%2BCNJcaQfkJma0Db73EWFsiO3fnPYnu%2B5bf7mW7SJxVqjPAXOsITyIGGyCL86p6BF385mGLVxUySgQYVzLyIaGLZRHUs3N%2B7lFKVF7OePUbcw1l7Vqq1Xssg9c2hLVQI4QwrdBBqpDIHkwGMiIbOjgV8WZn3g2cjL6KQvGa%2FYO0g7CHzHDO1S6q8aoaqKjgOs2MIaUpU0o%2FtgAYjgnPPQRhGG2h1LrlgfMAptQef3XkNGOpVpL1qaLrW7CoomGWRvTHQgXOgeDw0LxWXNUP81clJpsQFZZhCnDpwl8PG2loz82HrXl3j92w77KCTCdUmro%2FRv9%2Fcjofb7ww2flYLB4pKu9GMoUhedWyYemh3d3PwjqijNL36cIbIuBwUeiN8rTOtMgi7cXJ44MiIy1GbpBPBVz1qY5U8E43%2FUPIwC%2FrzCFQnlPRPSwLA2wsZXmiCfM37DNwCP1V%2BlAZE9Rw%2Fbvectt57g6aOuuUzlBxsPLdRpCQ6ePJVub%2BsiH12Aqrp%2FCTruriMedUTRuPtUGQiL%2BPE9c6VeKkCIBo2FCtCaPJkNTh7J9Qxbl4v7R%2FELAFspensxZCDX2lLwwvIVx%2BMZxRXdEZ4rq7Qe5FUWgATpIlt11rrb0wgdKoxgY6pgHFRw3SPoZ5W0bidnYYu8aEbZ2g3y3Vbcl5%2BuUxgPZxJbJTgVX7gm1g2FSnPVzwk3wPSG8yrFyRjM%2B39djr2jjchfiBbAOOwe1GJS5iNd5nuOl8TyxjVKGUX9HXZ0K2k0HOLLRYtWRyJ1LgCtDbJPWF6ZhS7wRdW0miieBpNYut8Kbf2e%2BHT9H9W2Ilpjg0AwEetGPTe68jY4F9xfj%2F47wM2Q2ChU0n&X-Amz-Signature=6692ad0e648e60e3db8a51a762a94e5ce47c15246646a9928c956d974e28d165&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


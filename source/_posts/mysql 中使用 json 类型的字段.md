---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWLDZ3VY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T170044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQC8OBK8Rl6rzrmd0nARsPgylXzkRep25U%2FBn4wOaQSDnwIhAJnpYvSEo8yE2Gv%2F3f%2Fp4pHq%2Fvr30CYQpIbY5%2FiCMAcFKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw0pD%2BqSDRPqRSMW6wq3AOx0ljdgiuzzb2guxpcLLlB%2FuXi74ZrRZZ0m7o4YQwhatoXDPYd56yagcQ56v8xJasrfboAFc6kVwjYT6E%2Byw17idEac%2BGzlec81dhxOiTcZR00TUgWAN4a%2FIXvfvWnsa2F0PIsakPC5ejpFa3oOwN3WP%2BVJsI0rO8qcG0H8hyzEa8iauIvEJFrM8RoyOaoIRhEUcqyHF%2FAR4txg9UstbfJs%2BteAtGzY9yYONPsrmXw55T3i012PGPqU2TonyGTomyMfPO%2FF2sVgvB%2BV0TQ9T4jv981sq3EdJiKYBfkmS9aeC8E4ghi9CiPh0lRV2zaLkPcbacuNW2E8PIUvQSEKG9FjFcQwADnUxyOwP9cTTdwe%2Fgoanwj3gWvQERESlcxK4M9QDZeL%2Bhy11bpPIBK7DVXxqzW6eZMPlfTKQm62aa%2FE9PLfiV8F31hW8jBD7GpCEDPi%2FjTtN1B9ht%2Fw08tTsQRkudFIuMrvtlyEt8%2BnX94iZ3znr19zgGP5Imktq48XDL%2BevbY3txvbHHhutMwTf84v0FAvvqfuDhz1cnD6thOZ7uFcRe7UN3U6nmo%2FcCkGNgaaoq49uU5vr6RXEs0%2BgJ8taRkve5Av8zfUqCz3i52ZZF2DOB9zOYXW9bERDC9gcPIBjqkAQiNV8rdNF6zWYWp1XJusHnZ%2BjyXUTPSF3X02u%2BByH6pL3XnWa3sMUwUpM7gwBNnKj6yBi3SsaH9JI9bM1Z9YpVOo2DJuRy7np8Q0ZFzWupu1nrAIg4ier8w3LNRImCIo7TxzeGdjJBrphUhIQxMnJMVgNOknNvyrIHKaWfn3CjfHDUoAxKtl9C2EPkDQwhiSWggD2m3fA5abF7DcQHBsCY%2BJkBA&X-Amz-Signature=51a069d79da233a8ba8463d7f04a1597cb37bcac120ce7cd5344d709fbeeb892&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


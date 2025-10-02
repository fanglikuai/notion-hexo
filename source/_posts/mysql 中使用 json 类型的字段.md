---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBF34QIM%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDuqQkNgyBoMA%2BxnZ2SVXwgwvsijwb31f8nfxN5JuVsgAIhAJBpERc0WCuOfcrnf%2Ffw7nq9nlFdXGEUUdAiUvksUnm6Kv8DCCgQABoMNjM3NDIzMTgzODA1IgxL3K2uWdhrX1jbTL4q3ANY33oLJvlBYgb3JCCxaRkyMFRBR4vTe1uooWK8HRAC2F%2FDJoOKXwJaOHLrLdwJ1d%2BZCuJeGBX%2FYpZxZfGZ9L9X1SzKem6ymoQPF4ND1ECU%2BMNEnaZRzxqfmPKAWXH13DtcLevCrlOBGi6tyC48QS1RhT%2BcRL99Uj9tnVXgIVKZu8tC%2Bw082ZnH9a66DtvPlutdQgCRxpcmd0vhlcEtsqEiNwRuzkLWLKoMwVLbWA3Dtx6n4Xu2FdS6ZAKI9jnBmIDcuSfWwRcPqJ9OPWSySLyDCJ23lUSjI9luxgDDZsyiyQwHgQKF5bCKC95zTNSg3Og%2BKnT6vGjM5PV%2FzCZ0IwaLFL%2BfUGhwe1Irnk4phIGx8Ty%2BVZgHs8%2BkEtDI9bKNwCfxtLDazqnf8HHScdPSIBUSMdY6hyk8OLLSHJQ4qkURfWM4gxTpQ4z9hGc58uWLLuFErUWNLkDWhQ53fgASYYI1nbuXxUwfPySoXZHPmCDqYzTYnfu4xazu7pcWGc%2FgqqiZ%2F9TL2jSZJywhQ6k7r%2FQcoHs%2Fwnbe%2FoxxPtYX6y%2BLuZDPGzetlc9TLjhrTButnZkXu0t5gsIC40G3AKY1kz0%2BSytCGq%2F4VN9W5JNHvgP5%2FfMVmRVPWudLUaFJKDCh0PjGBjqkAdArS12jJjOogULvu787x1apgamSNnryDJ0PSYpef0BKWqlEJjERtI2L5s%2Br9O753dXDeK10hw5hBGMP75ecAvYeKri%2FE9vB%2BoRjuC0Ks1SEqgPBcterzt0j114QezP35XVpNxbHTWKRyOKhc5zWbCYwluUAYCkCJi9cCAVdsjIqra5KYqQqBDGYPPtx7JQWUtT4QoXZWuCdQ8PfRmZjhj7c%2Blqf&X-Amz-Signature=fdae103e521e6c3d3d7f65c8109808cb6c834dc26e15ba6a29c79d97a48b08b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTWXVNZL%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T130047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDGYpnGst8h5PexKKnFCVdFca4ijajzcJ6if9K0rNistQIhALQoZmgJ74S1ooiI6risX%2BOhBNhtIvtZ1BlcBvXIrm7qKv8DCFYQABoMNjM3NDIzMTgzODA1Igwl9YfwvGVK%2BRLs1Hwq3APnFakDmKmfmxjntL4GXrktHzb9y%2BXa4A1Dy3PIrquxT4X5RUXoELv51K9w8SX0xAPccHXB6kIn%2BTfzIf%2Bhvauq58cIbTmZrWZLeVrqAjDOWXREd5Kb2kZgWEuPvdoO4p2lKBqPPrKuJWSQnxx5OWxVKfOUh42lZv6bcGkelQQSCfZCAtQjfjdbz%2B%2BtNZ%2FVKQLUJsJlaGZshAP7doJ%2BwRTNIu5cZ%2BlWvV28tn2c%2BYE2L5U7gVHqxSijY3pmiUUL1nld0RdFZQpES5a%2BqLaducB6s2SWOYSCZ%2F7fCvwi3vjK98O8JtXDECIzEy0EmRyA0d5MPz%2FcrdAjhQwSh8TQX%2Fd8wC1BzZhbD9%2Fok5YW44S%2ByuaN%2BtwITQ9D8%2B6c8GRGCcsJ0XcvMM0S8qPJWcSrf6e0OOAC%2Fw06ped%2BiX5cog0KRg9ESlrTIxrsb5tNYD5yGFx6AeK%2BBxE%2BhswwLiHCxnvM8FdVjZMtouivell0qmdqozg5QtjQ9%2B5LNH5koNWnyCpeKxkDbhM1cDHs5vaSGPPJ3XGDgVaMCdvT5Q730IlXAYGhKTVS1cJwEWrzh5knSXOtgCiynJyDt7iiNNOGvpyd%2FXkZY8xkqEGQTLNZaKKkhiqo5xBHg2u%2F8r2TLzDWopHJBjqkAUTmN5om0OT8C2rpnu6OlQ%2B5vKL%2BFuy72kU4y8ys3kAE8jgQMbbrH9uecYrnki%2FdAhDQoWh279op5FAOYjK0Zg%2F41Gix3ocWBhUXO7m%2FatYNXR0z91tys87Gy6pdnkRRCRH1UZd5VMaqpfIaasdhKiP2eqa79zQMGp0zsS%2BMue2jkAmlk%2FiThx7zjxKrf71dvVssMOZQQynLIzwmgFwhbpCXbtrq&X-Amz-Signature=4e44f64ef7675b5004ba664af6bfcc3e2f41d48146152803ebd16365e5283c15&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


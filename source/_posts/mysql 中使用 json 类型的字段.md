---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466REKTUSYA%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T060042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCfSw%2FHwShz8U%2BQLgPPNKzNAdlnBn6AEzl2gI7kEqT0fQIhAIs3jBu%2F0k%2FbxPCaZri0o2KkLKDFScedEbjYZeIY6OvHKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwVkJvrXxNVUoR4okUq3AO0KMTXLFisZIVTFMx1xOh73%2B2QwjZssR%2F%2FtuNgstvDU%2B8wd%2Bbir%2BHQLBNP9q%2B9oYvw8dqQwog0fpRHvkZtmAtIt3fejJmN7hTa0nOdXqCi3k%2Bv0xxY%2Bt3nfHwiD4g5gmP32KB5BdQQ5ZYovZ%2Fi1xwANeIiRWgESNbkEa1QKJEuhew53%2BfAKRVg%2FLx7g2AZ0HtROQtXsTPrrukZsi4tbufgrZXCXN19e7bsorqf%2FF3Ac0ryMuxbV5a6fd1t3isTWQwJsBFMSLe6vEvopQ46jJ%2FzTgiL0Xm2s9QqtOG2Mvk7f1Kq8JGbcbdSqTTlnv9gQrczNsTdU4U5xx3lgF%2Bt05evBFHO%2Fkjm8QcqiVBxTjWPAvCetiONgFZF0g5oqs2Rz22a56SupnVziyCf1fptd25Jol%2FPaz93Mq4m6z7PxDAPyA4OuZ82%2FKhLJ%2FrvNByRwT%2FdkzxOmpEI9okJMnNXAU1toU5MFlGNfpyo1Fdrl8dUxFcN%2FP8T%2Fud%2B8edqYl2w1BEzAnBrfVL9H3qU6JAXQw6ifxyDPrUHNY14R%2BDHjHyq3OpIJaUfQvNYOX8a2R2ffpkK90KwoU28jEJSSiIp0LwPayHc7wNdSKQ2Af3GVPD9PJOy70bt9ABETIBy6zDX7r%2FIBjqkAQTt3Nlfu2XvLMhFdvmeFpDvEQEDZoMeveFgWfM0CWA2aYAaDDWJsTgwe7fSMsvcWjGyMMDMSVeYURKQV6Wzz%2BRiRPQrSIh7UilKxYSCzgRHWU71MpOW3iCUym8Hx4HldlDh12igWCFkkiFzSVIKMKKxfXTeeXDg0KrOvocWXBAxOcPUmBLJriEL94GYRexmnfMmehy27U30E2WTSFGo86SA4vjV&X-Amz-Signature=f3835f5ebc7744a1cbf8ea10650e54ca417cd57e8eeddbcc9b53dd4afd5dc337&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


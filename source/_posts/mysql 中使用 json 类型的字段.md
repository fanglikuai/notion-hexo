---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDY4RAPL%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQDMDLtx82ecpGpxyKZv5H2kyG2J%2BhhT%2FpmhcfxC5CABJwIhAN4s%2F3BSlGSJkYywIhlCqsOq5TBPM6fSeLmeiNAEpk4UKogECMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy4%2FjzeB%2Bhxw9HHhxwq3AOa7eHl7Y%2Be34%2BuLYZmzWrE601R%2BDzfkjOwwbg%2FkeIb%2BKbQKjwBfcopZKo655B8T9BzDxuXxyICgAxopv0BcWnpreCORcpYeb7Oi%2FXNQ3iBxWQ%2B9ffIDVmxhML8rLUY41KlxfGunE74bqXno8DRDpsiWlktx5TNR4emq3Q4BexUrSW1J2kOogkXSyOo08bBiVBOnp4ZmEl%2FLOqpau8VNPwKCZyGyyqLZurFrE1WFDwwxeED4Osc7vt4oDB%2FVlCiBXAokbDcPGMj185cr%2BuEjrsm%2BvRQKspFTC48iopg%2FygIdWQMzhjSJx7Mqf7lxx50xWWziKd5fUKw4dJgYsFMQVErZBBYRAanEyF4iarMpHvAAftvgAb94QOHULlmr2tHCnlyKe6IWhCuzBB6aANEd3tiXRIv7MzcUrmwEj8w0z4UckHZr3JlzsahmZHLgZnsIRTMllvf48MCJkoaIDyndi8WS2XRGtga7QAcidxkNkVzZbcmhrNSzNPlcf9Gvy3%2BTCQG0dE%2F7YIalICK9BTx8rnbEDlOdiCDEoxIfPldfMmcITqP3kfdDY1%2Bay2tAmGnQ6tQwXHFUXRZ37sz51To%2Bv476QeS2kQpH3xoXFjbA2pXbqBWcCm7J%2Bb5s9ANdDCZyM%2FHBjqkASwXJtH6xllqmtuMCEGoVoWt%2FieHkD6qlIzcNM3nUL%2FMe58CA%2Fli1spxT79YSPoATm2qDRtbEv6sS6JYWJJWTrNUWEiBRW9t4kyoY48R9bFDHyE7UFQcfrmzPjBi93t%2B5%2BZxAf4p17NRPRjEElyH7xIkAIUMAzqKZzAtweE734pCTbeBtp%2BftcHFj%2FoejSDGOaQMNVV3AhX9v%2BF7GTgwJFhvY3jK&X-Amz-Signature=574d0915cd5cdf012443a1fc2936a8a0ef2118a5d0768cada041b9dadb31b14a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


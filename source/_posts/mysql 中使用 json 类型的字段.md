---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPKWOVDI%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB2kJXLTs8ATIfjFmMV4Qb3crn6bAU%2B%2BBtG8Yjs%2BbT6%2BAiEA4gRpo%2FHR0BILkt%2BvnBs%2FyMh3JMkoEQhlPAAphi%2BXnL4qiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCjZjY7WfqiLVNrNPircAw8dX4yTjtfJTm1Q8wKF5iwNVZ1oc0%2Bm6fPg9UaF5V2RWVKP%2FFqmdm%2FouvOHhknMy1ld5DEqH%2F9VXGLONlyEsNsJbfBX5R%2Bp4jwv8YC3lzBFFxk2iG6bi6GujsKYRwenAi4cJSadFX4NIc1smLK1YcHhwJTrtmQ0OxM8DJuSEwpb3BZtCVkbbPXFmr8EYAVasTIJc0X7S8BhvY%2FAI1MtXLlgaN4MQGWAZQbioURUy5YLegW63nyP8GWLTPLka7s5FriXrxU%2F%2FrvRUHDvvPYrJo%2F0IoQSA86KBAJhOt6xkviQZsqjGQT2a1o5BNPktSNojrf3vpw4CkWgYi3QHj2VKUNPDR11AJYihI%2Bqvss%2B1UlRKqYuOQgwlKhLc8lP%2BGc3PWRgl0OQ1FE%2Bp7X2PBBIKbhzLmh33zn%2Fp8O3vF%2BveQ%2BdJtjPwJCUqI9C05n5rKq1Ilnax7kcjKO6%2FYf%2BQq5bsSPgjslJUxN5u0PiQB8QWtRPP8n8CEAlaOhneqkBLoLOfPgyU3LyIdkxXzn%2F46SaQLOXiu0NzBnEvQMDtb0qqLPUGErye1gxzqNuy3Pwmusdhf2W3hD6ZaATa232uYRe2klHtvPd25EBKcSaJ5b7MbDKRRG%2Fhar5ghtEMzk%2FMOre58gGOqUBolU8PTyvlkqY1ewI5Lt1d%2BMyBMyDRgWmnsqxrH6Lzhij2w4om6JUftVmGfcSPf9eTKm31Xkyjx8fjv0IXdIOXLcZZHhRu599O9fTzVCPSmFjb%2F8afl%2Fi1LMEwIXsH0Fr0Q5cdW5Urz%2BOcdWLokx8tFcQRJlonTI5CwITp7Zs9kKCNVNRTVAZ4bKV1oS7jVHLvbBowtkCSSWkhl%2F03fxubBMFYCfD&X-Amz-Signature=b06b621b6d0fe081df841fe9b26f58e7383c41e92b7612850b8f2e18fddc2cb6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKZ7B222%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA09FIxy6AfrpnEDDZyNhr44hor%2BP2GmlVMrbWrJ5PoIAiAI%2FBf8xcwO0lGCoRLlpTaLZDVz98w1%2Bf99OXyxiG%2FU%2Fir%2FAwgtEAAaDDYzNzQyMzE4MzgwNSIM6eJE%2FLQPNiy%2F93C7KtwD3alpR9PteFMaKCXQClvvMKnu2oIIxWCsEnwyzFcoBgA1sckodkJ%2FSqrAVMZ%2Fqn%2BkJqBf7IcWYNEgtQwjS7FpHe5%2B3Ednt9tEgUFR3c%2B5dNzYdFX9onUTKNO4aWOs5NDotnJ64g1B2Y%2FwQ2Mbi4aP%2Blt1KZiIn195vioy5eYQUBNxqp0wiNb9IbkhCxskFw79FgVIZUZmioLwlse2S3H0acva2ZBsx%2BJKD6653HSrbHT%2BC%2FrsYa75L9V5tXv5JHc%2FvD0EW%2FmP3O3RuaohlmeIGZy%2BeJRFIDSxWsqulUnBYoRjh68q7YpBVhUWXaedUN%2FIcAG3KLCLi5ZhZX4u4TJtOu4cq4da7PFnhTpR4hyF1X1hkzEKIGe5iDhqotUz%2Bv9CrWdWPhlRefR8VilKSEJwS2k05Hnc%2FsG1%2BKlyGk8GDQ6LSdPJe2mT92jtwKpuvfe91Ji3D7GX4vD25XKLbthn1Jv7%2BZUv1guw3t0%2BPOgmQK8UPZgqwQL5HhsxZlsb4wHWFeGDMfZj1wYvmXvYPcH2lAcGuz6OpgoXLoDxKP0aFLvlJkuEgTFrKDgNRKdqmtPIneNiDU6i1i58gUXCpnJhXeFWZvVOC1YZRvEml3wDPAD2pNNWucxaTLN3EUQw17iuxwY6pgFAFqP7ZdWNWbKEUO8bvvwmY5%2FoKexnkr%2FCs7DY7YqX0NcjD3oSHTuKJpPsCD6ygjkPNE7%2BvgGuBkeMv4HV4PYglKTjFIcxNhyKpG8kdgS33%2FZmmrDXHxR2MajOmUaPnwfjEYb%2F4zM9TNxlJHn0OqFTf%2BHNqNImI26FkatNGCJQZG4JzwvPPV3dBG2u4zIkV1RN0jdwN9TAQKIcZ1DJhVqoeJ1MUZaw&X-Amz-Signature=927c55c53b6af4d47145602f09cc938474f72dbe5f84ac22fd7cc98b812b876e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


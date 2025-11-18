---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMGIVV5Z%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBrfDqUC5G16IhIT%2FCw6%2By7kziU4ijaLGaoYG6%2BSJR1KAiAukL42ohyrkDWy7ZWSrSmoj2Cdhlk7mB3gyCWQO7xn3CqIBAjD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FaNZonTpVw27HpXsKtwD1Taw3JYSrzW5AGciC6cYHv8EoEb4oqlWBAxMcc7wGXwgKl5Ea2vpujbt3%2BnjkoayBO%2FHKalwNQrWd36brdBotF7yukGCVqNlGHgMdtylAF13p7QfboXJTwhcas%2FOpMQ8Cr%2BI68hXGNWq5ltr%2Bn3s7G24AGLn17V97hIv2Kl7wA59IoJU5Md9xc4ckL08KHw6CV8jKRtD4%2BxkpYq6m8RgsNaDz2F0Ab7lZqRdM5Vg0BHrhZNscA4K8CixwyYfAg1c%2Bt612ruDKGVYUFUIR92OF%2F4gL4BypKqkg%2BiNFskBs2GR4g%2BugaEpPAoKSIPG7mK8h%2FBbqW%2BVm1HVVVu4oGeerndZm2v2D8k5rhW7jPYoA7YlfsJOWuNvBP8%2BzKntNVjLDotXU%2F2tnZPxi8rQiW09QpOgS7LXu3rBaEAFPdw4ZyC8sNoUEohsD1CRMzeViME6k8psnU7%2FSySSo4eh3B%2BaAB0DQX1BHsITMe%2B%2BGRrcKaumWxZ1mkUPUexySl5g7hxcuFedtu2BXb377IOOWTlvXG37x8xyj1OHoq17Ysc62tY%2BrMqYBUq02PFMVz6L3EaCV9kW9gu8QSV5ZHi5YvXBbsECCD%2BG7NTV3vqRzmjJg3O5PAH6nIK%2B1Ze0v6Mw3oTxyAY6pgHxB%2FuRsCyliOQulyT0s6GoiAL%2FNnY3PVjPQqhx2gOkXvXCimsY7VBKgGl%2FvYje25oUIF%2Fm4Jd8KV4MhC6hTWcgm7A%2Bp8X37xyC1FLonG6a%2FsmoArJbcWvcEJ62I%2BaREEJRPMWKLXCdu3UQeDkm9mofbIXdIPRJmjnmoRocfFM8R94ZNMdmw6VYiS6r1z2kXuPBc2sPYLmRJT8qyU6orL%2FQmFTOWntG&X-Amz-Signature=c6770d8c6466efcd70d0b044094d0c8f63ed18fccdb687c3e68a2601d67bbf46&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


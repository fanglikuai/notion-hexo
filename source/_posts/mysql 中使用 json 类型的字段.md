---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SAJQVQKF%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T130058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJGMEQCIB3RspOEyS%2BbQvou9OyBdPxvyDQBNRb1pTJVP0p0WVYaAiADHb0Q%2F0Nlv11S9Vt8d5rOpNt9c3OCl2SqSzJNCTPwgSqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJLr2IJ2xzFHzOflbKtwDg5p%2Bt2voxJqnmVa6X5VAU1JHRuUCOwED2H%2FOWEMDRKr%2FGCQUPCip133ii4Vku10stO7d8TB6bvfneDvdba%2BuxKvxGCjc5soV6tSoWDnxdJ7hmHQWnQCAFhiakW5rIeeIi2nVs9rFJP3ULtUdjt%2FHLoOblgkAd0ZiajUH6wVDBpJKBa9MZuqQ5jQdGviDS7hkzxnRM5tFTCyL7GDuI9wxvo65UuQW6689SGsCXv4GIzxASJYIpiaAXU%2FhI2DHXoCjE13iaWUGCmlU04%2Fih186yud5Ii%2BGNhLsePeuGtrcbnFm%2Bo4XlOPIu80U1N0BRBLpxYVPvZnnFV%2B%2Bn5fxAik3o9aP0ObB3FAe67bbD%2BptB4F2ckSQhpDVtshmRnZ6okNIqIX%2FHDx6uwfnnY4828j%2FVakhZIOvQJz9tfYw99ER5IACTwHViLoZrc4QcLmoHl000n3VSOP3tzGQ2YJWiI4%2BZOB%2FFXh4Th9P6kPnqAQgNcAYZhUa1IFFUdTjXhL1YatCbcPk005SJQq8TGaUkwazEiTRaQ4xnEj6W8z9aR%2F0nvFVCm4PQ1CIhE8lq4ztfF7SctdYihz0%2BFcmzhdWY34WnvQ7xkawhqZb00LB95xrMWtWqXG7XxW9D4FCiGIwg7SNyAY6pgGTRreL4q99AMPaqJA8dnHjjcCnodaBTpgcBSGtKXRbDFt3u1EVW%2B30Ta76XVGG5wX2GDWMdHcROOwyok4tl22SM5DwrYpHMyKDZ7jc9npD3IHsRmcCd5H2k%2BJ%2FqvfF3ElUWUmcDaqhUwU7%2F7%2FryZJ4zbQE5dmVwYWulPgANSFCNowTxjLeSEg8aXO%2FDBN8XNvdJweEqVkLYBX4xGajMHPVYkBAxPNu&X-Amz-Signature=4fd93a7dfd17d365f08a0971d44656d6faf327b71c184fda6253914c10bc250a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


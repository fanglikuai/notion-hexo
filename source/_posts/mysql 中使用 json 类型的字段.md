---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZL5FL477%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDHAo1GvhdhbkL3mwgMrBl9V2UsqWct%2BwnMsZTGTdPiBAiAXHN%2BXzRQRSgvptoQhPJJWZx9vhToYWXtaob7pKyYuKyqIBAis%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhUKWIPAI3jRVOa68KtwDJDAsRopVAwFr7b36K4ewU0CJK2k87CQ7zPpvp%2FUR3%2FwriqERlQM4N9NLXoq7zpzm0j9uLZ9v8lOapBn6%2BZvY%2FnPwx7cfxBHqYWQAds7P6SulBB1337iuwMVMw4Zl%2FB%2FwdqXR6%2F7qht%2BO2XBKku8LIc0vJrkIMgY%2F56GjDJ1lt3F4HKKZ6f2vP3g9b98Ly11Klfmg1%2FbcIjOa0BCvtcarlkSkDQ3C1F0YygC%2FUv0t8pPTS%2FYHd2gf%2FyrxoVpV9QxxYD1IeKO5APKg20w46Reo7z62A7VXKkx2o4oS6XbqyTq6sUPa5gAB%2BXGZlZy%2B1paTVipPAvON90d2VU%2BGiCBT%2F6J13gykI%2FdrgpwGO5CAdIuU5Y3qjti7PjuEEcAQjXplrtNC7smWm3dfwv6M1tKngbfwWtHefmiR0mwzReh3k3hkPuGsWbukEfCMMrkuoaVfPR9D6NWL7iegpozcilG3It6RLMMWn7zWN4I8q10jSNwHjoPKq0pVxUgqkVAZBErTKnSTrYIerCM%2FdXgeJbiR1PK%2B%2Fx6VxEo91TGVLFrfHY%2BV3DIU9tFv2%2BGrIf0vtwNIbo1CHSuqw4rqkfnvXnfr%2FkfYzjIXmFT4P1Q6hRf9wcp4HhUZaFjFCzJ%2BuUAwuuSzyAY6pgGH4aeaUWXlcOfp2S0T4XDQ1%2FJKBs%2BbOPe6naiRflzdKMu9DHbYo2wvC7ePXNK5RSQz1YVsCQIaelAWNT0wuQYfTnfikBN3dUpcYVzLryzE2O5IVULlR0aegKeuQGlk8RMpVbESAQ31u6pN0Q9TM7AqZZWQJacmyLzFlgWprxoZWVUfBtSZIym1EXWZexdSygWaqpuUN8x5Yw8tSmdkWxegqO4KX3Fq&X-Amz-Signature=1380caf71c76fc7ba1556efc5b3e477942de1b222debcfbbf5c0168eb99a0ece&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


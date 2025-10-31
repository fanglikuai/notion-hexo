---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SG6DYVK4%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T080040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCU0WKfi3v8KiLLndl5aodNJ0Q3r1z%2FFlNJ0e2jGBUn7AIhAKARVAI3u%2FwrzVhRWdTW3Zmf36hRtZO6Gk011V8xo3BNKv8DCBEQABoMNjM3NDIzMTgzODA1IgyE7I%2B1AOuHkb7LBbIq3ANJ%2Flb2kJs%2FSw3Gev6mtWc2fUKaa4VUEeSu5etAhE1%2BquYEc5tVIAPXu6hCIATUU17oZRmryJ1gvsyVBuxOZ8tzgGacqn4xsciaZoG8lZsdqP%2FAEN8Wm4Ym%2B2JK9ErzWndrmVtIuMrGCabsflZxL5K%2FjCbjyJAU3t%2Blc7Q71OEimOBw6lG77WoU%2FfCJtzCyhK8Gn20qgyM9YTbyMUlMX2sHcDPlzI04JpM4OIvyH1aLivqC%2F6Qc%2BAtKf3%2BRKnejvRht2etBedwa23gHmUxyepglbCl8srYwkHD%2FQuc2U0DJ%2FAW4G0n89%2FPz9CAe0jyfQJn7nkkRVlOidNYSqLWf06Ue9jPMe7FnKLzWCmbAwnDLjcY7WzjpqjT3eR75phV%2FgwF6roNhQFJl8TsIrZxa%2BTXc4tI4FDuVU6nrcHOY6sTeovOMXoooP0DhUkVKuPbWt5aFHp46%2BficF4r5gy96qi6O7ip4EmV5MPIsYdFStGsLFuAgap06QUUrauHZ18iRd4HN5WU8PmuoxpQ7sgZxBFEurCB61rthOgzzBF%2BFYc0tsnLqwWxQ3rC%2B7U%2BGtdDe%2FWq0HdgkDxGZY2APOd28hs6ZYYoysSki9ecgeOer1ytkhQ%2BJP%2FOG3GK2tKn59zD3x5HIBjqkAQeao9WJhrlNCrlmzmpBqExJLE1FHI5c6RWdPmh1AK3SDkv93f7DnFSbV4faxENSNfs8b52dgKXexgdMkBeKHq%2FXElrJpxwpXOKKSjfNS2SFbcyELxJNs47RRLSvsBKmzm%2Bb60gYzSp%2FeK5wwrlh5K3OO6WoPmffvjIYwHq3iSZn2d80%2BLJaF30gvOT3xdylv1s5DR3xS2LcJzyl%2B21PDlHE4TNA&X-Amz-Signature=b59efa0ec8422f8654f8482af7552f81616e488d0a55f9e778d9139c44322516&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


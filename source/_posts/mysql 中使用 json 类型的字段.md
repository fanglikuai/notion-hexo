---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAXZZ7BR%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIAQk5e901S%2FLMTdK20kJyRCtjivt9qKopHtPl7NT9wU1AiEA3x1T5UUaz1bbRVcG%2BXCNPOmKQZwIErqfq9Oc9Tay7v4qiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGIxcbM9fF1Qbp3rpircA3CiZtquB0oCJDKPY74GMg1BpfvJBIgTRdC%2F0kj3ySEAyojzKIOOkJFzncc4JN%2FpvqLTaVQNJKBjQ0CPtpkkWDogxj6AunzVqHF0zkJyCk3%2ByMrTFreTR64Nd0LiU5Ln0V7H2bDCd9bpwXE9HCUxIYBgfo%2B1%2BxyuQAUuZ5vq%2FLtuPc6pnIrE9zOvdu%2FD2TPbZT%2Fa%2Bpz5VKdXKKfWWiD82L1K2axwfEwkGXP8HlKicL22od%2BBZxJST4R5Bwc1Lhcvo31%2F7ZU0Tvc410XorNiZthLyUBdpDx5%2FF4%2FhR8N3ooXoKbUnMWVcEzu%2BB8Ij0uDGXK99Z4vWd3KZ6iiz3wZJqfQxTxcJ8hSO6K%2FXIetBD%2BI%2BtAZI4XQFD8U5NSQtZC9OYr0d1N%2FeLCxGic%2FWVcIuabxev8qRPaPtbEnljagHrrz2CH%2FdDqmAB2lU6q%2B7E%2F1XQUam7KKS9ypbLGejGR5vdbAc0wFkUPUfnBMjgErk1DaPkSP34flHFAh1lBO2PLVcUxq5TNcn2eXSMhXnrYzz5kN3nRuAa9GjQp5lacYapqeaN6S3h%2FNHWfo3%2Fe4NZwMzAI1kv%2B9p9mhMpACWBl9DtyxOX1mvBulFS76rzuJ0MDyHnQrLEHk3Pq%2Bn1KvTMOaAw8gGOqUB4FDLbg2zyEHBeKaDyIJ%2F9jT6RB5UbvLVLkXP%2B2aObaCXZACg5Kzqk%2FfbjVh0NID9el0pcjAN6qfHElx2jwHOa%2BRyvqBsPpms3XxpIcmJtv9d9bcFCtkGI875zoQAK%2BClIEPxE2BmXkCRPvmt4jYj2yqLKSrBsvs7c4v3iOZTJ7CUYgsMJBlWUrqWvE4%2B3HWWRqUmQhUO27vGYUUoJcKFANLMbPOm&X-Amz-Signature=a8e74c8c3ab85421672a408e35357d088c908d58529ff94c43edd49c93cb636b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


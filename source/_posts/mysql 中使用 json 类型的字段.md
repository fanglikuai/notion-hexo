---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BNDQB3Y%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQCEB6TTLphLVgwdoUkx5z%2FkZ5ChRGZ7%2B11bI3n7rhmLwwIhAK8MX%2FpDW1blkCwWRjb0IkpnBkWhUPrgXDocqqZIutiZKv8DCCsQABoMNjM3NDIzMTgzODA1IgwQdt0zKRC%2BLLqNXGYq3AMHhLMHD3%2BDPsZc7OoNOH%2Fs02RrQdrm3LDenPOSslzhLrdn%2FqsZRpfbJlhHXSdxGe4fb31luMO9OedzzeBdnUTr%2F%2F7%2FXFN5lLMC1IGhxmhXQUcmwjNvoCN32JZy9b6G310k4nDvQKJFRPLFV7qQrVkTdamQJxIEV1%2FNS0ihburJYW75XhUlFH2boqG89YUNOFUSfUIyCN0k8U5SRYKs4L45DraBKyGntZH0mEIUATDT2hVxMS8ckgmFgMQxNeP14JM1vpTKSEU3Oexf5VDuOtjJRcV%2BHmYMBmfYj89oWIa1sHS8IiDQWj7LjE7vi6%2BBteVvjwHZoToSfpc%2BMP31WK1mk6bxMl%2FCunqGcoAHAhgvKBiDbt7yoKKwMFSxmgFKpS%2Faonam22Ho6W9TXebzKVzWelz3XeEy7EmI0UG6d7m1lSAWuTb1Hsm169VQA5SD7n4ldYbfsKGYuc8DACxCKSaXkyq238jsbxSQJJ6dq%2BM2Z%2FyTPUlUZ9zShfe0PFiH8Apr5Lv%2Bw4a6er5jqkNF%2B9Q4R9%2FsGk%2BmJkoreGh2TGExSTwnCIj%2BIIdWBtEyLkxkblctHUuJDQoYkrHcCFXV9kLLxAvBI4LWtrtjQhlZTn50d%2BkG2d8InXqoL%2BdPpjC6zM%2FIBjqkAXfDmq5eUNVV5%2BgCN7Gd6oV5vR1wr1MgXFXamixyC4X80BH8fReh2FonvjR5aYqi5ZkQUgwvs7wHzIZKZoab7p%2BNy0rzl3iq7AtfkBNgj8fqDltwAYTs1vkjYkIAK5ReJWPOFeH%2FLbk6kItfjxMvR5UpADRcLDDmXitJCu4eRoh9ltFs%2BGYjzyxinUBuM5ogRHb4YDFvAM%2Fo3yZGWXeDJAEdRWxT&X-Amz-Signature=beaa55c35c37acc873baedaaf297ad9f52724b25fb791c613652b9a34350f2db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


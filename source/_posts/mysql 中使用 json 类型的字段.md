---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q24P64HX%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJIMEYCIQDH%2Bc%2Bo5gKh2Hq0pu5%2FGVXnaDnvmBB%2BP1wZC10uS4VzeAIhANipd%2BauJ%2BBu9Sfnbr51wBMNXm13uYQT53%2BrkyRcmCOTKv8DCCUQABoMNjM3NDIzMTgzODA1IgxhKrCnGxFzhRYMIsEq3AMtajddmhz03TCimcn4y040GhZ88QCA98wkErWQ%2FwBjh4OF4S%2FalgJ5BruJurOeh1tri3lJ5OQFD7lMydfY2mas7XOs4jtNjzzwdUlrPsrihtvTayYYeTQ80urTssExHJxpwzDBDtwZmUq0yHWBWaHJMffyWVJjm4ULnJWN6t%2BUksNmQF9sifnILRm0l%2B18WnfmAA5iv%2B9Auo7rjPUWH1x9RcLsBstIQ8Z4UqVgu1DQIWHEuX2E4F06h6ZlzE5OnqeY8Dl63kDvtLNQYX8PZLtL1ayeGvWdA3Go8vzumfox58xWgbABwcq1IvCEuuk8MPPeNcnpsUb76euX%2F18oGQnIgt51R%2FbrL3n0qtP%2F5rsAd1RbY%2FWj4C2Y%2BEvBjCjgnhFQHPCs0G61l%2BbBij829ixp1aTtBUCNnNvrJIIWe2uR%2BH%2Bgifo2LXaZLEnNvlxkRhKkPFAOSQdDmnZFUC6Bh0y9prO7BQjBAYY5ongCiXM505CrCaXAkkMZnwJRmwoBe3cMzDhJ5POZCs%2FcWGyReIQzLBnbV4QnHjk7B040UcEDmyuKfm%2FjxcLVZCua6TCmk0zdFRN6JLhwfAoxs4nZgzlPoByCUFgGwslG8SyNBePHCpSbPsVvXaj1a0s%2F8zD0p87IBjqkATnS%2FKh6npjU69aUO0OiuU1wcTmKZbz4J0kdr2z1EtfwNc50Wpky6yJaltE4J91vPIDUUKMthDhEWnFno74U5V2iE4omVB2bXt1CeCFD7KBbgp9cTXitg8%2BUIHNqIwQazD9mgf8%2FMFAoamCTdVADMsc%2B%2FQNV8stWdVooqQPRW99LuDn9TN8OpeQRg0qpct6GiHL7nIegmnp0sunYyCRBTcDICVc9&X-Amz-Signature=ee751382367957ba68b48ea8bae34c30f4fb77e52ea00388441abdc5d079f10e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


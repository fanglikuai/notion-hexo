---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OMN2CBK%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC8TSlDbJtpa58omMHsbMPKoUQlEVk0qO7Z9VZ8Qxid%2FgIhAMRf94t8XeUifryoCnXFEQC4XSH7k4eHoulFh7A1m9CZKv8DCE4QABoMNjM3NDIzMTgzODA1IgxWhEdYUW2wbPDy1Poq3ANodHqO0qhlxlx69RwHiX1K01Cu1fORnnrzNIVz43exc%2FOPB65Fu1zHE864cIC%2FPwP913e%2FT9BbelxBz%2FO1xIJdxk19ckZM8G%2FktgMSf11c811O3nVvs65iNq5yGGuGxCPuLWnNngJcSjQHIGTc5Eb9E%2BKFVkycXnPxtoDQnn2CZSHLbMqJHs8BagJIjqQuZ4K9ErDWR%2BlsThYwuG2sXb66t0h7cenboQOFGqDpiLFuDxsYRVaqJ57gTwUKy0Xlya7Z%2BcNGbk6HnX7kBVmfFMEMAO8fQ7xXemcwqRxx%2FlDJeym4ScW6mTZU0O6XzMJ455IAX%2Buq%2F5Askukqr1DYpmYvLqgQDvODPTJzip6x3vRCeOxWc8WAe2ovUvWaiHSySCLzEEvPGwjRo4KGRxZDokcxBrndTptFzVR%2FCbCUVQMx1fCnyjDbmLJA05d5QuxxgU1zxzPXty0h6RXUnn5yjiKWNckkyt6APRPFcAj6NH5%2BloOnupeXkqaqhf4GcMlCyrIwGO6tSUTQBN%2BWC4KVFe0y9XXk7FU%2FVsvp2%2Fn4qnOZMq5rD1WGL87QZ1eJUOSuDrrK8%2B4prTUtI8umr0%2BZJGa2U8Rj%2Bm%2F2eY7SmZIqgWgaoP0bhxUy5eRgO%2Bw4fDCV%2Fp7IBjqkAet%2BmExmKQlnsrDVaHPHzuJvCrGHQlntNNFKk3GoSLVxV4fdDyN0tDD9kmV2oyr141%2BzKd9HoU0I2n%2F5vxBgrBdp27V1Yu016FMlZG6GOfPWBj9gIy6gLknRP4UjlPjn%2FQ57QnXwz7pi9syVtBcPRYb67kJje34WsASayVRRaJGWhvRWsnMtuMRFK3948Ee2Ljr3OiU6PAT2Lo3T4Zo3uf%2BNz9Q9&X-Amz-Signature=c1ff5067b6cab8473d01a758420aecce8e7284adc8d94f478bd2e69932da0250&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


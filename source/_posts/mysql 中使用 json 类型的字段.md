---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5FBMHNQ%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCqC%2B908QnUde%2B%2B6UUEd4zWUD0Yj4Px%2Fx3Hoi3Yt2gpIAIhAOiHnAwiaueknly52ZUSXTdMLzrj1eL9sN2ExUIfp9pMKv8DCHoQABoMNjM3NDIzMTgzODA1IgxMZhHmLUBCogB4%2BFkq3APNirZiWjyVInFM9hYS%2Fn3J%2Fb%2Blwt0SeazCugndICIZwWVw0CmZVQp3GdVsUpQ2Mpsb2Q7nJjcMPi%2BxBGurTKjfoZvkxImsjwCw9FsnVDYaq3FzkanLqw04INaQuj8KqRspm5aB3NiN2Q7BXH2q2%2Bw3Oxfq0SZ2Eh%2BQxlVj8MLDpJ8UW1K8tGzxMDgSDZWfVNs%2F2%2BuP%2F5sZGv3oVoonisWEXNLfLqvmOK5rLbQdHXygNLuIoeAahTNyXAN29MeRH106Kshdoxnd14WFBchM9y9Szn9CobyiIuLUdaYc%2FRRTHEed8l57F8cbZayvyEDfgTuzLKutXQwhT7yzVbDLrwHWVvxaqQbdoIkqX1VCWXITZLbkpEq9jYKTOChuL0B2zIZznfKjKn6ggMytsIy4ZvNig%2B5LVx8XOmDyHwb7Nav6N%2BjD4dwwGBVgDhF47To9UkAZlj0Xu8vP%2FqShv6FziP5wDhqV9ZzmtvjF3Vpso%2Bsn7B%2Fe0PQyPCEvCNZbEudnMREMmC6WCOfq9WiPc2tBtdx990rDtuTMCc3pwGpnyGogYhawB9XpHGkFcfXeMbQ0kvLbeUCsvjSigGf72bKRTi5STHkyCTun2InMw5MNPGMix%2Bae9ELhOTSFYR1DdzC%2B36jIBjqkAcTAqjiCxGaivj%2FQNvWFcLEHOgbj%2BxYf6NnciFRcKishn8jmY33wMsFz1UncEkLCYYRZp3fakG0Uel2pKLSrV4exdUVscUaVRLLpiQSrPah4NdPJG94RUFOxPO5EplRIgsmT8uHYN2JMsHVCJ0tkKs1BE4zm1MF2%2Bl1f0wgTFWljNy0%2FGZW5vT4pvCqRHlVOzDXryRU8qidy4zDWiLM%2FA4cK%2B3hI&X-Amz-Signature=366d1acae1b524815cd6e9b20b8af5d46d7d1e95ade653b1e91293bb0e2a45a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


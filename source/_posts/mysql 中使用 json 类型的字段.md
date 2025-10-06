---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDBEFVER%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDw49VqDZid%2FY4w3UOalarWnTxo26SKVgc3BEHsPHxSpAiEAz8HjfoH5SvfcEWPIexvC5inV3h0iy9koYU5bzfbs7AcqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDfvXPFXAL0hOt2pZyrcA7HLCi0B6IJPRtXLBQSGHRoiVY7nCN57fWoeBVyEXW8bxN%2BgurPMC51Rst%2FH8DZ%2BIC6GJgMUvuY3TnO8yokQPhQdb1qLVU1xSU%2BnBEyMX4tEqdUw5xrdr9VaRkTyrjWDfLX9u%2B2ooH0%2Fdw8IPLBpB1%2BtylH38GAUXTKm8qVuXd4fOqvdPIqcQqYMa3z2d%2BaeumYERZtUzphElrBzQH3ohmz1zPl4jYTgz72EL3FrPPZC1vzi%2BZAtw%2F955bFvvx%2Fp4pXp6BGAdnPRYPa3zq6PgoJ2MHCQ39SM6Z34zo%2BECjoSF26l11Bx3kcbYuSs1R%2BgpUeHiGzvWe6mJipWusCy2TmdjkAMm0WS3i9nrhrbe7I8rvOKUUx0qIeXL02hyEG2EJwIAIPNyeAFFKATDq3ByCs2fisjz0sHod9UKTd4C1roGmio0z%2FVNqPLDCG4cfMdDBe72Cd5QU%2Fw1Kc0a3YdenJJyR%2B%2BvuKsz8owV5LatVvooQGFJyxkAlvtRmfDRArlFbKKMTtLTMLQjaIKVytHYtR5%2F%2FdCZYuGYa35sz9O2Lzp7eCfZfWmK1ffljRHXWODIeosE2dWIV%2BDfGRaJUag5UfxAzDydSmDn4rURs%2FvZlzflFndCbGp%2Bq%2BmgfhmMPr%2Bi8cGOqUBR%2FL6u7NW9cK8CDHjKec6umIGm8S3Wq8Y4sne0A780uswrYrKSfuvUHFoCGVO9DhEHbryuuvZVdaKknysQIE78qxTPA52BRoGSQdl%2FTwzMHj4BkXJb2Hk8vRWUFGfqlLEcA5cvDhzn0QjZVCaLYmv3z1HRh1dO%2F0E2Mx%2B41xzVc5bFvNhpfnsnWixXUPjt408SjMtoo%2FtzKUWqxGtPFtdfjb6jEPe&X-Amz-Signature=b0175911bf3cf4e8d822acc1b5a6b0edb58f6e3f743d512c6e190e826bc3b632&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


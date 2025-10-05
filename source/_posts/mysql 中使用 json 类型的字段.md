---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVTZ4AE7%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T170128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0EMtZ01w0jAfZLUVUZBQQP12JwbcvArN9oHHhVWWSjQIhAMjxg44UskBQTX9nW%2BtXY6Pa1CmInWLZjIZohzBV%2FaRhKv8DCHQQABoMNjM3NDIzMTgzODA1Igx4mBP9whf4%2B0SwmV8q3AOPbgX30BnQVpUOLVS%2BDnr1YPRXKizRJqGcMLv%2BzyQzhNcrLzSR3VYWO1LwvnL9cHxzB5fR5gyffweWervcV4xPaMySXUqlNWbKO%2BUe1dvMBjYV4FwQRefLZ4fc6M2v8XZt7iir8ApDG824yWi8kxznBrGKvJrCR8RL20jUtgxg8pcby%2B7Yf1lAONO3waZDnYa7JU86C8OCRctFDGAwZGlT3Kk5SSjhW7SPnr3%2FK1Sw4LHeVwBwcqL6bVHMHCd4Sv4F8%2FcEc6x4EjqccFOjPFq8sPLvXXtu1dBgouqIQlYskz4JgpZG0lo80A4ALdDHVBNEpA2DiZN8z9IP4GxbxExlPOFeupgz6T20d65HLNKvIxsOcgi5uS%2Bh6s0XXTfed%2FtlBGmxf3haXDI6i1HbliH0XJ5%2BtZTFv2Rqq%2B7TSQwnvmXfqGNDKOLp9BCj9Pyrhea6aUbgMVy%2FVfo6srsoD8Y5gO5gDcBmSQ%2BEtCfl%2BVe2iSQcCmLomuLXdeG1sLMM7NiWeChrhaxtJa%2Fh2Q%2BXfRtAq87l8IEgvHyYsCOlhm%2BuGWPLADSFhDNto5XVnPS2%2Bn8oou%2FGpJFQ%2F4AejqSgp8Hxik6xe%2B361cESd7eQLeVIsAd0NfEoGbMWbQllJjC7o4nHBjqkAWKVGAXP%2FxLEIn1COtK1IxxDwAsUbm2zItaD%2FiKBnf1PSg6dbaPV10k1FA7P9r9zHPsSNgHEqAPBz95QK6rAV6yX%2FZv0uXOh1AjShjqN4X5SCY2TzmggOpcy2pZ%2B%2FNYE9pgAj%2BtwLkIAAmoWQoC8jz67od371MPkCG%2B0Vj6RG%2B2JoRPdMI2buBMFzKi2tvo3R21Wf%2BQqbT9nm%2FZ8dKECEE3j6LTg&X-Amz-Signature=d23453446dd347e8dab6899fca37d696b9cf1817fb88fb5cf8e2452ca7f82193&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


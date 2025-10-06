---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665ONBHBTY%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDSAwjw%2F%2F94sXx2ZCp56J4ybXqxb1T%2FO%2Fg%2FMztHvMwz5AIhALq0TuePwyiibJMgZFc4iTZnghuHKol3at2ZojvmosxxKogECI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzer9%2FKxH1JllpQMuUq3AOjgPgpnLdNjB%2Fa873oTFSihV6ixxEJ2u80%2F9nOhWmniW%2Bc4wOf0Gghip2%2Bc19LjWxz%2FYiX5DTSchMKG7fUXrNCbiRxOyCnqwCRWsSJtD%2F%2Bar7IparAQigKZzAgyKWhQW4vF2IZ2nJyuChghUx%2FmigjQkjET9N1DAVIJSK5m9n8MPVaOVwrenSVdH7u9ErEidp1FFpkg7RUWqj3RU3izmwDqpg%2FgM7onRM5uMO9clFi8K5tWSY6BVRQtwMnaWj%2BfLlBwAybcwnvch3fcIVz%2FRUhbn8W7XVtWJOT4qfL%2BzI3%2FcMRcDbzAeXUR4hYwJ1g5WP1xW2mefQMeihl0WqlCEm3PzvDTCmAk6mkmqbjug2meQWnt6cMulGIe%2F5f6yIIRNWN56ns51DkNI91hDr6PnLsi%2FZgrfIK7yfx73TvPl1bolY9R2ShgppBqoqBLBtZwVG7Cf3tErOyPmR3Xjq5Qnda9ZdvTG3%2BhxXPgWTe5dta5iHc%2FDRdhUGEvXBf75mIimwT3DC1keOrPRDzSLxjzTcB5jB4wP6oyZqSTMAYNXjEsXgShXzGTf4N569Bem6tlZ3UTDtbb8245WDBknqNQOtydgRWnBj%2BPzNDcSnAcP461Uqc%2BOueXdtxFsbQxTCrmY%2FHBjqkAR8euD54zJaHlX2PGn2Ln%2B60fwrrtSNVUqwlHUGJQTcRmtz5Nhm0IGYWpJBd%2FHb4gapsdi8rh9iqVZD3yAUIslHlIB0ilgx69L2OZ8%2FX6WHLZHiUjJU30yULHIBWD1NmRefDmZaTf7KX5Ofp1TtThJrQtbobcfcvKrFgVEcymz1J29v3M6Je%2BNCAejppJomvsA%2Fr%2BmBE3K%2BzS79qqyVEkC4lnk44&X-Amz-Signature=024b076b4963b48643dbb7b5fd176e247d8b05e837b8e9f05868e4e716082021&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


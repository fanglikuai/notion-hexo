---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664QTX5BUL%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCBMquF2L3bZMOpjKvtndK3O2U5o3foR%2F2VzYHL4VOBHwIhAPt8AIKaKiD8t9oyrP8LXbz6TNt0F%2FM2y%2BKzFZCVOgFCKv8DCGwQABoMNjM3NDIzMTgzODA1IgzMowP2BJkG59oJsKQq3APtUQyE%2BcK6CoFmeRjrJImP12UavmDA4SEsAX5TXIb%2FMulsoqNuql9JTA1W%2BFWKQcRaI7POSBWyt4PXFb8FefPyKLDMuQQKd%2Fe7ce6Ak6nvcgQ8SA7OVDaWGOFgQgv53pkKPROl8H0sO8qh5w4dOelJNX0lgN%2BHRqPm5sKJJsVaYUr0QMmaA02ElKg%2Fv6fVhOmTpUIrNfysPP7CkYHYGsPY5pawQbZCLlZp4ku1issIderLEOk3NmYs1%2BSqE0lhz5YT5CtxcqLphP%2BLyNiFrCHp4%2BbpP3QNqPWHGkBFRf1NbeG6CK63YtmKASV9BvtyOPcgvdqyui4jr5hW%2FzqRQBQXZW1mtC%2Bmryzk4sT1a3cNtmiSr6yAF7%2B9Mq2nPiCjF9LbrX7WgSiXraN7EtMYXqyj3K64hyBxNbWan%2F7icLKxK4%2B87%2BVHki9XDDLGYhr%2B6ZPfUdDp6FSvCBhQFxjyZy97IfXkMH6wDF9Xc0zA9PjBnv3ErFTHgGz%2Bawze7I4B1Vord8aIEpPnX3ZXDZOrdRGNoRKL6L8KuZFThy%2FI9yf2eHiQEvQN9Xqu8tFM6Ko%2FsJb97Gh9eIxgoS4JyF1u8k3nlLnj%2FpZhg8toIFrEoKe2v5hrMJJ6Ar4ZJVaQKjC1o5bJBjqkAckLsC2OCbqZnez0K1ozgaUVH3Mjjtx1nQD1BnYGHF6IB4zfYh9RtCIcR4y4ZAYPlPzBnso4i3025uRx9X7BAwgBJWhJmI9MGm1EgCC6tHTqke58ilMGqXwx7pG5UkCvnCmfpfShI18IvNiOH2vabrxSRWyJ6npsgOZ%2FNV3iK33aXg2uT%2F1TXQ7bUG%2ByvS%2FhEwIQxRY3yBJb%2FkjdGN%2FnjuRAmJom&X-Amz-Signature=33797396a18b8ebb9254f99eb8900a0fc5adff9127b75d4dae79bf477d7e279a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PNG6Z3F%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDfHpqbKyCJ%2F9CD1MJZ7SDM1o%2BdP3O%2FZj097Z9x%2BLPzGAiEA2W9UuPB7XxovLrNtF8PEqZBYwqm146q4sB86IrFNAQIqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOpwCNnqTjxEsisDfCrcA8F74QnDCLQ0ZTqU%2F%2F21p4ICkpFWORXDihJ0ja9SQh9BKInL%2F1PyvcyQxOszzfHbMCQofBXI3DrAaGfJ%2FkqI1q7m2eiYakPtYJPKXunc57P1hV4aohNGRq8CMQWlY3kt2%2Fd0ckSlpF%2Fk%2FefEaul98vJQp55YBuRNMhVwYs7aVUs0nX3BXfiCiq53X50uqGqykWtwKj1ekhefXmQ5%2FyKvYtuePW%2B5CwLxPm3eZxmgDkahFT0Ks676ABp5fMRYaasmLoypcPzZvMIDpYcdW6QYbGPS2F9l7HXeMmlJTPsQjKN%2F7sDy9eR3C3R2QmVOdewfKtLNi9nBRwyMLFwu0uZ2lxOE0Bt0lytABvunNVZAaCiBlbAkLbYBD2S%2B0Owz%2B9ITowVQXGAUaI5rDa4ZQ8hqAf31ZKvGjEPZjx%2BuZ4HCxV9bsurp2whXs%2BYRN%2BZGahOpOxeFYp5ult7PqdZ2bBe0jdoSehr0PbwuyW8hDvGI6dyGIDE1isWVkR8utiFPBOoitysGNIWkZP0nLvBw8xjw6pSUt2P%2B8nAkAnHp7S3tUo20ZbpQ3Wjdnv9eZSFu5aBLCrwRXh7fl0qAbjIIulxgxv%2FnFVBFJBJPG04ksCGPr9TPpDp9gysTAWdM9qzDMMbBxscGOqUBGUAlKTewJ%2BivNwUdTPLU2LI6ZmgCPadCGyz6PSKfLLOPyTTSYti9pT8l2eUSDsVJOXC5Ziv7BWs3CmIJkE2cQtAntaqVadQicLTxcfyoXAD70%2FRCBmAuAy3ySTJANsh2Y5eKV9kxZZZpjdE2ekFqgLWtZoF2IOIEaHE4%2FCBJpl7IV5BA8Er9dHnQ7Ioj%2B%2F7h3ixXKuI8i2jqgSdE%2FMHXxbJ6UMS%2B&X-Amz-Signature=bcd2594f7ea45630275efb0949f117eebee4f638f73765b41f3afe991d2e4be1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


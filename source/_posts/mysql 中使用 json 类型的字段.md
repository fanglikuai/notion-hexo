---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666X6XNV5K%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDrWv6StsEKVow%2Fm2Xg9FaWxdGMO3uGHfnsHNnxqlB4tgIhALkmgw24ytIqn%2Fb1F0BqLsjAcUJHQ%2FWIAF2otJ38RppVKv8DCHQQABoMNjM3NDIzMTgzODA1Igzhu7xhwCklIC6rZqEq3AMyBOzuVzOFwgdTdYPXzQLgRY6%2BNpgmC7LPwuvp5R1fGA%2BX2DRkNh8BNAMMYOvZa33BsHA1rsnPeGZe7wEBzkrCwlTCW6r0Z6GyvVrl1IfxnR24Sb%2BuJY3Lv2MydflM6rPo2%2FQBu7EgKZAXVwaFeBMlofDRe6zr0x41IFyBoxztu4mkptx0td7ZdusvdqDmdtpIcfcUzl60unoAh2nkWGECWsXE7MqN9DrfhRKgRrtYbPcM%2B2w9RpUq1Epq1Y%2FmrY23vxyP9gGLjho8Ol%2F0kZxNWq%2B5t6sPJ06zANA3CvFC0QP80A%2FVMZ6Hy0a2nB5gDrbLBVyVZEJuZFbQQ%2F4evyHiNTjd1aTxs9pHYPLXdLdGKTqQh4u0f18I2qL%2B6oYV%2Fdbp8T8p%2FtriOg2luxGD6wIjW4ZDKXndt8P8DSURbqCyT%2BSfgPROz2Scbbxi2Ff1UZjhfnQsAqnmN5XH27swLaZtw%2BQqRdyOMZomV6rrgl%2B07vPQv%2BB%2FbRe4NiPKfOuxTEpLx6x4qP%2Bxg42Doit%2BhHLJZLutliEk3hehTttuS6UHsVrxU%2BmV5eRz4aXk0ikFVvkSMJA4%2FMQiBRsgGzWN%2Fg0Xwrgy8jsjikto3SUGs2hkRKVaPZpFiRPr6Wd7iTC4vdTGBjqkAcb4uOqQ%2BsGiS6YtMXm8MD%2B%2B2LR05NgZuUoxU1zJpiz7Qbq%2FjzRSjAu%2FqaQfwWhtVEj1K3O5PD%2FZ%2BaY8LkYmbPZN5t6k6gLMRcsjAndFukegNKpbfG1%2FzNC9Z4hRezCUziincjB3ZcvaEqzBZXuaW6q8baJC799YfWuQfefYs2exLfziEgt3FxUhTxAeS2d1iLWBho4sVNmb2JgbaviwV2sECw2R&X-Amz-Signature=d819bb90982ccba0439479e2514f1e773c163f949dd2e4feed9a5f158642c67d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


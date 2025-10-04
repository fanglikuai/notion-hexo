---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YB42XXCR%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9WNCD0EY1qhoQG2JQgSnb94M8vfzaJ3UPrbaEqN45EQIgWWuM2zmIsNeYW0o2%2FyGGxO70F4fGDlPZWaKAtUWAOz0q%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDPob%2F%2FhSSVOCZYR60CrcAxouhVehJ9qYueh52OrAplZ%2F8w8Bztv%2BGgs90jq%2BFY0ny%2FZmuLY4RKlfVt6Z4O3wTvIfuSWD2vREN%2FMPDEuKr4XqAqYyHzIY6CEDBW2B3xt0QBj%2Fv72Ol4zgdVwYvLG4f1U8pJZIXsMBh68AhkHAPntU8VdSyqrYW4ofYjdYaV7rG%2F85zr6N%2F1DXBahaea43Sg050zuwWd1oF9UsRvQTE%2BG%2BmoisFWRSFku2I1i7SswmH1DR%2BeXPXauWRVKk4Q4jh61P9BOAPvB0BIVihL61sHATrrfjS14chqhy7W2yQXS2XKSkfxMEx3oskGTG9Du6Mq6WMm7ZMlv32DJDXw9%2FbRqGXOu5h9Haqd5316oSOVMq66Vb0h964TXNuhulZgRu4LZAUroPE%2B%2FE0EQ1emxBXTeb3Aolqrw%2FyMP1c1Y2ilm49TaKNDgTeltYctCdzT3tePzluXZKh0njcY9yuIrX7%2Bf%2FnbXWTuFJDUoKgKFKDTTswcXh2xhKiwxIfDpsfz9mQKlT2AEOBlaAmSnrWF8HamWyCi75AD0hh2tNxh8779032P55lM4ie1JJbo8HuIS9s3o0mu2ZaYoeFSG4gpMQgazON1A7yuPcjB%2Fyri30S2zv%2FnF6KAucmqthGLwnMN2vgccGOqUBe3OF0llrr%2BgR4dUsCPM1xaNrrhm6of9zUKPvqS3p4JMaOfKPiMnjis3%2F432syaTXZsRxAQhjvJtuXVFpXKjvg%2FrRHCTq5JlgEKKZ7lqTnmjE3pCC%2BSA1d92UVegxd9bR%2Fl4GJfZWjdJNdMMlesvO8CuWnv2TiWTK%2BGMAkcBc0x2jw9%2Brp12WpntgnHS%2FQPZmZJ6qvEstRZMWb9%2FhFmBsJzQPpDBb&X-Amz-Signature=02b3b1ec1488f34e5b3b0a34ade065a708ae5034f9f6eb32ca41a38dc302fda3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


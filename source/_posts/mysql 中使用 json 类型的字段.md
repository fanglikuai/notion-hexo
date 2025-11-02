---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN5RANMY%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQCGEq%2Bjcudng5uETDbAuoFMukIzvIvidz%2BciSrk3HsmjAIgYUqI3eF%2Bij31vJ5jKepRtd%2F6E4J%2BfieIxISFtdGoXcEq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDNDD8kBxg%2FCqiGkHHSrcA9JRxbAqXwpYNOBmOZhcIFrNZsg5upPQZ6gKKYwcfRSJl3FvqXXw2aYkSPihjN1jj2UAl2ik1xg8GWbpSlE357LQykiM6qqRIzh3obZder%2BKu1Xm2a8OLvTeKPP9xwhu%2BmCQhzacZRT0g1%2F%2BM76HKu0%2BxIHSy7p8O0LH%2BSnaGXyL8wim06bWkcvtk7CBesXzmlvUZmd%2Bis223FakbjwjSL4uX9oWnFxOfhrKj8iPTzx%2BVP7Rm%2FGqXDfnQMY5d%2Fp5qaaQa4fV3FSyTWgLy4Ob7%2FO9n6%2BYcHmfQuuQGd7sJgyNGMr8ItyR5K2uTI3mBiK5w1puxDp1cdTO3wqNRrWYMsPgbV1ez7%2FznHkJXK%2BcPwLaNA1J%2B139iSSU0fnVRWVS5DYootXfx1d3g87uE435Qpx6EjfUDvqh%2FxjNXUHHguDOFPJBCb0COT%2BRadXPRVl4zhdkpUA5gVByXuoOGHei6N1lQVlMOJ1mEITF4n2mC98VBpUP4YLI1ohviVeUEtYtIYYeKSnQbbhZ%2BU4StAqs4mXR6wRmMnfR6cl8alX3PiZ2Fp0B4I%2FoXbZvgpW9thc15cMHkHqac4dTxEC17MKlweeSd59umi6MV7s65xnDYWeQRKkkq5260a0vblhPMNTTm8gGOqUB8EDHoVqd%2Fmhz%2FY%2FZc7K7Fh2Sk%2FMPBF1b5nWNL2gNbBAiKqG4amXm8qqxjEdzUMl3k11IV8uKrPXp2ydrMP7l%2FtWSRL%2BQZtmkBAsh4W6hHOkJMxsatw3HSNguWZdsREKEfwESlwbMzFkkuVcso5z2BH%2BwlcqEC4S4Dl7LopeW99W7IM6Iv%2FQS4aERPZQjNw4iZmeNiHbpHvfzu5m3MsddMPG%2BrZNc&X-Amz-Signature=0c475e72830165a1a5a48c8332cd74d9d3fe67f3332b92edf842c9f4aa975840&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


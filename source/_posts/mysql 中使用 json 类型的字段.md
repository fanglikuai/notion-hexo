---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AOJI7ZG%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIQDa9Nn6eBaAJ2QLCf%2FdN9z9J2m3EK7KLvaTm44tsAySdwIgPrilRDvl3OLngSOfw5%2BCQPSJ4qSLQwDTlagsB2tGtuUqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPekwvHSJPisrbILIircA8KEl%2FuWZpFdrz%2BVAmIIJeP6%2FRBnVjyrWMckQVDIn1nBTUQIYCxad5Urf1Lg6rQgnk2mEXPAjePNwIm8qo7PIQAgwls5tAuW%2F6mst%2BFJ9qY0HOpe8rRilVvaTu2nQXL9QoyIt2iK9ETIG3nnZ0vXaUAp2cC9tfd6RqOYJsHO8il5ZmMaBsu%2F8u4yckraw30byXAX4wozEm5JjJZp6zeNlPa2RAOAr9hlRIxZ9g33rBXMm0gyBv9rMNeA9sM9Bm3L%2B6NNYs5tJUAHXqE6RAwTPl5WQRvcfWXp8pziprJ9jWZfG9fxXHdlcMAvVp8jTBUQ9iKu3sfrUIVPq9AyHO6sNYvmwLihY%2F1cKgQ2cuDESzi5%2BVvhbED6o5Fp9whH3rItk9%2Bo9dv665BzO%2FUdKUwMtW4yzlqekgEfNVI2CHH31XAXUdu%2FtfF519i%2BFIqafgT3SS%2F92d9U9llB1adP2KHXOTo4oJb2sScnQ57t22s7s3t81bY7HlIQy59GheTI5LIP%2FQtNZeFkoqBn55kCmhC33%2FeH5S4mRIwxgXI%2BQ6wWatF2VRGA%2FGTOnizUNkeRxp58f3ns8rG28Eldu7RFDsdJJUYGaMxZF%2FS%2FwY%2BL%2BuNjYf5VGrd8juX2cJuPuyNXMM7U6sYGOqUBZjTN5DAo%2F4S49nCD2LKjvcW7l1yBtxD%2FN4mtOtj0s%2BgPG8FdsTyWiYReHw%2Be4CywwwOVrItgBPXMrukZw2u1DOvO4BsbPtjoHPB2rx%2BCcPQwe8q9x3Jvici14aKHg76jn3K5X%2BQFG5hDaUNnLpBOXF1IuwGaaNl82OTV4Nq%2Bxvy8L5Bqxo59hQKrMqJjGDBb%2B4tW%2FEYwqNeipuJJ4TtcshFwBlZA&X-Amz-Signature=263ebf97520faed93aad29018b52e6c5ce0750cf9f57891956d58e92af35edfb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKX3CXW6%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIQCG6ul5T12a14Th0wSLyT0Xk3up0YU%2FKTzHLtd9gRp%2BDQIgOvF1A66UgqY%2F1H7R0s9J4hs75Eb84khDDK8U%2FEZR7W4qiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE%2Fn4plqcrs1AgqwQCrcA44Ukzrzscb7UWtjRF13RmoYL%2Fw1KQL8%2B6Cb7ztHK4A1nvo%2B15CGs91XYgs%2F2ZKtxX6gw705q%2BzBfjEAe0wsB2lCB9q8r5DMCYRq2Z%2BFLtnlcEogX67w0Kh45mP4RKHPvVC2VjYMzVGMJcj9JWuedshnxHcNXuGgErcaCZVtD7%2F1%2Fs2Rcc3iktG%2Bli7Z65xfhugwMhFACSCwLBlvJ5klcXU6ZmAmcYw3CSWBZQzaRMlyUYKy%2BGsGbLB6hDS8W%2FHYjC4Eaq9FrhZv%2BWrEyaA6QPW27EvK7WrWiVKrkxrrZe4fuJy9bVNgkBUM%2BclkvA19ditYx5S0X8cwFq6hnh5WOhrdz9aVvYi6S3y0b%2BuvFooH2hFY4jW%2BpyYUnHDPWAZ%2FUBGS1JzjW3jz5R3Kvc6%2BeeAqjkHihi32i2VJnH106HOEAVPXPUdBjlacvErlu2YHv83LDmLlZabLwwtwIA5aQvUrsrrax4OdxJEDByePO1KKziZ2OxTSm0EpSPqfoULRQDudpfpiVUGvehYDYrtLz2tCzrtYCPynT%2B09FZxGcInHOnv%2FPn4J%2B8Oc3OTIlOw8jkdHObXNeRjkUWT3oArCITZW1I847%2BTzVW3akIVg0cwVosvdtyeHhdGhUO4UMN2a4sYGOqUB1eITAJrYW%2Frez8vMCwwlu5M76Pw30A%2BSucIbS5Yfv9d6zlOCSj8U%2BNqsIOFe00Yu5%2BwP5dXP%2B9%2BlfaHgzGRWOHSnFlVwHOQKjYhRkWki%2BytkThfyb40EmJe0v%2FYIMfC0O8w%2BW2YAAelWYDpgYUad8OWRvAA%2Fdf1afxlOdzgUF70l2cGY1JCC3OD3aTKFhGnYFjw2gGKIRW6RKwO1QoygAX9N451X&X-Amz-Signature=197b14d64b9a39d36263c63c56a5d41a5b16deca410c89de16f14fc9739b7214&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


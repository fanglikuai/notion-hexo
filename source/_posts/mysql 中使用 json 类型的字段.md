---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZMD3VSCC%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCicdwijO12OUC0rEB4tT1Et7aJN7pv5z83OBOlW7eO6QIhAIgG1wV9s5oaCrbz4OCOU6gwyYFhBh2lWfCmIDVcf9zyKv8DCHAQABoMNjM3NDIzMTgzODA1IgyTWgHYwj1Ep5HXFJIq3AMuBK6Whv5xlS8aL3nqxePbppettX4CL9u8Req5XTvaw6YUBsiVbZ9U%2Bcc24TFywB338HKaJfpM2yR5Aykee0DbFutbsVJG3eVywZcXwQtGlEVyl1uJ8rGBwRaLOZ9Kv%2FXbaSPKpgXKLHTcAxWrFJPT9lXL5o5Rhuiv0jhlhTMGYozfnfeVFXcoDffWpsAPfGsLqzRh7LGj5fPN7I3XC4xmIZ4e%2BrtHLvflIYzL1uoNq0qUzoS8Q75SxACbIv5F79DMrusVDnK5mDtFJL%2Fi8%2FbM%2BqVUueYAclgJi25xUjrFXeKHYnZEBM8IwgUU5tBpU4EXbFdbp%2BxifEB%2BzNsnmgxwDF9VLILtDBoziKlmdbR45Po5fNfmtF3E9iuRPzqO1Zu8rXs56%2BN%2BLGscDktjaGFlqg0UOBo2MUXwa%2FC9PcZP92iWC7kNpfH7iuayXKp%2FTotRqDnHTw5ZgtwTLcnaQvXiabxjIrluSWO7UUGH5Ok9iHB1FKwF7U6T4BeAgEUIlhJFV2Nmd9Datt2g6XQKX74AioKrvbrdlELVCSPwxfrn6A%2Fnv8Zf3rpSp4Ge%2Bj7LWoCb7OrwOb6%2FpiE%2FruWVl7jhgbOShcdaBPu1C2qIpTYANMjwY%2F%2FEjxXkedRIRzDMy6bIBjqkAXdwejH5m%2FHkRtsjil3kuK3mmgIcPRnqMlGUvxlnDgrCvMmM8OqesihVUZqGBrmyBiyR1FKE%2FuXR%2FEAfvncifKC%2FBTnwrYJIAI5Y1O1NISDW%2BKqu7kEpz583yPpTgNbNJW3C5g9fPYuu4HoSnM%2BA%2FKf2d5SwCKypiirAr7AKzI5xbiDN4O5Vm%2FegvKGztH%2FH6jvyt7a2gWtywfANWJQY9CADEsU8&X-Amz-Signature=9cd732357fba78979332a6fe90108d761be36378d70f985ab5ec7399ada8835e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


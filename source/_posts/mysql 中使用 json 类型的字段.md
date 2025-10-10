---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q72H3M3G%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFIaCXVzLXdlc3QtMiJHMEUCIBG8o9h5wyYn0J%2BUUE7Cn9ok1f4AUc7TyF2R4u9nD4jDAiEA9%2Bdry9LzXjOrdxt%2BprgKZGigBqCS6bIh%2FMV%2FcXtz8lYqiAQI6v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJVN90eqhC5AP4ngxyrcA%2FBwLltON1%2FvAONSMn8vOYzl5UmFU%2BLYNyIbvvpONd3AO7fR9Udcbr4FjZmYbSxr8wpXiTkMRPGSEycalo7x7A5YF%2FnICEVlG0yFuvci%2FXRE7tIbQqow0ZRnjMeY58QOwkBpfOKx0gT48qyg4gfitVkgAwwXYaZLXLGSg9bHFEaR5Tna6wtDPNxVAaNdq3c%2FOc8%2F%2F8UCq2M16Zrqb4%2BXQe69oaWIcI92PwclgO1dyKi%2FoAWhc2gnz1wruk33QFs5tOnSeU2joyZ2T%2BsRiwpRyS%2BBpYw7rOBc%2BW8XDAiPWivBHg4%2BlsSsfi2tjQnCXz%2FJz0aZXBBn%2BU7gRbUX72EbmaNo72dSz31UaDpUb1IE%2BPQcfiedvyQBwnHSMFDhdnwT9Q%2Fxc61rfoR4Ql%2FC1XiM2owq1NTSkQiVI%2FlHPosqg5XCUL3M8pbxHWCjJf2el7LrwE1JeTFNOCErqKbzOTrWGrXw3sgiqFdBqlATFy6jE97hortj1bw2h9JKm%2FYrgMagex8CDVPba209DVMkeR0TrPKF0rY9hyK77knYSeReMHFsHqE%2FxI%2BsV5lO1zSOUueH7TyUzt1KK3rTBxdW1g1fkfFkpy7gful9hhBxw9pD0O37P87P%2BnOLbH5TzzFuMNOeo8cGOqUBHxVJe%2BloIZcVAwn0fUIlOQpYtYN6dctMAwfqK17%2B0XYTpD1N49OSTaoXNYhlBBFPfkpRJCialgG5a2Dgtp6fbNk0Cq1SdsHZSHHhsG4UGLtZOLUU0dwoownbgqE58pl7HZleuiSk2ewCWq8UsuulPudwnlJkq72N%2FJzDJMdpMSACGzIYodD2zgKlqqTquZiBBU2gd1cS71x5ByA0n3TgaWIO6jQY&X-Amz-Signature=437332e383addc7f32c379caefeff95b5c787e704dd4b0cb7528e5249fc4df9d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


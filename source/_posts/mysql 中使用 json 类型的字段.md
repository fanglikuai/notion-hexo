---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BP7VFWE%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJHMEUCIQC64Ws5HXHqZpDNczYEWSQOwfBrzmte58RrLQqBROT6mgIgWLA7R%2BOjaABK4VKLW8Q%2FJlWWvgnjKytIMpi9lM%2FhP2IqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM%2FkBejri7ctP0Od6ircA7FkN01U94xWmMwzby8bzffg%2BNzayZruskzfGgaXUiGgX0BRoxwptc5syYAWxDz%2Fn1UvlIVeDhY3oMG%2BT5rPN460Jd54uTqRLi7K9QxolIApWBGQi1MDpewQpExBMLHxupQoWNMVy23ha6ENR69FRcIPZgLBD7r6TJ%2FvSzEZ3rrq2dY%2By8Tgqy7Bx%2F8ohUAV0D8KAXHbwfOwB6Taov30n2377kYSWyy5QA1bKVBzYZGhDH7yHsh8UXOfkhZXhutwnCYttK9CQLBtyvMr7dJsQKWVQpXpLKuTMU%2Fov2MF%2BSOCnSvJEOUrLv4Uj1S49eJzuJXTmjWSDMtn2M8R4597RlOmmAk8D0PhDoNY66%2FLXdSTkbvO%2FqroudFb2wsArSaDk1ThWpiCT8Lmf%2FnMXNPqZqMmatfhNIaBXNr01bSWWij%2F%2BhLabkhtbTzVG41otbvbWAqn2eMdVKgEF20HhzALik1Fg4ugjctfYPv%2FQmq2bfZRIjaSaW1hYemgDGJbsQOGbYlK%2BvYiPxCHxwcHtIzg8HqzJ6vU7WAHSsT1lV5L944OpYo5jcxy%2FXnvMQMUxZ8vsn7GVPJz60StKAtLqG6vR0CZA%2FeWpdJ57JE4RKIMdCDG%2BBsJRTFJv35L%2FVu4MJq1jMgGOqUBk7m3%2BNLCBeul%2BjEHTf3jORwTuDjGA3bWilR90XaaD35PXjqQP0se8DVGLSV3TILBzZWh12cP9UqkYVjs%2BcxIIRgRYieRlNEe2CcthWugMx4azfDWJD0dgCC13UOVqg48Sc9%2FO4Thnwath%2Bif5CacgY3BxyXdmSyh99WVHayI1U%2BE0bWHHLSivrucP5GIwLrgSkzFBUGLrAyRPbhQglkugc%2Fg%2FXwA&X-Amz-Signature=d77a20ca00e181b1f21d5d017a1fa390c50f60838db40c4a30286e9ca6075b50&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


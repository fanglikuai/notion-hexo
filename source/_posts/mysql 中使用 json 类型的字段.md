---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665YHNERIE%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIAGhXTWR2z2yfUc1XUocfxFpjnJMTmEjCPr0w4R50XpNAiEAtbSbElgpuvi1BC2Lu9dkXRTZ9%2FWRjsVjnW%2BfN%2BwAMjwqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCPmGMVlAmB9Ex5k7ircA6XWoj8pHj2rcikBBlZ%2Bw4f8xFtXlRj27guNHEg873BUj0UoxQhZSLF%2FM59B68dB9SKqMVDgpNpR3s5RQtj0X0%2FuGCQH%2B6WJzX%2FyNZwo2sMjn%2BGaBTa%2FABRqjwhTJBxQr3UAioGc4HvnZ1TTxLrGYxYS7YDPinzHirp4sgKlnFzMv97WNcCY63CfA6QZRERSkxyQIlrrUhbjHRHXhQ%2FZlfGs72qVrZORAPUSfnOLjDOyqrtGfiQyIol9lAs1WOVugQxV1P9zCctMq8Lqs34XToUbhMGpB5UrPMaz3zl4IyCxqEG64MkM0fgEQdy3KDx9v5zu34Pz8OgAYdfr6Q3%2B3Lz5EBHGiiWVnGwHtAKxS1QJUfUGdX4ieWxFNhNRhULVrAyzpkQ5VYG5SIoWAJDSdCiriNXHBWgJsvRnQNXLf9eZWhBcGoLCi8WuXdFOiQjIPhSWJQiEjKtBWfbcp65hRPV4caTKAlBHfynM5E77ARmUXeJGxkgvF8sZL6vmwzeOv9PATsG24NDFaFHF43x397J814FPybvZx%2Bb9%2FYAVrO39b5GWcfqP3dWhEG9GXD79zKoUdTEjnyLnpYd47GmmpMqk1uYjqMgixXJk3eARwAP9ofLkTPCycVR4%2BLWSMK2nv8gGOqUB%2Fz0k5rx2OxmjKVO9SKFa0s1fcqI4HalflS6VRNewOViuCoOpfUxo2%2FPTxWGbWWiBBbW3%2F0nFCDJoxXHUp6l%2BpEfpSU19SNnCzesyZ3GctjSZ8qLcaZfaZ%2FY%2BBe31jB1jcIeHfUHXcyk6UgvKHFHmD2rmbxAnK8r%2F2rgy6OZ0Z5nmFE15kS2YndwhOXCkxHwBxkj%2FdGJ5xUuVC4MrwDuenyPdM%2BCb&X-Amz-Signature=bf7187ffcd1b231c911087c08acd36467ec15b9a3032fa71d8c71b43671db222&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


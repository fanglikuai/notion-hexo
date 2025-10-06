---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FHLRK6C%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGIvr%2BKFnoiPqynVrl7JJI%2F6WbqlJSFDGs62GBfYhhf9AiEA5RGRl6SnUHaL3Vo%2FvGddwEjwNDdaamueEfGj35JLIhYqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKZ694HB6%2Fbje2QvoircA1GuLrxb5xqWxpvbsn5mJbnXH7E1L9KwjxTOlXGZQzk%2Bgd5sKJB1gY8mTG8McwMmyPxhPeSiZ%2Fb346%2FKCwBsph2BKHUvE68pN%2FO%2FAIT%2BffSpM96k0jDr6eD%2BKeo%2B1JgpAgPMIXFuzrBI6NK%2BZ5DMHDTfWZrkRqv5moM9iwQr2EsKbJvbpjtcYaBbjpYh7YqRg%2FhLbhSZ6I7kHQMqLVA05IJW0qRgo6YlPsaUt55ukZYHTtUS%2BoEpi%2FIYd9eSg%2BBc%2B8sysNUUrygp0JoOV3W3ZES4xcdfNKWn8t60ko%2FmeVJAfPbWzG8B7rSBhF3BQsR6BLZlLV7nnakwYGO5SGDg3SHpXDem3hwXgaX9VBWEVKzxTgeUnveZX9RyybRK3x8OqJmjyINt%2FxTkCRo8ml3SOg6Mg0ORNIvT%2Fz99nH8eMf51vviwm5RbpxuDG8m3IFVUtkPCpomTOhVk3lBBJk7hANoTMBHuaqD83u6UeDf5baFo5LUTD9eHoV9p5K3XgJ1NzDNpnHL95rn4Z8e8Db5%2FkHL%2FQgrE97D96SNJfvVyJ5FX1Zz6GWcpMeMnmgvVAdnsY8GQXY%2BePi9MzpVEmCpMQ%2Fm1nf9JvdayTZBNm04Y782MQjZVuNcBGfpSPNjwMID%2Fi8cGOqUB9oPKVTiRWzIcvCU4o89CT0auyZPI2M03la4%2FfTbydCqFEuEJDLrHkB9pwGnRvnqNOSNJ32iLiekM2rLH6J0e2N41YAzf6%2F5zkj1WYWXXUqhvFdX4joMqEK716hsMBQ%2FZzmX6SGVVH%2BThwtWrbk70NhyOGZqmWPf2yEyun1ho2tvhSkj%2F7z%2F9cWBi1TaoQ8wvscU2zZpo0BX99FFdF%2FwL%2B8HlrynL&X-Amz-Signature=196b03c576439adbcc8c4bc2b202c8f6055fa88fc306280bf24eb4de145ef12f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


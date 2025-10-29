---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6EKOZ2C%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJIMEYCIQDN%2BpIGH5Pk7UvCT9jYKqofJNmACizquMDHGWfF7yWoSwIhAIRyWah8Grff2z1Ee1IBgipZZ6Y1NleQ%2F9qKMsHUST2TKogECM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyli5dEnVRZIcgdO1Qq3ANAuMvIlTEiubYs1YQ0mxmQRRab98mXC1IIx31mTUTjxIRO9DSMSUYkOC%2FKoXq%2FCiL7UcvLOfjohyAiYtHqZkncpFXwCQuf8QnSTz3VcU9eBzsD5ARvoegh8cuKR%2FClhxspcjJLaTpFbNdGKE8zmzqzgW0lFSnH7xE7%2FF2b2smw8Yjn0ZgIu8WolifCAYVyxem%2BsX6uMQISWZRMhqVHgmspEOzCBEWGMDwlTtBUys0braIlIyGiV%2BiGv8zduPbbfx1csAP5bKG9Cn3Nl9i%2BdW9HJrIVo3FtJINxtTocihqZH1XmxDeNYBaFQevPvBhaLWxLylqSqZMrdGsgMlnsqd9IWVKov0GbBU5raXsiLDAOohjwwnc81k0HBZqmqMiLKTpudtRnIkAsK0ZGRLzLlhAuxDf5Rp8Ac%2Fh1%2FZ5yUYtEakD17abcKodfo5nwrxXoiDGVWe9Pjw83c2PJAon3cu5PcEUa2AfuluVnc8chpKmilr3y7%2F9x%2FkCSZCrddxaDxS0yYai6jwITsv0wIMdBgONjiTfeIpMv2fjl%2FapnTGLAx1BAhEr%2BlwWAG52UD53weAZQWGEcnHbpIBXx6nhQAiHFTd%2FTmh%2BlEL4rzmebtML%2BziJ8Yfo8KW%2FMX%2FezrzDNv4bIBjqkAZ0Mtu%2Fu6a0awDisx5EfUFrs7j%2B2k68QnPJ72df6VwBIfcrPbkHswoIoM%2F2c1y%2Bd2yKrmLk%2FeYJC49hbevUW5MJq8GKl6HFDCchIPhVZZqdwFnEt8DmO%2BlyRp5gCyWhoQTq06P1Qbs5KKHQcRmPdtaBNyCRqdmohM1rigTf3wEOpeDZxG93VCh607V4oBYdro9hYV3GZVnWb63gMPu8PRlsrk1JQ&X-Amz-Signature=ddf06415997f5168e4fd715888d54ecaf1391452d1d0e40a8bfc17326c1a0e9f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


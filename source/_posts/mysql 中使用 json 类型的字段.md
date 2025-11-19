---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VW4X5VFF%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBkaCXVzLXdlc3QtMiJIMEYCIQCom59Uz%2Bn9r3IUKr0xFiM2d7H%2F%2Fk025zH8TxH%2FreTRMwIhAMuV%2BD%2Bc%2BNUiJgDjp1sRojmpiPj4LONXSsSvK5SR%2F7PnKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz2JKx43gZJ%2FbjcRZUq3AOl7w1z8G80CrJM7S3JNxKxt8LknP8l2wy%2FWNLXZCZZtg%2FJi4qI3BFQAy%2FwJMUjbE0end1VH%2BlxHgkSPGxBDSd%2BrW%2BeHyWwhNoHG2FMurU5UJflKRcYCyguXDL5hFmh%2Fw%2FisTWOVaIb%2Fje%2BcMc%2Fd%2FAYZAjfeaR5LyztFn2ibVf6uQHmn%2F1tki%2BR85xsESySvR0CU32v2TObHpFvQSmWPBP%2FtPC%2FJRE86zFQsJ1vfIer2U%2BnfQD2VMdqtYZT%2Bwre7yROznlnUuA3GPEvG1TWA9M0c8u8shUvNH%2FPs5mGgPJf4WoqCOFzCTFgkgCf5axmjbfxhd8VlHq74NKUw%2BwrujupBWYpRqMcepzkn21VLPJBAyFiInLsEvhm%2FPcDnGHpqjunC1NfqbDwpGpbAi8yTKP8icl3ECsuNkp6ILQK1onJXT1rSioCYkngYHBgz2p4Bbt%2BWOqBbkNwgPP%2FOr9E27IS7a8WF9fJ9lRmb1fyNwIyUcX%2F04zqbhgtigS%2FqXEckYgHj%2FGhpoDrYBlKiyFwIgrYFo28Gr1YQpbs8ED1jHGaN214rhUz2nkrady0DQR3jTiMrXXSlUHLSPx9QMBmX4eGKWqLwtCsQ7egl1%2F6Vlvkk6xyCxp2vYhh7YyofDD54ffIBjqkAab0Exa%2F1xkWepxTf2qWg91l6NisQhwf4mRl2gwhQoqzuxA%2FY9l3pyzkswPc0MDHA3tNN8saBPYDLoLuRHL3nfuebFyI82EdkljLyeMaPJcjKrYnMRaL8HZ4dmqESoY06mxn4qlx4XCwHH%2FrS6YjqtYiInITCogmZ4mbTRykq%2F%2Behz1LpNtP8BfDPeTXQDJphTGh2GWGNyAPvnusZXmcP2I9cMj3&X-Amz-Signature=4f59607e93bf323d5ee0cbf54b7d0dcfec3d524418511ada6e24f17d95523ee3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


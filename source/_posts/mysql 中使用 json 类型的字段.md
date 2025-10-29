---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663M2SFTUR%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBwaCXVzLXdlc3QtMiJIMEYCIQCyy%2Fak7GVdkZrPBs%2FkNis46aY42M2MsUeip9n0ECOneQIhAIvKzKKLDNeEx8SMnFlPfxe%2FspKntt7eAFuxUkiVUMSaKogECNX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyZRDg9Bzd52g9xMAwq3AMH98JWxsX4nuo45LiLxo7FCgCCChl6GqBeoMKPUK%2F5tkJYJWn%2BbT0v3bSbBHXarEp0I2Zk7BEFWhlQ5DMGrlEmOBzeBUtdS6OE5sArdXuJIbXrnqoFfBCgqrVMYyth2MAf4bVCTWc1QMN8t1JaESDZQts3etGd5M%2BvAEwkzzMnTqBk0kt6UAnlnCZBa%2FREjeJ3ZvOJrAgh60etcAa%2FAktjNqapQ3j4c5HQtwcO1IOC5cLIVJ%2Bq9fTZAM1PP7h1TIkdT6n5F6SrzMmjNybDZ4hiSNxBS4F4ZvsCBWcFu3z5N%2BHFB1fR1tPrCWlXjwPSicc1FLBoH0vP2p8FbnNkcH8tSjr3U7r7V3xE6cE%2FVLHM3iwL3DgafI3uIVlYOi8WCJX5iESnepe2cewlJj1tMhPV%2BdOenJHijHHXZCED5JkuEItyx8VhXm6QpWTgEqynPGNQOZ2Gu2tH2ikb2jjs%2BxvnUVqeqX5e6GxtnWTH7Rn0MaTpOn9zcsU2KKVqHrokBBc65n6rk3AoBWyN4vQ0l4ijOoV5eZda6x473OiY5nb5oUFxKAS8W2FvCXRch7sIRBT0KWz0X8iuj5g8h%2F%2B%2BAfYIrU3qTjKrlu4ECTE%2Fo9QBc3ERng1RkuKYetICVDC%2Bh4jIBjqkAa38q%2BiPuqlimf6IFPnWt3BICXOb7MaN6e7M27Vsp0bxC08jHv3caNNZPi9SWwdCLKe2nHogXHPZdtsPuWf7AOYsPmVkMtPTFL%2Fep8Ue2p5Z4cV%2F4i%2F39DpOwlW%2B1NRdMYK6dQCYv0o8y3lgK75OsO19j%2BKLJyICs6O3aXS%2Fj0lXT7EHp54yQKFyzCtkUr4FBi4Q0WD4bVP7DCLqIQRIgdvjjEP%2F&X-Amz-Signature=43a5db0c7716143f92e9039abb2a518a3272025c4aa679614ee2415dccb91e89&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


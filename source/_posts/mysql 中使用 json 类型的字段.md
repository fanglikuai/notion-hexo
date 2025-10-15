---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646ZWLPAY%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE3DwxT4UbApUYJiEatmbp8%2FOgr%2Fad%2FIdtrie%2F9pYocyAiBJr%2FxA%2FCcGZkBVNzHI4KDIyrjRC4ep9SopHpYKEFBI4ir%2FAwh%2BEAAaDDYzNzQyMzE4MzgwNSIMp1yDxyukphkluEbTKtwDyJMbD%2BDEiGeTZWLXkaM2B91RmTQ8qOiMN660m8sZJMj2%2FgPQjvUS1Q9gthwujRWMTZ%2FOvqzPK9nrks8L2vz5TupvVZABXHrIdgrgr5oeVupUkyXceWoGmzyvHWrkgDQlLgU7GKXS%2BGB3nqA%2B%2Fo6899OR7J3e6DUsGRgyB2X9AQ%2BDZVZGmcdSKl4inVc%2B5ufgJ1%2Bmn7xvW2p0of8CUTyyZLUjXGGZsEZ2wVzHi%2Frc0i09wYL18cGjEwK8TNcAKNZGmwz1bpKeOZ0bwG2VVmgiHmT6C5j%2F1pHeQ01XpdfeFKFdJwwtQVAwpGfNLHKkyTZ55irv9nCGN4Rrmd%2BkBOjHy8DIxEX%2F79F8UiXlEFQAs1zyf5k5Pj1MfQ9BY0FymgkIpJikGMxBNuAVWPevS6XCBqmtUc9DQhBIhNWNaRWaoEU%2FiUVDcrAMCkxUaubOxs13ucBGNA6qtLNvSI81XVvZ6vbW%2BqreQGqlqLVN3ZPLQatqbD93t5PMPPdcmuAoaWrHZFEowF8AcULvjr8ylEAPrREmWNv96nAt%2BF7sOZOG6lgavbWH06zpUCSWmJH9RlR6v8SBb2J%2Fgc%2By1FlGeUta%2BBRLdGz3tb%2Fr1lWunzf5pStdjjp4YGIKwEOP6Oow9o%2FAxwY6pgHtpMIarkmOHX3OtCq3SydMgmqXVAmdfgwMLqqgjQKlu0BNztEEYTizZjp4MkiOvWej4LeynXNUGiDp%2Bp5VV5dENDFJ%2BjIYPMWG4eYb0vXcut%2BkEaHUjSvjmYRL5FM9AZuFamuSzs72Diy8BeVbin8PmbJUsZcwY7YRc22aV91aTykcmS6Jsq3xc3xASjzWL%2FogRTyShLgsQiLmNB50SFXGjYNgoTjK&X-Amz-Signature=4f8cfaf589f5a8f215b540825be6a981083ee21b4fdc351398788b401e8317d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


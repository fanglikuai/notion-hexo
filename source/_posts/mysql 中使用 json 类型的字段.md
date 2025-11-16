---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIBUMW7O%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T140048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDdteF9vasAY%2BO4KDloBN2tYTk2SQsNTgqQaaIamz0LmwIgaVu1XXorcynFVgC7ubrhCFW0gYEw5rRdwsVmi0zQymoqiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH9AYBnOU2y8GuONISrcA%2BIauJzEIOblYSZrms13ao0JdwVX3HM9%2FvcpG53JGO1BhNZ5zzT3NYSnk1SSZzMi5PorHMDy%2BD%2FWMbKIZMBiKx5OWu2Ki87PKt78pbABJWysODQ5ett2xF5WpLkNFifPyiCbgZZlJErcre9W7BwAonBIT78qnSYjp5CkOLm0KFhFkMc6g9DEA%2B%2BOg9Jaa%2FRsHKqVYlMiI3czgTHl2hmRtFauK9msKcwazoEnLy2DWeXo6c5oB2ZQc6YF4hKBlCeiLPte4jQAguDyQAHjC1H5UfdJCSyMCxVssefiUWIWpBc9B%2BaUg5URHnv4vFEzAHFs5DoLhSTuyMQ1IU8JSosE7Urn8c%2B%2F55GngpzG0NZpLAN0jzU%2FyBKvV6aVaDuPkB4u03GPPZi%2F71dJ%2FgD9T5Rh%2FBzHDPs782dFOFuXlBKWOH%2FGJyg18iCle3Px75fOd8WJaPeoUjp9vMExuuZrQLLLsiBmU%2BlgZ3R38nCI%2B%2FDda2TaCJPrXjHIfIfQss1ZSIzWomfaSsC1N4bYQAQOBSE7FnqtkcMffpAOPf3K%2FUCMAnm9f4NESFvP4WOHfHV14NybaPWpV13ZnjUxYgajQWzn3%2B3x%2Fr5%2BnpoHRQ6MPqyAHKW5iswNmkAdBxjA2bhxMKKa58gGOqUBK4exemkSaHWMfYyr1nPtQ24RgnnWWA5OfYe6EtQrbI%2B9aEoyQnRyM%2BLTxHz%2BImeos4zToJ2S0JKzuE7TtkMruhPddqr0TxWOUgdau5igTCFXN6KxkDeflPEyFmb8y9jfxB3xqT%2BNv8niCRqVjyxIVDsSrR3b8MF5%2B0QajFEtXJaDuuonLFBz95KgEG%2BtQEPjWGca1XsvqyBaHPuzX4a9jpuqWiN9&X-Amz-Signature=fe6581476ed0e9beb9cf68aa1d514b794f67417badc11fa172580a554f064911&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


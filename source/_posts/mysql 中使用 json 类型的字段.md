---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SHYLSSTH%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T150209Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIQC1XokbtfcRcwckDO0T%2FeioUCw0hNEW4qRuspStP7cMPQIgWsdHKicq6u7X9VJxX3Skf80s4oZ6ynC4H%2ByvGniA7%2FMqiAQI2P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ%2FYw%2BLvArwJY3%2FnjircA5%2BkNdraoubjYSua%2Bp%2B9DlTvF7MEKh4cdceg8HgXgUW1I3xpzUMNawAfLPQlmOayaZNrGaBGY7sBX1PMMaz0o8NkjhScaHFf970vaXkqhaanoZG5w9LorPw6zusITnHx1%2FvTwJcrzPe8mnT2jT7kQJMFtxESiEXHJL5yKOkrbWVzvmbKpaF3tGWmgdRIOa8H7oK%2FlRwGOSUBcJzP2pplsvWgRGo%2BCBrxoONFcKXsLTGdFEXJxfGZYEQ8cyfgRLO5qxYEY4ZjI%2BWIWhV7NhN2%2FiDvrbjsA7Cate5Hl5XY4G0we%2BN%2FXDgHY4UDu60g%2BY9dPEqbc0OWOtoF2bVnIVXWUnSBCywq%2BAGTVMqQBN%2BqpCyYTv1cb8v4iRpW2IeVq3BmkcKFQ4EWjl4e%2FvUgO0xA2GH5IpCptSrrbgdFNWL%2BZ4%2FEgIdAljk%2BO9MZm%2F2EQNEtW3yAT7WTzlf6tg%2But34IHYsyWm0YTw90eUFLpfbuPiPvn04W80L6vjXDMiFRaH8VjRQ8ZKIJkAmqlnIDQ2HLmAcSe82Bfx%2BGNNQMjxcoY08ePO8KbNfu%2BNLtRhUD8Y0ey%2BGSubfP0Qoky2pc%2FVzvpy1AXfh2BgVpKIumpjwzT5XWT1OytxTQ66ZrDPmQMIiSn8cGOqUBcQNNT6DP31ETkMYaJSZ9MgfY5VnqNTtdLzZ15V5vElj%2FNmzAW%2FEMf4Z5lCIXLBLowYejJrLe2jf5Y5R8Jd%2BwFEy6DrRcHliUcnCwTlVQGcZYUuKaU1hHbaSLIj3%2BhNt3nGb20Cu54xAudtwMOoKePTbZH%2FtZL%2FAtJ6Sq5yuoYb9Ce9eIDXRUzSpkeX6XxYs0p%2FASZCVJiCaigUzoibNEEWQ2mrU3&X-Amz-Signature=616fd6ff64b9443f97cba57d92d289f50b9188ffb03f548d7b56b499b2b56668&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


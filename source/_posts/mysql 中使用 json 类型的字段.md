---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QYNCD4SL%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFkE%2BhANUdeB0dPQmDEu3OH%2BihyGtoDdmACW1h8F2%2FjVAiBIvCi0xVyfpeRMBpkUWXH%2F8P0t9sMshCZfZcu%2ByETv6iqIBAiD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM8ruB%2FLbT4CtpgXgTKtwDfmUbMyzdlWUWDs2p8UJ2nyhXZWjfRpvebhf86FRrD8p2ZU1gYm9QZLMFvtORtpxjTUEZc0iuiamre1wOWfImv5KMyXAcPOqNhrGizTM5KKR1dlmyZYPiDPbFxOXVhZn8cGOHsdZ5jINZ2LrhJ2it0VpKxgibOWjzl87HM038iFgvdae9d2NDI7TXDlZa3MhJ63VWItthnPTP%2BJjYDGDC4xMq0khGt3k4MDLTn8yydZj2exvCPfQPBMbtg0MqEp%2FBQK82calO2biwUgtcg9hhNHy4po5G3Gv81i%2BmeuJP3BXA%2Bi85Hw1ITyYBckCnQ3e9NfcXRnWlmJNXyZOFOVmm4UGg%2FQA6ZIguQh0IPIgb9RpF7m%2BZOUSx2VTbqR1I7BMuL5OpkfaTmTKnSaXJ%2BqmOjT9xtsn%2F0S4cwmaBL31y%2FqIEvMdI1PvtyBY2wSGZWnyx3dAlEiZPRHwJuXVSCuYBZP8hMU5%2FLiYsTd6OWEVzS4a9jdIRwxS3%2BUjugMPUPvUcoLmBv%2FlWsYUOCmDjX3xw0Re5AX3qDS2ZOG9xAcFOQhj4dbRoGfYb4zudPSmLNYEJrJZBhUtc9QBU3DNPjBjLH6AZqBTr%2Fo20zOISz9h5FGl3FyIhX4TyMcgLUaow6PD1xwY6pgGDcJeXBw%2Fy%2FAX7%2FWQABctscBxdi1ReslZDJVsIiuIBRerAIh8rQrhKdidwJDbmAeTJYsG0jjpGTuuEzh6Jp14XE%2BRXdmdXtyNAgI1JUfKf1NBDVZa99N6AESHQLOfRnk344diKrCpYekxXDpTXdJQwOKszCgX%2FieLY6u6Zewy762B951NoEO8exRIHn%2FWLxpAFfPc5dcth1wa6F%2FwojVD5FCr0ZxyW&X-Amz-Signature=645b57ccf8a7054b83b39d8b2b1340c22ed5dabd1ec73274f3b8a0101f749241&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


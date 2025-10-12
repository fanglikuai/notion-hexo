---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKYTE4HI%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICclSKL4N11JHLvnzHRMjmVh40MLAb8%2BBnXW8kF5Dcn%2FAiEAyZETVHhz6AJR48ZFj2rz%2BzMS3WeyxPkigxJq%2F9hwSeUq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDJqzuXT30WYX20gpdSrcA8wVL%2F9G8mnwKJusSEzuCm6OkdI8BSL%2BYl5FQOVo491uSfkndg8Mkr4%2FKPikCUbvfHrmYYyMD4FW0dBbX33hsLD12DOD9Wea3M0ac1Hx0KmA%2BtMiUAmPX9rq1tC0RlZ5B7bVSWUVyWaXQ6iSa54REmSJQ1rxqxmK1fAVBssb1TJ6BOvydcCymy2kQ94fhkrJcYkaM0zcAbcqM%2B7wn1FxutScArKUWauoLjVoKbmwAzpi8jaXrKVdJfrWTAp%2FgHcg1zXdKmFwkkw7jgmBQgHf31PAE9Z7CqZW0DSidsWdIAuO%2Bkyqw%2BXaaIcEonBbgFu1LyxhP7rYYeqCZ%2FX6XH4yYTwibfRC5CNc3LMP5BwEE7CMLht3Qe6uxihElHyUTfDII8lw1XpNiEvzNoGo5IKTrd0xSOCGxw%2Bg60LD1XzhawL3tsP5riZJvGL187bUa4YeTIM%2BdqL2NldUi7PKdfecXBsWrq%2Fn48sCBN4KUqEK39FmFrVfH0vTwQgyoMwhVBbFyu%2BPdqaeBc4enE8%2Fpd9%2BAfEblyLqOoCe3s%2Btf1VOL9nh8zuu5TmydTEzlIoE1oggRkJg2K5E5ZGAczxNjf97PO4ufAyycAjCpduRMg542pwyfctOUqZTbQ7UsIDzMNiJsMcGOqUBZzPfltrAJMvq%2FDe39SeytgEvHpr5hTGUryUeNq3TR1o%2FNswr%2Bt8r%2BDe3wdiU3gFfeRLGWDyhhtpq3WSlxD6BYzFUhA1bEvIwhgVoZM0GQ6GKxIrB36Ot7d0ktVHftVdojUT2QlCamdOpw7Lbzexg884npY4wZZZ2DNasPkTyj4kC1FbAZve1pF6rtJJD7NSQwJ2ewIehJm7q1Cye8yBvAZhFst9N&X-Amz-Signature=1df5d62c1eca7a8a6fe92029346dec6ebb46f5e895e36afb7caf899b01dde2bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


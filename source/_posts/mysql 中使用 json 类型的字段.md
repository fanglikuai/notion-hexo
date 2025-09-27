---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCBOF4RF%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T020038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIB9we0hhFrNU0k1fWsNWV6YXLuufXT3CMvJY7%2F4h46xjAiEAobqK76bqywEGGgczLngXSy0DyhQjvccgVdPX0sQuCysqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOalcfIHHv0u9LTrKircA1SXUVZglrDatbb6ccZcsdwsiVc6Lp4Ro8Ddq3NkulgqkxnyUNsuccKefR5UyWoHbaDHHXzjpcKrIoad9oCMUeSMB20q2MVIfS47lueM8t3Fv496WbG2Gmq1ki3E6zUiX5wJgRFmlJ4MTDQjHsGuT5PZTMWrgmeT5TTsQP3634bGeCfCFv4mw%2Bp6%2FWDKUNixcqTGKLExLqhLefG02GGWhRqkAiT9sbMCKhWgVz8ZYLHR8hcMobHjRNSNwZbw3N79I6gTlP3NwgIEiJAvkO2MdHkFJg5YZklO35cUIq03QH5APMobBLcxqCIWboHNBwxdVM5TgYWeSWK6ALwD63RH1XsPS%2FrE6BX73e47AAdTKJ0B83iCtvaAqfAq9%2B%2Bbg5fxidl7o5nI%2F63vNGGDy4nHZZ3eA0eAL0gpPxCHpGhocwM6YhxWMheV7LJcFAnow06A7nJJ1DeLNFMZ6%2FVAr66v5Ak94iw0tMFowFzSgv8IPTtAUqxuPrBnYkqtdPkhRy9v9AVqM5VIStZgL7tnRdV66KzsVg%2F9RykYZg%2B9miS9bVSBcsO6ST4%2BaWpYmSCYFJy0lTk46f0y3bq0flPN3YLRX24GJ6nq0NiQXIgYse1GHTUPzns1E4EsylBt2KzGMJvx3MYGOqUB7lStwUczK9t7hZ4zth3kYCWtnnJwUsTnUWTN7Tg1wSsY4p8ujCWIwHyq3LEOP1Riu%2BToXZt3jBZcF9hcg45gG4xyqKhoFaHkXegUvgNIyDLiaatn5vF05J47pF%2FSYuQ5Ykb23ge5GGxXX4Hs45sKtS%2FM9JyDgfgei6pJoCcbLQEkFUpUvirKXoQxNAdPMjzmHzWhIgjX7r3HIezzM5hqBqRIJz6d&X-Amz-Signature=35099f399fe0c1ac79465882976f313bc09851692b59f75731dbc1d078d402fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


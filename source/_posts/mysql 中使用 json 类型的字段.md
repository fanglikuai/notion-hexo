---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646DY47KP%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T140039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDRkgrzk8JV5lJ5MFfSzQ30Q4nKax%2BK57YdcsAArm4FyAIgfN7jBY6sFV7TuiaMGTcZ9RJhCw73cZ1QiE6Gcf0A%2BDkq%2FwMINhAAGgw2Mzc0MjMxODM4MDUiDH7CQ7rv9fyZLy7oDyrcA%2BJvbZ2%2F61aEWZx34%2F1qvIAphOXSW1B0p5dN%2FDH7d1WlpD5WWz7LiYcoCvswQkCY4pnhjMkMVn5VotUP9jsvWpfY3nsOYFbS1aP%2B6my8gLRs3d2zpLqH1XlW6i%2BX3JwR0gK%2F0LwLVz3ghPH%2BDMV1KylGfFXPrmjZHsMrFF34WsM5Rmoxb2k2BNjtVQCE%2FvKkGQL6UtiIlCU%2FmZksqlSMp%2By%2FR6SRGSON%2FU%2F9vA5g5DiHCg%2BjOf6YGeZl9dyG%2F%2FK6ck0l71yu0UABaJCnOc%2FEeGahjCvkwD%2FR0Bwl7dqBHlwLkDDvTMyv7i%2F26DDvfmyOG7atLRSeVCKzI%2F1M1z%2F%2F%2BqY24zD5wg0XWV20Hpyi9kK7mWX6puB2Z2T8oOQUo3M%2F0PF8oKG1YvpaksvPo1Jv6om%2BdlFIQDUkjETHHahPN30qFyKKJLcreFdbThnaOcgtXNjWfMev3uxUHG0UQaySsySDwEUW7p2dpP%2F%2FEimZYbJS6j6DmJlEX5bl%2BeVRWF3%2FjaS0rQd2e9d5laDNxCR97hU%2F9oElKckD1z%2FXztyuU1Arog3tNVWghJhb%2BXm%2FvHssqKYLqe1QKm%2BB9YxbbnqrhSGkOfb1j1SwUt9Lcmryn8HE6D53Mbl4ouiNODgmMIeC0sgGOqUBYEu1ErMclZO4ZDm4%2BEAb%2FUnQawlc3MrkF25%2FqQmN6SgFYbLGIX96bG2PaSuSfKyFF1uhJdTTM1%2FxA6xIvH6a1HCOTnE8ZZp%2FM%2FEdMExUdCc4KHNSAxGt6S5hX6rdand3pvOHx2W5aO2rUmiF7JBpNE8xZXi406z1Ob4rTEtJvsMEwTe%2BqhZ2mJOOnFdr5USOOoAxgHhLjqyFjfa6uYiZmQI3GV2H&X-Amz-Signature=952bed038b47e030d55af9b18761f7b38ee61d647c3c649d19e3b40c3077324a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


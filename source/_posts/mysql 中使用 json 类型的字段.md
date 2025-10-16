---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EGEWTGW%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDrMp5pIUOG7NhUJVuqSSw%2BtfxzmkVp%2By1sTorJesFzmAIgG3shvmMhFGNp81r%2Fy3DYtQBkwhCECwG1FI3F7acQAxMqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCae8VJn681akvf6dyrcAwweHxfCS9Ngs9ECgzxGmqFVbklvQSg2LB4KsB7cHlcWebARVh4poe7hUuxqVqbiPhAm2Nz%2FGKfUArCMa5ksntxLGtd8NbOJ0bw%2BWQ3pxaqjj2KSWPoK6gdhFjhZJuGUgLKO3yaFDPbflDuGHQ8NoYrMzgT8TzFW04C6w%2By5ktNRRcJg2s9azVtZjID%2FmD%2F6%2FzeRqtcf1675XQo%2FqCCx20yF9NaFKRVXULjxdvRRzqJGZ7gzIT98ygk%2F8ymtV0IV0yFfvMoiUp%2BOp4endbnl81Fsy8pnlCAN77%2Bjh7u5NDjtHiOyNDcMqvdIgjUfDGMG83sx2Bxw7N8NSDM174LOEgSYj4Xe22z4Rr%2B7qWCPs2V%2Ff9NX840uCV8%2Ffmn2ieIYfWN3LKLPO%2FUM9tWEwieTXlCH2zpU0OvGss9fhdOAmaTUzhyeOgoyKGoxXQcJCQqkC6j9%2Be87yabc%2BI8LOVjA2uTpUX%2BS%2B1KSLxqWIWuq4ymwM%2Fg%2FJEuC1vWzdd%2BzvW8qi1NKzPQwzZuW7MKx7p2DLU0gtY3VKHHSR7VFkeCsW0EjtCbrdgoZtpcFErpKLJdHCtvWzVsW6B8ZGtxuCsGhdUdwmJ6jpgtkjw743LtIt1drD%2FScGwrqXD3r0pmQMIDZwscGOqUBjjVzNzhFzW5LNeeITFyoPXQXh4zfGYn%2B%2FeIR%2Bm9QspIQSQBXLsoBxdToTYV2MhEB9O8XpBP7Sgue6PE1RTvPhS6XmgLJb5K8KfW1ectQ7EYqNo0TyWidVT8pT76gSBOivlxjYZsFS8nT7mTXQQ3zqo%2Fji%2FsNeH21EgkCfXcXZKMY%2F2xxCk2l8WnzIHDls8r0qsXl7x0aB3fUlwPp9HLF%2FGi%2BdwhU&X-Amz-Signature=e9c84a0fba66a6d2fb0c09702b453d37c2f0ba99d10d026a09670c71f1a0a7e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


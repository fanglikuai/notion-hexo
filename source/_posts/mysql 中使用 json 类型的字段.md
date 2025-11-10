---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RWNSE2LE%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T010057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIQDK9myl36veaAW2X7Buw4fmZ65%2Fa2EeEpd30VnF9KZ5BgIgMTxRgG86SLeil2VFWU80fp502P%2F50HCU0Mzx9bS9KtgqiAQI%2Bv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNK6kB%2FdmJebxub8GCrcA%2FldPO9AFIMwY9k9OE7JJVnZ7K%2BBrSjsxkPFY%2FUmmP6KOl8%2FEUQ6%2F2%2FS5oBVqv4cGiiLjGomXE0z0yHXK3yAijS%2F4020B%2FopNlMhCrN7cN%2BH5W3XxI1QaL0kgcSSysZgzGw13dKzJByok8dKDw1LTlCmT65NIW1BaBKqZt3nMV90YuDGJwcp7tQ1GVSS8O74d%2F%2FSW28oBH2usX%2FlT539H6Ve87t2jkYwLtng9ieGqdXg7ezMzPZmNdH1UA1GqUiGx2YXIkpIkEzJLPSdcrYL5V8avE4q8E0JdYAaduTk%2BpbK1jlaUjTp1uM%2F0IfJ7u2sdeGRjR4Ad40Kbix8IxYF9Hr4k7qlLqtF1Lg6ovB985S%2B6eBoogvB34561%2BBmqVXrta3lS3cgunyQfv10yObRo%2B9n%2B%2FbXeauNai8QsynjfbQVNLKivwVJx3Lf7urhljk110K2%2BoaBw5xrAfzlfLbZiGrI5DbZYPxFpKFspd4KWbG%2BPn3WxM7p3qZrJcAHRip%2BudZE5BEspkbKBimJe%2BEIA4BUR5xQ%2FAbAwPEgZUzVeWlqR46M71%2FrVVZvXcBq86ItmpBe6hew%2B4WCpXwrwVvL2H9n%2BV%2B9ka7Yyrk4NNowKWatbiVbdZqXnaSjsZaUMLnvxMgGOqUBzY81LJhp%2Bo0XDhCgXJEolfIRjv6yu%2BpNuAsc5Nb88iStNRfjuWZHnVvBgJXxadbHgo9U8C5qg42Wm8AzM6geUY5RF7l3jNb5HJIfAFaQ8KUoDr8OSqfStsQhPasjhWXa2f2Z%2FD06sYvD8bieYjZaAwsIoJIs%2B6smizkOEcER8fBf26gL%2Bs0%2FCRWGFbHYaNsiJeC4ip4WRUPNcpMXPc%2BpkT3r8epH&X-Amz-Signature=8704592465626ba5db6807cdd0cea3c17dcbc3b111e9ab788ad5dd876e3db39d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


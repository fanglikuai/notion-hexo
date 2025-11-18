---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666SARI2S5%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHjwcYi2fZwftqr5nSsSfvWXMQMeHSpKx8byklsOSOI4AiEAg3gkTl%2BSICfvF35zwThBwXOfsHkRTzRztR4avP%2BIIDwqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFKHjZxL4J26PtEJ%2FircA2nWhKPvw4SaYr1a6wdXhjnzXMNiBR9aFlxeBg1ju3N2gddB8mG3xngYNCLn7DGyFbX2UpBEOYtjWdfsy71UEegtXjk3ZHaN1ypJz0f21L%2FhGF5JNOOix9GVHLaIyS6%2FKLm5lp2k%2FUbC3t1FqmcH40AZI1BF01p8OoEiiHG8kLKHCs5LJOkVw9HZwo8RfDOo9iq%2FpIX6Cn7GDGwDlS2wuwIStpmhx9XTH4N%2FlGTITuZXNdg%2FhxeQKY5zDfelefcQ%2BaFrAeGQ7aVjt81rNrUP3wx6D3xF12Mnvj4elSbe4E6lzfd%2FmSg4ZB6UQ3Wu85Zq9FM5mkoUsj5E1XdbQiJ6OCXv50enr%2BMaO9vNjDb4CgSjNMS4TyccnGD01QLGx%2F2E6IXriD1PqJS04XekqA4Zlm6o7Kb2ODR0SPS19hULLZsokoxbeHo2YNG%2FwugcuYp%2B4HNA8Yg6AGXtWB8aY4WeF%2B8AZ8Ma8%2FRxtWMY9IotxA%2Bq7KKEwBax76jZuUYoyuQ2rgUu8k%2BrXlG0rA8TB8oFzjBHaHgiXEjIVoETr1hpE2ZvmA4LvuLKhlQr2bQaxg03gNbTKOCJ%2BXp%2FLpbRzvXILON520dcSU8OXy1zHEc%2BHyh87rTrl54%2F88HvMyWqMI7E8MgGOqUB%2BAob9GbYrkEUvPGwLQ%2FGfWJN4F4Cgy4UV0s5QsSKs1tw9rljsjH8Wg68D%2By%2Bw4R%2F7M0fKOkTXlGRhXNchjkQn5hv5vQ5vB14h2txF5Lwvjp8T1Hmd%2FAMFXZcJJBQVRDNlQVWvhyr%2Fo90VrA2IePcMx4WBcOGnZEFik0FsMxaeHRr%2BGIj7tym%2BzXpPko6itMsqXQ9VihXZedLpbwA9HKcz5t6ZWEv&X-Amz-Signature=10b29ff3bb3e277c608bf57dc58f6fbcd4999ebb0fda73a651c055035d324f50&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


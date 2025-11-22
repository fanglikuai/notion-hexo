---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VSIV3DU%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFIaCXVzLXdlc3QtMiJIMEYCIQDOS%2Fla9JsW%2BWxoArncpraXXQvmD0tFfq%2BcOdGzRC2XZAIhAOEHEdFfVq%2BKOdz%2BDW2FQDhz3ZdYaltBa01Ckq4ptdqCKv8DCBsQABoMNjM3NDIzMTgzODA1IgzQ8je%2FvC3aFuIyLIQq3AMoUlKQDuXIoDnbeMFd49jG6gUahMhmM6%2BVW5T0Szh0h6%2FCAoJMMHPs2EZ%2BCNdtscA%2F3WKQRT9sJwaNUyWWLsypVRZCvJO8bMNjJ1Ct6PQEby3NOh3sLdWsuWfr4DXjXPNKzhXXzzPHrPrendAhnt8cdp7DALR%2FngCkTzQzay%2FUbk%2Fphfe8ensdHCMjr5Cwzxtmszk4nzBAjxwBuKCoY0Eo3L%2F5sYb8afuahFvY9akR4AbewfbQbX6LSpqwW2pcfcZ5vgV1CJP6q03jQeBWx%2Bhw63Nd5PTMIYsF3b6q5TvjxbVSlqERQZ6Qma4rw30FjGpxcnZoG%2B9IAqplMAr5JTKm5%2Faoqm62Sc4ISvGD9Zu7C09%2F9la2eGqshXH3Gh5nAVstnGGu4TGQE2bqJ9jfrVevcHV%2BIS8tLi5qMj2lsR6tIh3ExOpvKnpOuPFWtxsNP2kq7gJLc070LzNwWFge%2FCFggpxGpONCpzm4cl2U3S13lSxuuPlO6tJKcEUWqtwtuhbolM5xTt3vyTuNWlvgJMFZgpRvcr1vX5FR0Ud6tz5zMv18tiDZ4Dlomu%2BfDeplWhqEYQzfxs%2F1QQckJdXPY5c5W2j4aBnY8FDfTS0LUcCPCF%2Fo40aK3pjQlmvsJzCQq4TJBjqkAeN1tMZVJllBfgpcFF%2B3404hzNv%2FGAM1m%2BntNpMOecDLgD4fz4gzebi8xwojj1zpjcdjbaCpeWqBaAxnj9Fs0hEXS27fG3L57t2%2BDzI9eER3jISahCxHHDGQCs9W3F%2FtY0TVykIl%2FIGHp6nyHu%2BnaullK9HsNEaT9VcHiDxn3dHtpX260TSHwT5IrEcb0cuwl4%2B80UCQg93Xoou4mHFIBiCl7ALn&X-Amz-Signature=ea702dcc30ed5ac55bc365a283efec9db3baa710067c8fd462967830944341f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


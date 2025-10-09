---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOHMJBTI%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T010113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQDtTEJPjtEfkfLTTvjDmxE7zrgArMcxur0U7jU4xoD%2BxgIhAIoGEF7e87gSrWzmaSbl%2FGJDPDxDScWZoglutRmHWyhNKogECMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwlLtbu%2B1VQxMO0D08q3AOR4g1OJ1KyVN%2Fd6Dv%2BlwDixGLeRy37egNLvBi0Mo%2FqyJmlIvR9q5o8Epe2dN58OrcvbGRUlZmTeVNvyzyGMz6s7Kt8JYtOp2PbV2zJLAX0yamMGLaVNuGs2X3fa%2FUzFhUuJ%2BStCQ7GBN14Xr0ThC8cVfne7DJJxDJdc6w7LvDy3vZk8YcFUhKPSiUtp5qu8vbCojdMylejGOzk37A38jemG%2FsCepOil%2Ftm%2FjtZ45kXDfHpFuNClatQtk3k1NAGoDNJh6igfnufPu%2F93r1uh%2F8yqb6OyECMHxz5OmaZQg9zL2HcO2rZHWRzFxRlgOn6Rgot6Kk3Pn5XcSCM6hRnacFdZNsKokdwj3xIUVKBXZCOykJ51QJILcx8naFyzFRFt%2F%2F00lZ0hZJst%2FBW%2FriOC2SJoMJV9HWUNsk72DnjQeOswKTzwaykgBgQDCDBGeg0y5EJ1AG%2BENTxVn43NtEdzAOgpjUbMqKTbftXAn%2BEuU%2FKdtNww46Qm%2Fo4vD18GL2geCu0Lp6A4hYqxFObwLcQIKAf4XqFyQ1JBteNpQwV12ZrCIu6HUqweeWeevhTS98cYP3typDTq8gRLT5vrWtNsTakWHmMGMut0EQwYc6SqFsCO%2FOrl%2FU3i7gC7mem9DDMhpzHBjqkAa%2FR%2F19%2FKInWoXERzJ5mAkk9EDJR7tsnbtgMQOKTX4y%2FSqAEUz3gG%2FE%2BiZkn2663OleUdcNdnHV6C0%2BJ0MFIK%2B8xaNS35faBhaR8H%2FOvNPLqJRLAffZT6eDzPpgbh0uGzh1llxfB9phJpFt2FZydhijD2YbMXGVJHX5FPc%2BrvXzQYHASjlZOXXJWSm0Rz5vGPHMo31%2BGkdtpMGWsumUYlWxiOKd2&X-Amz-Signature=b431e0c54834ae4604547c46fe8c498e7df7b82028a4673dd3ea831fb53d0a01&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


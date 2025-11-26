---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CVGPT47%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaenjbkvNrVvol3ngA%2Ft4cgdb4dPZrQkbnTfckqsn6qAIgUM%2FKKalYTDwTGkPXX97FV%2Fb6bEAZ2Ltz%2BlT6y5Ou4rQqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIN2cCWoxAHM0BeVzSrcA99DkkFeF3%2BEk9LpAlMmzD8%2BcRCbJUSoBodPGMpLxLZIhVJFPY3ZKSq9NGVkxu0QQQ9A8pDLjF5JKK4O0ffIbWTKoHdka%2BJvvGrBKhQb16A2zk8UBIyU9%2FjpulU1bS8j18KqJw43vc0qg%2F2sV7fvRKqZdR4QlD3GVp2fcGL26IaRD9cA9xxlsuqj3p%2FNVumIjXKEDgJGcYOgpLUe%2FkXLlweilK39jECnEGLlNGBYdV3A%2B09v0wy9NnMHVtnQ2sRQpeEsSSuoQoGfxyQcPi8msTF5rJff5nyxa6Sd%2Fs3y8XCy2KcjyG0rxSLvhxzt6oEppKxlj8hBmODAAAtB04UA1%2Bs5mPuKetgF%2BixfFpmbgZls2CY5WSpwkw4S%2FrLx7mSok5TJdlwUuhxBU9De%2FF2uo7mEcRxO3%2FsMtbzSZw6oy%2FQO%2FyctPeVN4lqJwjdqTyaF%2BMz3ztdLFZeKtXknJnVkHkevVnETeULCeg5BXXdfmAPoQNeLc7eLTkKyhhAi9MOuOgTzZNhMnnu9XF7FwZeEpAXSFMTqQ1HxZ2XP0jh7FXeTEaeJqV4yFlzPLni44zbbI%2BOOGWM%2F2PN6qwtgiVUVYGYdPWmBtpn%2BFnzLsg9fJFNaI1UzR5nMOx9SaCGuMIbrmskGOqUB2BjBZC7g0djdgeNr0lD1RVwjVWxUhXqP5Ipl6PbCMlm1F38RJxpPFwestXOY2ICL%2FNV6rpjwyoaAAqVVShiY3MuWl34F0P7m4Ydq7Q1CpO%2BFay%2FN8KxEBkZyJrc0eA1NCjb7tmFRoRsrSS1slciQzAzoFGVLje89hXyqnvKwftOsh9IWqVThh2%2F2jz6%2FtH7%2FhT00OXqznrzRX8T15Y9V1er%2BGQMJ&X-Amz-Signature=3fe718674d641d96330f31b1e4e33722234cc08bee92ff5fc1ae57f010eba364&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


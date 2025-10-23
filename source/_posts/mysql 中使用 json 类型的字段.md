---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666B7V7AGV%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE3VWj2rZ5EuSch0tJg9IJ9QIFXORxlHjSM1xhmS%2BH2FAiEAlRfPivtJv7nml4IDSZP8TAFbVqKxVfSf9tWJEMw0sB4q%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDHa76j%2F8HDL28Q7VDCrcAxTbl43hv9kOJtn5%2BcCWku4n7lfAkEeAOmLjfu0JUCobRQYF9fH1yPPdXRYYv08QJNK55Kl0TrOSK9Wr4KRt7gDeADliv%2F9LxIv2C9XHEw36Qg2os3MewCGoxlj2ZTap5ppPOnEpK7GiAxqTzSJKRfXtoPqqbKfOS5pynp6BMiyMyY%2FEwupl1QRVNgg4QJiBsHnEgcD4i3cyohiMYHN3rkOpWUdMNyH3F9ywWV7hlp%2F12sdrUePtnlaW%2ByVqm6JadqyinD68g4A01YT1ANeLFdZZWSTJLE9SctU12xjXdyJmXRHRB3JBKaaP%2Bd5wylMt4iAi2u5q7Xp%2FCDmNmalN%2FQ3GM1FbOHpTV8CKF5lKGpt8UxGYdPgES0dmedIsazHG6NjfR5XSrtcsemyNxgg3YVvcSJju1myCFqBQY29aIa%2BEs87yXoW5VPX2%2B6eHPX%2FowYflCpe0AFxIUnJxmVLjp0bcq3F39M32jZkbUpGi6%2FmzcrBFULb5vEhXRmQAUMHoOmfEmXMJUkEJK5HEcWr9k4fmuGY3JZVwZ79JSLPLkQA9Kz5XtfZWJ2ZCDFIiYQKv8brOuOJ9t68W8wSPPWWIhwFokt14YWcYHqqjfGgoRcQzH2Nq0%2BWTjc6DJx5uMIy%2F6ccGOqUBaKpl%2FprCsVA86vI7PSWq6Movis%2FiFdZCuZ113AlcDjbauEE3nqpsr0EvkJfgUZeEdJy79Jhi8ksTkvYp%2B4Th7wvSgmmFFQCie18azJaJ1x%2FT8sSu%2FaUFRDI5WqD6AjfVLePYU00f7XMzvs8SFjndh15mZrHmrAr3L9K8YBsWbAcwqBibB%2BNAL4pBV9ZjAJaNHdaJAS89Mcrp09ZRRJqBmrhVc339&X-Amz-Signature=81d99c8e31b48cc7b1f350d1fb5110f3ab71e7510875597c2f81236491913beb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


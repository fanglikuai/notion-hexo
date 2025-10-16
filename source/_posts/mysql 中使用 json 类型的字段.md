---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJ5XBW6N%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T230126Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGHnfpSK%2F7gLX1futJUkcA7DL%2BRW2cGFi0wx1Cq1tYqWAiBrJqG%2BeQXjwIUhX8S8QZ58Qrz8PSdEUHT9IutYOVuVfSqIBAiX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIo2UCPKHrxTR3or4KtwDl7jkvMDrJL05Qdk9VfoHeMd8bnVFREyyPW%2Fj5EWAP%2FpaxZPv5aIL4QWb%2Bo%2Fn%2BmLBAIM15u4fUaSsq1k%2BW6paTeBWVSPr9Evly%2BXOSN0vBW6pKpN053DaGLbS7q5vq0QNfHueo8pfjRDQ1yAr3ZyTr%2Ba2jGux5jev6V1QRbf2QSFZmu1xaptBgf5fZ7YY6vA53%2FSAZ2lbyc%2Fz%2BGpomGVStv%2F0uxgCcO19S0UUHHMyjdEcMHIlhwxHKGl1TT4SIInjgdfs%2BGKgECVzoJNqtcB0nnJlamU%2Bpg14iRVUWi0ZBbTLUGDH6DOY%2BLFdVcyBKpmSQxZL5UacE9MxmOM95VuEutEvkirxd8kunheLdawDmJ9SSJJ8htH59z5n1wv4XvOGXT2380Dz9dT5jgzQRajW50wSNve8%2BnAghN2TtJCuVTlFXyJ79bDK8UQEr0Pvc3rWrmhnIbE2wnyxOlYeLjTExkp%2F2Ww9nRyOq1Jquxqc2fFKq1orT4SM9HGNUQ5WHbF0P4DqBPRRtVx6EDtl7YSpUXRAdKiYv%2F633aiXsAjoSj0S5QnwCYq4IizP0RjQidDHaRUE7AQz9lGnuGIzoeU0nEgiuxzGmRfCXHn7Lbl%2F5NbUCsOBVtPHH%2BOk87YwmNjFxwY6pgHk8uVATZtBeB0WY16TMQJk%2BZxG5dEGh3j2bDwk2hJXPcoR6AWjRfGsSxWrAP6BnqvSu6RupUlayal1OZf%2B1uXBBBZMmVtzs7%2FZ2ngktA9IRv19LdZEKwtPEEy7vjL5jpyANAX7ok%2F74ZjDcIlWSyxwOZgFFkbeJEZ2ktff6Jzx89AS5y4s4VOwsH1vBOfq0%2BgdzloZqVzuJ2P5qxjNsTc1%2BO44srHK&X-Amz-Signature=d160084a9d596d1da96e21fb2281ce0bc4aa1d92230b8583693c52d36bc6b9ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


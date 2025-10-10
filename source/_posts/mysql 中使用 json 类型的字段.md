---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WIGTJBY%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJIMEYCIQCJH8xTjt%2FR0DqKW1iBOH6f8p4VrGDXiSBor0tr2bUOpQIhAObNCB3fZkPA4PcAAwVJdpABGFIBPTVUNvwZ%2FQDN0qyRKogECPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzVcpxkMXJeWnuImT8q3APTtZF3wfNbfyJsezc9Z5JYORXrmA2RL%2BJxEQtjj20DfRa%2Bu%2BvSxV29WCFVt%2F8uaG1PP5bTk8d8Yt8wTUwwaK6Vdc8IU8T5wJYG7NimUdnA7a%2F75b1MSjfF5syneh9zsNkO4OQDy0XXGbAxeH%2FfTF29rxEMxrgMhPx8nhsjC76lpGFXzts6ZUcP2x%2FkpMpcbqlv%2FcbuZ%2BdW%2BwuxdexIMNTG39S0ugTg3bdjB2HQM8KUDUqoDgZgHJhamTXtnMvdh2nd%2BFNr7pE2MKDmCi6L1jIraTEP00DW7oHPP7wi%2F14Ul88y3Xpq7HCTj1cb7nGD%2FLWcNGjDoypngkwFY1JDaMvuexEDKpELkhZhOP9%2B0do2a7XtQXyl7oI%2FVTajNXqcr5pr4d%2BWn%2BE%2FKsy5L%2Bl5o%2BWtY7AaYGCzWKw%2FyIU%2B9GA6JCxs9jb7gf%2BbCJVj1CLJ2o4owFYVTgQVKKGQkphS3vxC877MuYaU%2FROd3sK8r8zI9egQMK%2F85z3YVanqOu4ZVfVrG0bAvDvrr%2FiRoYj676hSg2id5CxWwV1I8OOORxkG0IQv%2FY11qT8%2BIuE%2Br5hOjT0IFROU0tW3riagGIf%2BDA0%2FIdKwqkXpRxFDl0TJM8vQrYrIFLiql0trJ%2F2LDTDf36XHBjqkAWhEGmuTkX2jYsKqGyrFC3dW135uC5LfJ9%2FcGBU2Lm8QXykk5tPCrOC2BRnL5umT2hN%2BUt7GCGM4lD5q9kLY1A9jAVVICkRMpHyYh%2FLaaS1JJBrSx5T%2BQAA8rWZL8%2FRFkYRrLdTvHz1Knljn%2FS07Uz1yPhtQ66FjSJQBocSswILsubuiifkpFMlFxlycAfpf40iWshBh%2By7e2NeEaR19%2BMfoNVDe&X-Amz-Signature=ca83b792114acabd94f5d40f2752900e756ab44aae24cd81bed0197bbb8368a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


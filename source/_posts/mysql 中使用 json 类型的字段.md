---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662T2DXC6G%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7adOUJvnX2vo0RI62eIsMOxaBJtDOheADcYyAaqofgAIgcFJb3hpVD%2FKTqLFMtATfYfmlOcK64XxAob4kE15yp8MqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJrIGe3%2BaLWz0X8UUircA9bR8DokJEBVLNOZD3s09ooNnDjE%2BUfvGQFG1a6uwkvecPrre5GzMXNLA846567ZfuKe0p8nOWmW7S0s5c1wLT2Jwd4p3C4KpmPGk9z%2Bo%2FUo4dsCGMyWQIdtidT82sXmmOAGpiS%2F3aXIoXgfym7uFuKPcl0Ye%2BdlR1B8%2BIJMoljkrOVKoqb7YlphLRCfeKBfcY%2FghoIg%2B83HZcslkHnj5RImmZ4vrQ97h%2B0J5iQ2ao%2Fm4Pnlt%2FUpkyzKHIp09CSJFdUW0F8GcOFeieWszLN97OQYYtjRkqIkccvo44lHt1PecA%2FkMhe3U9%2BMBqoQb8bJ8BurlzyAmW6UPfhgG2%2BSjWlgZDjh5rkgwObVt62ko6tRJxIc5w11BRCxgm7PZXGkW10kjR54PXT7Q1ANCzbQGdsqWCHHQEqOaE2B7Zc0M3TpZnbw9r1KX6c%2B4DpQb%2BnzT4BcfVsy6B7vyJun0fjBYY4JEJ1ZHRnPUJNLPbMPuWHTfHJ9o8ehAiPPcgRDjE9ZGQxDS4I3Bu7usN6IkkIN4RiKgSf3RyNzQBjn3PoFSSgy6INmvZeck9CJEnM%2Fhy2XbGz2QBqDxoy5Udgi7cKB9GS1Abh8KsulPVXO8eXnNE6bVfwviTrcQjpBSV7TMK%2Bi78gGOqUBFBGge%2B9feeBrx6JAEgpkwVXdEfpLQ7bc8XARIA8p%2FvQlOu36%2B0njyrOHa6mIQcqJWJ%2BbznMDmMPVNK9B8fThQsmD%2FEVtUH5zdMsvJOxYNZ79eg9bJjlfcQztbL49pESSEHx%2F3AvvAxtV9gagqQHBfo4wl%2B0jSfaNezIFZo89L%2B9cb2rOodZJ3xvpAWaN4cuURkBIvCTTfgD5KQ%2BsiJaXctlYD08e&X-Amz-Signature=3533e12937f096ce013ac5f030fe525d49662c7226c6005602d8b7b8cd32cc5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


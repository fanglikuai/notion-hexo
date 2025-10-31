---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SWJB6M3S%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T020046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJIMEYCIQD2EqmzMyOv9oq%2FDACajNnPlyx5ZsNeU3bJ%2BrLQPku7xwIhAOWWcK9u95rTOLrk1Q2%2FVXQoTkbdegfhkXipz5Wt0vhXKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igya2rFdZ2oUd9R%2BG84q3AOHUdPBSKjoMDfXenMHzgPoQTXbzl0KqHbGL2TNt6D6rld%2FoOTlWVyJAn3dgo%2BFB0WWy04VXiniLdT3KNHof%2FDOWqq1o9KQyYdT4vbFSE5pcS2OKbw2RDcVrFRP8vUM1LstYDHdxWrTqupnLlvcC%2FbnZyELTTvDrPuc%2Fd%2FaPh5w%2FSZQTG192BuEfRkwfUViGttvzpX7KyIlsyYNt7PGdWpEHAmhIN4Q7s4950zKsgVgAWkYN6ReFkzvvZvNXOpxHKe1AnwK7uzu3tajNHQf9UvwHw%2BbOfHC0PtXAEosuXovlioX4WOdPxBKJFDCSCKkJMVQR32Me%2B0EHcgOm5bgiMYKy5sq9EIQuR2%2Bm%2FFoYyalsKXUBgOouAbRTPqeNerRDqApAzTXuodioBGOODeuQi2GsicgjU0K%2Bo40nKZLPSG%2BA96MSZn4pvOdtXe7E8f57SEi3ornsKh715LJIAqx%2FUPWuEai2WsvNukOmR77%2BmRDNHRx6MCVtFKCNSo73M6XQeOVOY0L%2FUv40Fdn%2BHo9IavJ5ay2IlXEEnUzFEE7sG3gPcG5t%2F7v%2FrWlQXO3X5i3P%2B2axQW%2F6Za6CqANP9iOZ9lVuvPr50zwOP2nTZEHub3Ile6qOqfDK7NayAC2jTCbk5DIBjqkAQs727pKWA9OmlE8UFv4MTSDq36lQ6t2hMJimqs%2BMaeUBj1Aa%2BbCPZ%2FXiON8Q3qsUfeZ%2FCRsWzEMqStnrPXOSrjJEwP8kxLyuArwkPUDnmAQ9PKRbuYxc6ogeDdIcnlNZKQhL4NS7M8PKV9W5MX73hEuBHNa8EdSfWsJNhtFsajf3%2F%2BfYxaAL8kvJWJhnHbTsecwCqs1m3jW%2B1ko4NXLLp5QnKbc&X-Amz-Signature=c71a3aea94fa6d6155792a0eec1a03cf9d6b553e129e9fbcaa247d2a7309d4dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


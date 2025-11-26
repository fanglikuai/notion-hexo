---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U763VWBO%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFL3BlfR91gZrkq7ldPxKlZ5N68grefRQ1U8KvtrSlwyAiEAjpLdrtLdAgMjn5ZOGlQnTk28x2UhQimZGZCetZGuAGMqiAQIiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI9z5XXu2DhhS3mS%2FircA%2BzvZCWOYkrhzUhA27%2BacHH04aSRVfqpsxbIkpLa8Y8BO1iGwzcAz4Dwo9DNUxQRKjGecrUvr7YbrwBbEI57Va8Uw21ETOw76C3BF3d9%2BR2TtfCfu1ch12GpDR1ZZbUzgqM%2BWrqOzpX5cGoqJRV6FsT%2BXZun2uMbME2NPa%2BZ0xl47hyarb8DK7FmTVkslkBmUE8lAYMUH9ElG%2Biss9UAUjF2NH%2FDDrxFH%2BBwqNLQh%2Bv9pvhj%2Fmn204d2bGjSH0%2FpDXK2MrFUH3viAKDcWRwuIffj9GKhBeCJB%2FJiwcqAxyG3ZNc6uWpcioryH1HsFvIQcAqEWWWwdHl0yz5esbsU77cYDMiD8nWe%2Fa9WvMqLUxP%2Fs9sR5nxXrJ3T7uKqgAtsWVzVXQ8d7AuMuf3OfjQNXf2E%2B3uELZAT7XaaKCIlT5NQf3fyIWsLC8MSjZg8kucjLB38G57xmcChVQlW76ti5pG4OPi4UGWO8yVnc2eBAFeoaxRjuPTBfuAhFCLJDDwSnRMiWhsTCR1n4WcH9X2ehtF9SrOkfi3Fqo7%2FMEtDJER%2FKmsZTacg12VUcLKBLsQYBuQu7iEZ5j%2FnURaFyOuWgsEf4ao3wnlHSvTb9QD%2BWZgAPf7scwzB2YdBpSheMIyonMkGOqUBmvSSjSNENQDW8RTqulRIo9Cdud%2BiEeH%2BnLQPY3iTrujz%2FJAaZrAAYGgGL06%2FPJIRGzideGHtk61NwfdMc1bJ58owJalCiz77wKxxc4PbJNewRwDsHfhOn9WHfOjK5my%2Bxn6i5YD1HdxM25Dl%2Bv6M%2F35e7wf3KXKlSWpOsVayCIWhN0nMqVyqHEu3kFqQxSa003jF9WOYGN%2BOvFs7wQB%2FA84cxDtE&X-Amz-Signature=480aca1cd1524a16d78b9579d28250ebebe81afe04fcc4a191ea414c3d345877&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


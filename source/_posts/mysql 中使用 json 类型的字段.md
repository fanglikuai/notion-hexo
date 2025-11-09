---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FLIQAIK%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIQDY0krBwkeajlAMDvWeCHfk7o6%2BTLZzk8Ezft1hiZuV9QIgYxqTrSHSALIbmxxcykdn5D8yrckc3adzw2MaBps423YqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKepHBvCHmp4k88u1SrcA1MQDLhTjdaEPpb6yH%2Bj0r%2FL8f7nRyL%2B0mWG6hvV8TtUJ9ASgLFsKTsxZ37rGPgqVzqrpUBbTGGyKQNmipuAU5EahfKha%2FQfH8kFiITuGtLWD23yo40WSB6ojjrMfX9dDOGPmBZodTztt4U0a0ltqmHK3BLJJrIQulzOEJKjiKbP%2BRAfLstI%2F%2F1AEuR636%2B7EYKfkEtLqo2NHDi%2BCnr6q0P9ZMYkr%2F1MTdX%2B0d5qqazCR7YAtEP%2Fe8IPJDqEFHnTesAvO8rghwe5hW998tkEaqwtMmiE4tr3N2XHmaFXjzZFKVlfdI916Ir3IFmZlFfylA5ZLhdnBmAL9nC%2FtZ2nWHtYrdMyEOfMDBUPACgejuhOQLGXdnJ61HWor4br%2Bh%2FQcV%2BBVjakgbaAjo8LJOxxXAmFH0nFSfuyaxj2DRCqALh8%2B7Exm%2F3TAC6SolojtuWTrBp5aHoidedVXU9cRfQdSF78OYe0Rk0fs8Zk3wLb606B8EfGAdWFArZcEm%2FOcBlXCIRdEzOjHUwZ1VcidvCOn3xI5J7xmqu%2F5dUnHL3%2FfgMhcjfe3eGPCdNYZO93si8BAQ9ZIPzTTz0A90pUaRdq%2FMnTJQlGR%2FMcqwbeMm5HTjlpCl68dRFd3wXYSVrOMOCTwcgGOqUB70Z8HG29IjnhKHv26q2CQyo7Q%2Bl5amfFq1RHww%2Foov%2FZe3eXvhRwL1YGhABy8%2FO6dKt74tTfTiAHc3H94ACWi1mdeYWTguIkawHylCD%2Bz91mKcuaikSdL1A4Jh39pGwIOETZCdguV3SzO2RLBecjmqJxQO0tlT92HRO0eueyXZGcStsAMBjKG601tkF5J%2BA7Lv5EIV6IS1TZ42yNXh7FIJrkYgGR&X-Amz-Signature=cf361ea2f61c3b765b881a4927d1a7f3c697144bf6e01e8999a233265a9e0bb7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


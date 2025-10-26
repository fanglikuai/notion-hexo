---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJ2M3ZLM%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDA9Wz8TXS781U8Jiw6IF1XjbmG1CEhk1MPNGn0UGgMeAiEA6CEQGQf%2BnGIkoGbOR3Xxl00O0R2nT4dMi7N0WmP%2BG%2F4qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNutSz7psuMh4bUy0CrcAzkqluRfOajj2koG1IPsgibcEcirRcKWVdovmFnRau1Hg9QOueJ6%2Fok61WOD2%2FxKEN%2FDyBeZMO9FF7tokJzCXCvOSoOhrPj%2Bp4yOYiauuQ%2BEHJg9nGNP9IVIGlGhdOBwN%2Br34kGjicC23vZ6YVKIqhIJYuS7giDNInSnAuaLGxzS8tVAhmD5FEaTsgwFI80%2B26BpIltKLFacisBSILjQV%2F5cxWT%2Bw5jVUb3%2FUkNjJQDvj3dSyw4tD1XUeTYWezqLbvUdcIvT4ROPGxDfAtpyIBJ1psyWi%2F93Eikl%2FlWVgiiSR6oEr2TXPTXVNuej%2BclpY5BpyilHM8QAn9U65xSu8r0aJm0cYJ3Bigv7KvF4BMusk1mSSJNM3%2FretonkIZ8zz2tQ1wa1ZJeV%2BMY95ARJ3igwJBQjEmpk9oj%2BS%2FtmgSPf8TqsvVobsdrFhYo%2BpsVWyBqgRD5HVN5nvgC2EyyaSiYs2FMPu5DzM4KYjPA08llx0Cj7dVfGmwiXIXdnpQEVRh39FB4jm24M7b4DrzA96dMzSgFDF3SbNchK18jp%2BQNDcSc5poXJ%2F5m5jRgXFlYtzpIDU1fcr2Ti8%2FpNVZ9fHrd%2BkZDBKE2D64l3sxl%2F4mc0m%2BkeXzdDY1MMfiWCMJeR%2BscGOqUBtgne%2FUjd%2F%2FB4PYVUdxzFEDmoB8k4AbPNgxsnXCX1xpPls3N36Mbq07MR1Zze1igsVYNkptF5honuyyIPBIArIk9ltR9oXJT1ty02Fvu8muq%2FsOgOSm4nwBjRB4tBjOK9bOQKoPvcm1Hmka036qPXcTj7Dy8z6VX3WqJQCaXECu7ZF5HwtdFG35mNTCpRuPX1tuVnF0116OLkiq2139q1yYhUHwhA&X-Amz-Signature=9a5e41d94950707973f92a1d9c452b3d5357dbd032c8192f4cbda6b0cac51b90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


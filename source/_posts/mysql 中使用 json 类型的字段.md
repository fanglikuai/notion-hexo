---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JBBRLUA%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T010048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJGMEQCIEjBHL2LsBznpLS0Es%2BmWbCDUIv2bRwgqFzCeNzPFe7GAiAxFGA3SSY331n04TSNE6A2gKHdM5HVIpwS66QnH8mFTyr%2FAwgqEAAaDDYzNzQyMzE4MzgwNSIMBR%2BfTjmTGWWJRCToKtwDiXvJImEHX5PMN7xeaq9r9qCadzVIPEm7IICk1z9C4%2FBotKFoNo3ss7Q1%2F%2FoU34PTRVCvKyPsI%2FN3Go6kFtsAmYac0D5xut%2BrmRwm5oTn%2FhVDkP6ojjSKU0IEZn2upIpL9fbbIThiND%2FwCma5xAdMAgljcBdyZi6fC%2Fu6DCq4xpy6LiACEFHGVfrEU6Tql3QDd7Q%2FymWS5P%2FfoLPZNKvmjxmPVpeLdNnaXBmD3MTaDMR2FafN6xe1l5NvTOy4DKCdcVaOj9J1paPKtNRHD6E1acMyfZNmyVLRKeV%2BWONmbTSA6w9max8eobZw0w3OguwKYlZtJ0iQwlIbSKgMlxhbvu71ntfgppzrzZGDhXhjUZZp%2BRdKEzpL0NVERQAV0KFmNqFW6kZjqomAycRx7kg7f9cnKv%2FXMyMb5psecYnVOuZh52ATtWVcHmR278XmkIZzxmJ%2F1SJzPs92TU25EQk2jz4RDweIZNe%2Bs%2Bd96%2Fr8OCS7NFMtbNv49ILmw9EW5Ys6DklipTHMMSbdSZqhgIo%2BazrG1NohPsZwzo3v2FwwozE40kswNYjmdBdmMWM0CYfW3S12quIfqMOXUaC4Y5pgvT8VlxujNi3ib156fX1xBaRZ4pd9CL7zN%2BkthZgwq6vPyAY6pgEF1YbEsSnd99oxVmZ6mJI01EydDnX4TYToG4ZsGmvbrRzilzr%2Floiftdm1tPx6JCoOtBCJN0l7Qpb7HvmfdSdrrQWCfn%2BeXS2jxnEawe4C%2B6VcQqjs5dXdZxA3%2FuZZeEt1mL4dtg5UscqLEOEItVpM1p2osaTvlUvKRh7GSOUZAptvpeIkYnuoYAnJ7vsMqm0y7dRqljX2vk7%2FSWy%2FYm%2B9TdXxPnrU&X-Amz-Signature=699587d2e21366420ea49d323132b4ff2c79c6e2e9b42d88d41c894b50628e8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


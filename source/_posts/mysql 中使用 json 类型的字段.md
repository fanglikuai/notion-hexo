---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IUX2K6E%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIFA4PuHoDpYg76SE6qdX8wXmKJEJhrgZWgNwrSAQd%2FlVAiBXNBHq5HbLv7KM6JNHGz6H3rMeebOGJej%2BFsSZpMCk2CqIBAib%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTtfGFokEju9vrnT5KtwD2EDLy9aMX93HcOGbKju9UphXMsV54xMOLwBQX%2FMZPa6WnFrJac9fvGcljdsK9H08NE%2BHEvJFr%2F2UQZVbFIE6sWm%2Fzyn0m4sDllWl5RFFNOnwJW5E4pPiCT1OKT3UUm%2BXAizwWuzN%2Bwia94adLBpgih4UjHolBE26AMBmKnVyAOXIXJNgh95nziokBh7lPOn%2FypzYLY6Eq0ffC9xgC7UQZjaABtw9cDrqKIkoFvCZrRS%2FjUuZvoPZtmb0A3E6QJjKH0WKICQgMluaSIWeYpsoeDO%2Fbx46Oe%2FXqNfDpSRYWVG2WJHl2ydKK5CC%2BkNkGSdXchjXgChWM%2BNyD66tZJn0P3co5S%2FQDNuaZImFh3uu1%2BS1Rk0sSqTmhI%2BoOPHKNQ1vcgSUAdOz11%2B4K3%2FdRnLuGcM7oA9h1fEOGigm94PpaOavyGu%2BWpVD%2F6Z9G61Hsa1LoI8a%2FWS0xxJULRmPLUp3ln%2BllLOurB99RjewA1WomIrup7RnwMkLUKtAzs3GWZGTL7kfJG79rJ37GOtINeUr1VyaWP6KGl5zCMIcjQ9TdIYLkk3hghfaA2eC%2FFAR2f5zHzW4LP%2BPFSp%2BE9bp1uUbrREe9c%2FFPqMehkFStrjFUFissBVW1YqA91V%2BVmUw2rSoxgY6pgEBcPD2PZ%2BHWosQyDH7qOJNotXDhxiWSM9V%2B4oQm6bwQZw%2FqGGj9spMnc%2FLzzzLekNhRZMAycDImibJNH%2FUvxNrrX9DbCptfOmQVehvalbAEJERoeGZJlGyib7iPsUTz1DpNNm55sjvDTXf7VEa0FD8UKFqFoREJvmquVtGaD5oBPXhkpfKtcbSkw2TGJDOxajy0Tbtcv1u%2FZvvAYBRyMfj4CUD7oJT&X-Amz-Signature=380b342591fd144b43cc953df18c404626e850c16504172c036483eb63c5a7fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466USCVHRXJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDqNgFDcoXQYATk7HlzSsUmUjU4DQXgHwQvtpANY3j9dAiBwAmoFfFxBS8gnskjtkeSm5yHG5RZqRhJqOxr2CWXLSyr%2FAwhOEAAaDDYzNzQyMzE4MzgwNSIM28n%2BC7j8%2BeVj7BL%2BKtwDi6waA%2FEeVLOEHvzjKXthIkNjWqzuEmNQiJXH%2FPc%2BP5FrluHwvrNBV4Qy4n8qggH1bN1iXRVgjvmr%2FIAkoMXo2A9b1FOVJQZ13T1DP509559j3XKJM%2ByQGiml02TJjihDWBOOn69XieJv%2FtoMtFY0LDRLC92hV0XODna2ZkH1a7wpcaW4h0RRR6KsybnREIg1yu%2BFu%2FGPsbJieBR77TeiQr%2BCi9U%2BCCxkEqawk5GPetlF2aQQIAaTO0Y6G9eDkKzh96RfYtwJp7S%2F%2FCVVmbJmZGqdnU%2FqZPn4GyQ%2BLGRnrHH6Cb8hQa4yaKxAJ%2BMD%2Ffm4AAOlVsf19JkcjyUrLUEh%2Bh37rU3xJqs0meCFSPCJPs6DX8dF14BmSgexIhu9fcd5Gs19kFYWFzSBPbf1Bw8w%2B4MMlNyRHm1ujMw6oiwtnUrmCIKvtRYzOVRkzp6yX6Dihb37Xfl1gEFfF0hAcTBBXrFiDQQqO9C%2BGH8916sOhnCM6HLA%2BwrzRbl9K7sFe%2BJPAskI1Phcw%2BMVQ9xvItm4LkhqOLy1ELTF%2Bu4uVqGcgR4W0q4BpyBZs%2B3B5nF8jaxx6RVHfW8evcSXl0vzj6yzXXkFYqa4dm7eUtfSwXwwhjRhKOoVIl%2Bwu1G8XYww36HXyAY6pgF43WJ81dPhdm9AyRO5YTlp26rXBXNWJ1DBC7P9W9jByEWv1Gdo8vxI2K0rsAQg0clweUj6qUBYVoE8qQzq5rJr2iMRUtiGarkP%2BhTYckaiL8ko6WUx%2B4UO5GFaKJsvYvb1OXv6WtdKjVZ7O7TWsCfAVbxP2ECSQOi%2Bq2B8GUiXNSr2C0BjVMu0bEIGw67tO7RpioQJKQi5pJjomW8ZTOzAdrlWOhBI&X-Amz-Signature=137f5458851442102dc27253783fd378d478b847cd401af218216939a1b44fa0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


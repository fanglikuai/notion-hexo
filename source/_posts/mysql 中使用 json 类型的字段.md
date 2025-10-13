---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46644AKDUEG%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T090059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAShc7hiAnnw2F%2BN6%2FKn5R10zWXiStN18HVO2wkrsKIrAiBTqX21Zrg7sWK3a2vJTlgiRT89fAF40lPghJd1O9AENir%2FAwhBEAAaDDYzNzQyMzE4MzgwNSIMwzkKEwRJpg3LcbVsKtwDTyyEBLMgWR70bNMt%2BfSuziWdPcVoDDn0aBJx%2Fxb0aq8b2%2FZy%2FJ48ibTzAnmbKf%2FVVxPL%2BELLbYXr%2F1yjUn0Y0L8QXFHkP%2F7TdwSZL686xvDaprJebpi7M0nNip%2FFuIBZCDv%2FSfohkohkLtLfIBR6111QUB0EaBkbJCsOEfgZv84X6AV9vIXszVLP0yr1SguSZMylxiPZT5zThP%2FB6gkoDewOymqSEax3AE%2FCBLhq7zGdmyfEgVVtfzcXEOCW5hr9%2F0lJPux2anJPETA8s8HKrxjCfTyXFWL3gWxkl2TeQopTdv9OBngIdp1DIAL%2FCt46n5nLtgy5DgpQ9BEqLNOVg9BtwH5ddFaaZuwq4GGaBIiPJX4Iay%2FmvY6p37j1MSDVIekkjb1rWs1W4SVyoRUzvwB4BULd2mvUouPWBfg9YB41ejrkWoy3DQLq17strpyy%2BdnFCsVP%2BaAQdpXD56AW9Ve%2B4PJXYbzTURMvCJbE04LuIzUEolm2fJI22iHeq3pmZiiOxNDiZmqKqkI9kbwshkrRY0zMgSOTtD0P4%2FOeRh8fWLNB6Wl%2Bc2IaqtCGB4PGs08uJHF40o4kPeemRIwR5pxAhr8YaF9aQKhiIQY66JVSPOX2VvwQYdgf%2B3gwk%2BGyxwY6pgH57W8jDkz1uBj%2FZ1zQEz3YLsw%2FEhghiya9YDmF2TRToO206li5rWJPiUq18jtU5e32kGz1B4%2BkjFepkV3bzsyCgFFKaIe6tB%2FTxfMlp%2BAuM%2BERC74bb06jpd5WCOzMDSp9kmEIx7VAvh8XIg4wK25pMoiotGkr5OTSeRElTADDVJOGPtSkSqx9kL8wWeaO941bTAURhosgJPEwumNCBsuHBn%2FfAidv&X-Amz-Signature=3a27574bc887ff39cdc3f4e258158d3e238a4cbdb04f40568c4fdc067c4ce038&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


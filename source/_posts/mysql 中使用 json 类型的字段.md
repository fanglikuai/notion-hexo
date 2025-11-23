---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ZGZL7UW%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T050043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJGMEQCIE802NWvexTIBp84KGx6sNhpsHu%2BVT2FwGMOdIy6wAuFAiB0xn1OAmmJCIg6kb7SGtvva9wCtFkFPr8Qdl2FjYR93ir%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMRZyVUNmpcZz4baWLKtwDVSqh%2BiyPgp4BcDUjZVu%2Ft5oO5WEeeZm8PZyeGiEmsoyHxe8uTNZz6ga%2B4gPE%2Fjp51k4Rfby7HonTTll5Oq%2BXg4qLdX5Gse6dq3fMWqSCrarknTVHYaNOOukYIDog1paPaEQXMMUnmP9nsoVCwldCupDJfhAu1q%2B326Dy01y%2F2HehK0OcSiV4%2FoCr88w0zmS0gHbyxlMoZZxcXPRHEAOnZ7ALQoxW%2B1zo4hqpBxozIpHs3SzjJKuqdC8u5TFMcOMa7eFBpkruo8piFZ3RzuzZyuxNOLhMgx%2F0mf%2BMnq8YGdOaLDa4g7mP5aST6aIqyZr4pRrSzjTBhykuclc%2BVooBQguprCxZ%2BzlCTxG13E852SkecvYPPsYqPCTOxDBVPMUSopccCwt4ekGteuHLFnPJJBqB1523Z1gDLiz0Piz0Va2jvlTClwcIfZpM4TkXsEGbv2d47h6Bl2uxzvOcpkyiEuBtHVpc0N2c%2FoFoJXVA8cyTqJ8FphhCzvbzghmj2T%2Br5P61cxOV5xU1%2BNjVzon9Zad%2FdJRtU8%2Fkxs3uqLtVnAs9cEfslSq4law1EiCNHsRaFkXMV7qr7PRkbuKIYGBOfQJVDhv6SPmmAIyMddTp44XqWLo2gyTHJLy%2BEcwwm4%2BKyQY6pgGCoEbE5A0xozLDJklfQ9KI8sxjQOqm1YUCAwZaAeV3QhEqarrF1mmshADv5FffMc%2Fzq9kDNRbFe78RLUl%2F3lhg%2BoC4ZqaJU%2FOLiSncMSD08b5bquKPB6ZwUp9I3VP3QvYh1SoeMP0TkXeDKgO0Qx9DIRH8TKu5VBfM9Enw%2FY9ezjRgbXb3%2BaPvcc5TtWIjNTyalr0RaicYVjrsQ84hUMVQlP%2F9CfHa&X-Amz-Signature=a57aa0c6c01540b688537946fbbbd1de7a9361e7eca070641e80368934a2f75c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


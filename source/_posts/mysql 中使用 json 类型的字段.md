---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CGAHXDW%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCCtBfi4%2BHC2YOHqBTV3LsqCObGIhQqFTC62LnY6UoYrQIgTsiw9LG%2F04Rle7GqUCggeXq3UkbwcD9mZpoJlhMDtRAq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDGEjgCVrtSvOU0DDIircA5dZO96TvL8ntKR%2BoBokIr8ViFlecAmzm%2BtG2rUz%2BIZPkJ%2Berr9CP%2FL1WWpj0w3SAWzIUpeUMv%2BZrNeaONmwrSHKzc3iPCWk86m9iQvkvVnURp2Dos4AbDmPey9QwsRe2dLNUVs0c8VI74GFiSpI%2FQxcPWF%2B5vBYedNtufYOYgreL4Yk4ZI1yunD2rVhF%2BTwcrbaRiLC8Q7lSsCHWOK6yPtEhQAd13Cm%2BAwO4PDBIn5pjiS8RLVrz0VQhFyzoyt%2F74ehF71PssDiCaTHPSefPPlONEEU1Mts9rmZrCuXX%2B3RwnoEJmkDt7dpPLu9Q7dtSTiMvu6iostpeURX%2FiiJhSTbHjnEihgaIbNmKNOhjLf4sQizt1e%2F%2BY4l%2BMLr6syx%2BOkizcs8g1Wfp0F4y73cnIK00olYUzwMRNy4m4nntoFJsvPsFKpCdvFjtrQUHHg%2BgT4Gxo3hBbbxALqix5kVFiwkO2RrvOv3uRSGHq%2BqP5BwLiL8HiIdpgYdGIjcH0GLF1dKEIjz7r6OniGnRu0FFJXuAatIGmwhfP992jgPTrHfbefHoBwJ%2FCk32CC94ukI0Oby7u054tH6shE0FZ4xY85B9%2Fambqax7UZ%2Brt0L2E6UNxiUobxi48nvU16bMLjf28gGOqUBelnFTWAAgN1vUJ4c2ZeJVCCyNRNUs6FuyBWIKxRj%2FO4QEpPZOcHwWzhmhE42rTWhnQ9ciLVySsejsRlunTkav4woIvSmauP2d52K9LiND4pJzPFp3lcRiQenfbY1BOtxty2M%2BsGSIQRUOZxF8INyDftsh8b%2Bkxv9s6E%2FIkELLNpvb9zEM3wH9bdSONbqJIFiaVKCbzLRjQ%2FFPk%2F9udFEx29Ug%2Bi6&X-Amz-Signature=3728c8f7450edf51f81899ee9b1e803b34e2c67ec7e5b0137988ac4365c7cdba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


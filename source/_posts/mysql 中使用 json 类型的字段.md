---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ABEJYG7%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T130050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICaS6Bxq9Hs5LFCVBMz6SMGOhDbijVmxCQrWUF%2FmE8ssAiBelNKqbAWXmETSPGM9Q8KnX0fiL0vQakwc1eyYqbkGCCr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMQBPeyy9FKnwnP7%2BHKtwD8hj42BiTATMwAckDWuztUhFp4RVrkEErR%2BQ8avd1ojjCOMrJDQAGtJiCEhQRjargRjD3RP5U0kiRSGT25Hx87V89MkwR017Ra02A0CSCh5h5CgeUcTVCpANlI2evE%2FBYtO6jCreqFbg30XqWD3wWunkFmPBEy8Q%2Bhv1H8NZIJuRAT6LoKJD2cskQdP8Hg2ebGGRETT5S7a%2BKNpHKI00nofA3r1yd%2BiLB%2BkVpkqwXI%2Fn8M6FtDum9F5YnmwwCoZUQnRRyE%2BoudwWPWz7rzinIwDfOD3swWxig%2BpKBGNg4bLiPaA4KoTjozL06M%2F7ZYx03Ics8MQGYf0s3ppahHYfcC0n6WI8zeJNkbnOi3MyAscg6uRYpCS4GbP8nFI6NTKBXCoG1pt%2Fl8bByfMdKHEkA%2BdymCYYHHQGvUm683jRZpnCxlRUzlIAB80AKO200Fe1cuLjMToa742mG7pMAZrEzPj09gpOZCzBCWb7Cz%2FLrxFIgUrsfiCbX02cgIQqbAER66O3dog10IXR9%2Buep9l0YkKXeCgrvjNXjBpS%2F5dJEP%2BJfjepz0AZWSzClC4que1sFisAotcEBc5A%2FRl0cLeMcHuRVnnAMsU3um%2FtI%2B776UNw3gA3Ze8iNp3%2B6FBwwmYPhyAY6pgGf72n22xfZvH3xn0L9Hapyk2rsXNM2LNdwCdNdNg2MBvzC6XhuaDS6S7eVoqwyjBuu5KvY2rN4PtlB6UTMlBDuLefkZZSOl%2BuOorIyw8Cw9jD3GzaVPIMmpZJa605tiK5E7jlgZV6HpbgKUg4TGEDyqBSOdV9%2Byl772ESqyQxnnx5dYx92QNoShDANftPHVM8R%2BMyA19vyXewjmNr9TG6P%2BfJqKlYB&X-Amz-Signature=2cb11f114a4947f8677b1f417db37190ed184f785a462ede8e8a2dc401dc00e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


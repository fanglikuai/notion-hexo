---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663LTF74DR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJGMEQCIHDv3Z7CapZK0%2BYKqROdOvrfztqTNGTNBIVBE11%2BXvQzAiAwwleujgFvn43bcEPiWl7gtgJ%2BONsvy5TX33Jb%2F6VUByqIBAim%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcxDttIf5REqIe6MAKtwDWd2Mb7TJj%2F3osAlxZSLRDQI%2BJO4RK4H1qVqpR36rIDwMrCspX0pn%2FhmvvHtUa3CFQxibj%2FOOsotaHXR291bWA5RDncWkJp%2B4Ahm81rL2OXT2wFa8cjbUy5Zpa94PNkByv4KEj9UBE66cejIhRwla28LgM71BtLSFnKLN6JUAVByQd2AFCJntzcUv37O%2F0Zudpm4YCJ5pAtpkN0Auts81NrMS%2Bbxpek%2Bul1rwRPpm1BV7Xp8A42BI3iHm%2BWRE2XhE8h56CAh1TzEc3f64YsnVOlwyqqbzvPYSRXBSJK7mfx1%2FKQ44x8YqFI7c0ILIjwrnQXp37dMyMploPG3XKWWULR%2Fe0wvnrdLxzREdiShpwIPXpCfFhPERIz%2BL6v7KSorLE3t8baqVgIuZ4xTnIBiVp7zVuhBDiQqsrDkHmrbQDrr7nOQ8HK0PEXTUSOjJ0FCkeBPtw4kHehXCRqkON2GgFUb%2FzmKtMtd%2B%2BNRHeliuTolNwgwpifGJJEsaS6REGuAcRwBUGg6JaKgGyMIliKKzkiMvQDD4WStNEPSVWqt6jL5fPFZQiy8lzPvYN3%2B%2BMnVwDXbMiSA87y%2FrPs0j7XGOtP7EDPCaOdqoog2QSVhdDVzQeHLcNcDrtVO8JogwqpuUxwY6pgF5u8hXLeau2liy4clenWLf%2B05YDksHqz6iCW6az8tqtJYlGNssutf9RepT%2Fz4W%2BmZxzLUm26IROdD%2BH1wYSMCtcM8hXS5aSVXjYjbQLok3HXZhtwdZOsU8Y9AVlGN0ygS3zKumEOUNE%2BrvWWMV%2FKMrXM6CVKCYCmU7hkg7E7ysg720fgu66f%2FLhjJRb60VD0iWheBIWiVknJrRK0grGZwiQ2HELxlZ&X-Amz-Signature=e9fd4e4cac443dccec7dff62f3c38e8854a3ab04a191a54730e21aec36dd9808&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


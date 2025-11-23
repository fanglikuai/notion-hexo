---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKPAF6EM%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCciAv59mcTxFk5dUv6jJKyJa6jVekr%2B721PN4iskijvgIgS1OknThCM8g79JUtNUohrhstz7U86BHsi7droaz2cfwq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDIg7Ma2s43U8dIGVrSrcA7QCQV4RlKaahyUQyJxB%2B%2B5dyZQ9QpIco9PGGjb59509tC0WD2WIAAHzkc%2BS1r1O0%2BDaAL%2B%2FNFCDn248EI5dRgVI%2Bm0EQB9nRhSYivtpC%2FPoALwFJONadeqIDykh2Y%2FQyWZtJfCzXIfRJpbqcBmUW0zGGWLWmX8zOrGZJ6oNRc7tqh4K1Pbh%2FEs%2Bjjz59QaGH4SJX%2FNRQjQqbC88qXQI1JGVnDl5s1f28zTQQY2pNj%2FsP13B6I0EyP70Hw6f%2BF05FPA0aykxaPBEhF9kzdZXrUMzF7DMHcKIBRYrt04MKdOrAOIblVBhy4PwAZFGUoj6brzII2H%2BkgqWebzf2Mnyve68ym6ARLbQ2PGV22G1jMi4QGcPANAogkifJ%2BJWhTFJSvVS0ykYG48SJsUAzVdkqEZwNOzQUmrstF5p17kcQrG%2FVp3EnfvU1sMVIlaKQVgEURi%2BLX5n34bBvsurKsRgDAF5Z91tMfyW3rHEiV8DQJprSwHA436XXKP3sxPjEPytwrsmtwpe9P3Uj%2BESZkNC2A%2F8ph%2FaMItUVPvLIUVHx6aoxKwLbrWkYCkSPHzXWW3lATgQQnai5PjoVM5gtFMokoFbUBb83T7Zr5jNgH6aVUQu03Y00e1AJ2j8REbSMKOfickGOqUBhNTx9fBerM1B0dPpQarrnYxp9ZmaJU3ufrq1cKMUGvNj14v13STOKpi6EtwwdKs2AaAKRozoBVVsaZoWJ7nLR2R4dDn%2Fubwlrds1bxFoG9uuH%2Fzph2e7PK%2F8SwTalqI8aGNHBSJnqE%2F1ysAfgE3ZyjllhG%2F7879F%2F9H5WlAwpf1h%2FDD4sNGvTkrw%2FEMUZiPQxp2NoZLl6ZA8hECS52rN7qnEjKgx&X-Amz-Signature=666e3e970801a0637bedc9f1ce4450121cc643b73b19fc7eecfce427488b1918&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


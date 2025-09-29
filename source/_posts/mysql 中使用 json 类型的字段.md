---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TH5ROVTL%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCICSgz8mylNQv%2BKA0rzRg2x3UCqGkz2R9Zq%2BCZVhL04wRAiEAn%2FOy8lNibW8UkQ1zO6zmsGCRhhbMd6hi5bbXS6gMF90qiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGngDjpED1SK3konYyrcA65J4lO70%2Ftk33wXq1zR0%2BWJLTMyb0RAInNfNnaEf576hXjFb2MTtL0q%2B66SBMZIVXC%2B%2FeNlQhd%2FQButZUxSoZPxmHqXbtGDOG3jPzQP7dTLsuHJEp8Ht%2FHNVSX9rht4zgBH7diixfA%2FqXREBXftE7FvnO%2BM%2FxgobN6GtumMQfr46Gdlvxp9xVGaOatRXYxXArK1K7cUvq%2BuXyZqSFgKbqcmoh04kPtDFOwkGmKSHdneY27pCBr8VAvvg8FfznUZVp4FqhAEIXAVpmWa5pkbdNY6rpptUhOYxaicrxa3daMROspXQJ6ukLtUmgumnlvWB5gQVUXSdpeawfLxvhf75LRM%2BRen6MhywmocImj3L2RBbWrtwP23FAmORQ6rOdWiy3wgoSf%2Bt9pNepsA2vDq3DgqT098ADHLxqomEvbXr9V6s9ZOkmtT2lzaN45Wvwhd%2BndIdU%2FGJDI5mn5rVCHAai%2B%2BG8Sl%2BNCm%2FXtEF4LxFnFcuynoLHclJB3R%2FJuqmZwT0qYOZM5WXKVxx03QnCoF4mFInNJ3aNmv7MjSX0%2B6jBWiJscMq1L1BUrAHWfcRGBw7%2FTDEYeTNUNSPVcTbt6xHn2AvuXRrxY%2FxuXZC7mUXO2us05i1q4Yv3%2BQT1D8MJfX6cYGOqUBmJKCtOETd9CjqJIyQ1sMM4CAOU54ZU5Fy%2FFkwEhq8r1i0dIk%2BWZcK1mhCWaog%2BZ49xf%2BA7ZeN%2BR3tgahNL1hV09Nn%2FZoo%2BJRt%2Fi14XZ1yUcnNq3sfRnRG%2F%2BMxD0vB44x1QGQicImlZwD04pUiTy739CKfpg7FptYWlK%2FHgd9g%2B3cN1E22BIlU2QfcPb02K1JA66y%2BHjybR46HaHeVG1VNwhNH21x&X-Amz-Signature=1050e63e77372a566c736ae0aead660b9672a27b974da4197b10c696e0709ca0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


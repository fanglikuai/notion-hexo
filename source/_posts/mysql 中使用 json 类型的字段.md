---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHAY5CDD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIDMZYCBmk%2BSG9TnoKCDYQzaSdQkD1iJqIJRNnPsxBLjzAiEAktKzn3ZsGIx50c5Vdtg540%2FZ8%2FJhLhf5JkMw9SzXlA8qiAQI%2FP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHU7O9H9HNk3y14CxCrcA3o8jxr%2BqvTv6%2FzKr4nso8dVjRYVjqGKVenjVN39cLFdxoae4%2BHFljWSv9JWYRxp2AAz%2FrTMkCdwRxtscMleEWpGjJIJlSmogeHan8aNsqDHjqx6A8mvAd%2BYh72unhGyQcw9yYLQ5H%2FEazcDp5QwsMPRQRSQt0NblJbvC9EkphLsi0nahCbdE7x3ZIrib6IJn6dwb92WLhaIsSH23MY3U7IryAyavbCjP%2BJYL3n7z%2BLq3eDFcHf5Ij8P5aRgsLYKWuD6kB%2FhmLgpvgkjAW%2F%2BuBIK1i2dJJE7mFA0aNvGat7ODkpJAA68xE30mcaf9Iau7DcVP7lthKPrMtQrileGfUXB7Ym%2FWc29yf8u%2BAKYBoRdf8zCrq4GRxDt%2BT4vzE%2FIxI9k4ug494gVqaZS%2B2kRVPCRsppgS9kpQtYQEtFhG4%2BDeJQfJ15nMoxMB%2BFWYLdAkzaGwAOs5peDtwDHhOIxZ70xviUvtic%2Fps5InoyYxgSShp5lRahIaiIIz5%2BKiymbClQz6uAlw1VwmNikOyfqbeQX%2FYt3LvkVdtzpdN07n807m2OSQvOyTEiPtGi9lbBsgEnC61DEfoCAkIPlOqMdSaBSij1Hxs%2FAHDX%2FeLeIKB2dGhGrmLimVfUVtrtwMLjq28cGOqUBzlfHMEnKqGzxlNxKDeKrTFTCTvaRLLSJxrpckTBDDcQiCFasP72oY%2F7K16rzoOH%2BtywE%2BeK3cN6Jd4n1%2BKaRRu0Mjao6pmGerWsGRIfOH2%2BwthUuRae9aGvRwGlqAkvq3DgmSc10TJCB5HX0YlQC2sbDZEh7JZkZuKDFHXZtmUW0FPgYkfN%2BY8YG8D2W2CkdLJQnOJ5BrUXUDrDTcOyMQ6TWv1HS&X-Amz-Signature=55f5f63a344409209b93950ff6d8d3c1a7fd239509bd5f9f22b8fb22ceeb4ded&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


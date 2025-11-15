---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTXB4ILE%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T100053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAzs0ll1i1EMOqjqF8H06Ba0NYR%2Bhswp714EbexVNJzwAiBCuqfao88YOXKFTckss4n9uO14OCDm9ZAGJyLMUN3dAyr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMxcNVL7xD6K39mxbmKtwD%2BmCe%2FPVkXGyD4WXLjda1c8F8MFnkCk9rYuUQcAkO50DldmYIDYo6xSn6LG8zmrWcaeSUAgqyo1QF5NiUWkAMbikX5pBumNXOgF92b8yXWKOq3Ikd%2BkIhppk2S5JDErj0MsHDDvqmTc%2BQ0x4uOhDmwttbqaiHhA5MDDA5XsQp3%2BrzK1dwRHqNb%2Fvo%2FaypwIlDT%2FruI9AhAg4w%2Bxw%2Bpaz1KxbzoYmsy2b6nKVa9SJ6NaLUo8YX2FjJGGIHWOzojt3Onb%2BA0yclnG0ZWSHayo7Chv5pTsq5h9BfJGThAdwPVzuLxvW6h825hs%2BIgTTvZU%2BLM3DL5VsckV6luE4%2BFAYQXhHqAN%2FsSnausxIoSryKPPJQBVoaUmSogPTUAONvKUqbVbCFpZFDiyt%2Bl3G8eDdHikshbY2P2LZUdguecqSKDGse0Js%2B5jb5jO1ETBGBgK9qHE%2FYoEEtzWmVhsxK61BkEXsgXF6Lp66ZzFoypo3rK0cYrFLNbrPDkEYM8ipiv3bab9u5i63RJs6887RSMyCL3B6gG5In8cJXA3YbPDk3ZT3IkaEXgdjRLrNSTwakA1fjImKRqEAeEyA93ZEX7UIg3iniVNH8Zb7IwyRIFRGbleMFFPuPxTKM3T7CIhMwgYPhyAY6pgEGH%2FA9tjRxBmNXdOF%2BmrlpXWV%2B5aMaWLeSzyI6Ipc%2FOdw%2BwCfR%2B9gy%2B%2Ftc5Gql3D2fca9l%2FQiRxz5GHTurP8faGh2MtC6WpMMfevsUPn1NNoNeT5x5KQ6ZcLBojOsQMkm%2B%2BlJETQCiMyyCOLRg7cM6Rc2aZLLdvAVQm0l%2BtL1211BS6YMcLuGhwbSnIJ2MfX670uuzGzU4zhVPdh0QKS3bIfQE4yZ0&X-Amz-Signature=45796705bb7d5a92392eee47a4b2fb90a25957984aae68507badefdb4995d6e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


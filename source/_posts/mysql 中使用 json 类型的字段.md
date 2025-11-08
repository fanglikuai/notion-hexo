---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665M4D7AMF%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T060052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJHMEUCIE8GAcJPKlws0QS5GAmEJIDEhYAK0JUzwMAeh6ZTSnigAiEAkC6B9xhuPRLT9BEsLXmEuJit8e4aXhzM0CxEvvbmnr8qiAQIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMurD4fXq4bqCO21iCrcAyT9i7BePllSorrBMc3F79dA9Ezdi9OdCnfSzzvckjcuRr9mLOAxXPcEe0rmLkuec3%2BTuyKNANlfhRDf7vFos2awHvzu5WGwzyjEroVpvxX2m3pMnC5eljiNNBjqFoumwiZjk1HForMoXAUJRnaW7LZejMADcvyOKJ6w3MPNDYanyUW%2BQ9Hz3ld10%2BA3hCqyZnChR0ThP522fDzpOrxrQ%2BIApFqfS8FPL41EhPGvmiOEWJhbigadFpyQ8ebHxZzYAc%2B7xbXdCb683SRnH6k69qGnPicD3EmA48vOqs1D%2BRpL2DYoru7%2FXGT8EzVsLhwpWdkmxSpB%2BaeHzUY2c77MYzohhhEU9lQ7C%2FbZqDU8h1DXTi01PWO2FRA0X%2F%2B7NCePQJwPNWjB3dIU1nVkRZyC1TVrkkd485af5yCW1Yr5STBN4X6c%2B2CFtg5yvFVPDZr389ZNb6Pag27LEehjxLACcbUBNzRyAadFjo7AU3mvCbYSOLsY8SkqWsjp%2BBYodcJy9GktcejZYRudK9fwOGSSFs3ahF8XTUwMz%2FcgbXl3r4ot5oSv%2ByUXad12diGUOiv2B3p3QmVtMPEXzRQp8UQKXyjQIwqGrouxaPY2xYuYN4x8YZaEL1WlDNb63a8cMKuvu8gGOqUBhLVEPQRld3SIBNwXItgvnzEgayxm1yMpbJ8nfyxYPW4%2FN78R%2Br4q%2B6jGFgvLKhjXMkCUeDU8%2FABWdSz%2BSdBkYaqMePhIysX%2F0gL8f4r9I8t%2BaSCPTC5Gl%2BRHNre0JBvixZFH8vWi0Hi1R7QIS0Fs0ch8SdP%2F1m0OOCotEOSt9%2F8%2F2MMrxq8MLaE1FGD%2BGG%2FY5ugfBuNx%2BsV1huk8jujzQ2cwd7nl&X-Amz-Signature=d56e8028aad4d9d67de927c964afeb743959808fd50995f962b9f1152162a7c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


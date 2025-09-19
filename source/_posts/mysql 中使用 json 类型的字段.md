---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46665J7UW7D%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIBQ9j5AlF0Dh2gRVaH5CpPTJZs%2FNZT%2FoLWt7sIPTmJWiAiEA2dtZSKr%2FHAsBC1wthKyTRbuCXCIw09vTAzjyIFx2MGcqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKy3%2Bi4OrL%2FTxcPdNyrcA5%2F5vo%2BHjW3fJz8h%2BzXDL2fNfGK3pcCUTRAkYcgf1Mk73Z%2FS96s7mxoj26bPQ501V94YN5Z7fY0g%2Fc%2B8njLy35Ckz9SigpYRg%2FTMuF%2F3b8ZPc6XRgDr77J9uYnpVEUZ%2FhEYsyj4gKGwt1iE7lm4umkDpywrMSjxvU9USph4EqolWeresDCSokRmJlwmV7CYzO50PIQrs%2Fd78%2B0%2FlK7hWmg2uuaFvAKKu4RxLtuZVfzA9HiWVPm0Wqz894nZmoRjCdzzWP4bmhMHCzPTZcYmsctIVJrEAqcFTAOd8dYHOLXjOaM684CKBR0Z4uNrybsj%2Bm1X7%2F4QMP2kmpIHviZ%2BVjOl5AGsICQcAEIxBkrwM5rV5iWg1mWCWa12MpLmxNGsTgC3jDVMvMCMDNusRlk0k0KelnvdB9BD9g3%2FIwML%2F0KntIVEj%2FElJyl0TSt0d7V9D6k4OQbeifZeapi18V3JKVqZMvnadRKfX21Zt124dgwhOh2re0smcIAyzLcqGtt2KZa7%2FjVvEB2nTgPuV%2FA%2FwIDYINNmRlDUiV9DR6UJGjaS7jdJFgOPp2ZludjOtD41xFSFwbq0P%2F08bAYgf9TAqlxdUsf7aNKdLo%2BEi0WblDASS2aPs4TXZYtIrbtI7MJmHt8YGOqUBTOt2lEx1HAlJ%2F7zKYfUKWFqC6wgbELY9KTpyAMi74ehWZbu2sZkzFjS2gRuCk5lvxh53xc0FdqSiwqDEsSXz9rM7BgMqKNbaRiezkH3s%2F6yj4DuhgXNxadUmfrH90z1t5L7dGyI%2B850ErSuizp%2FYORY0lEA6RXTopYY5oCUXvBPEeNQOuq9A%2FLaS2qo3FFJHsPHFiPrH3qe2n8Z0bNK3o2H4XDGj&X-Amz-Signature=88dce7ad7c9edbe9c1b399fe29e05743ba1dadef837ffb457cd0ee9421a4c143&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


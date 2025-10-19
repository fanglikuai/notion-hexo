---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRWGXZ2X%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T070052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQD2QQi2XSipIlqPOV4rKgg%2F6uQDpkpkC67nitsiVYj%2B4AIgbFOdTgxFJJpPsAhegDfRJO%2F1liRuoTo7682WQiFVvrYqiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOsrp303ImD2VLnnoSrcA0qe7y5GBjSzwODo7RhaB0%2BJ%2BQlSgMPObk5Yjhcqm8I2mUFVPqyFE5E9NTqOEfVWxF1WUDmuH%2FTyx3Y3uilBURzZxsSN30hekJXm6hY6LaAhBR78bIM3juymxHqrmE6%2FkVZ06%2FtUPgc285fnDvc9I7xBUBBqUo7FYVz%2FH6DgZ58JgpZVVK6JFUzmR%2FUrL3SVK0BzaJOlaqCr1LCA3Cc%2FF%2FX46yU%2By2sKGasrgANhnW0Og59kB5P%2Fdlcb7Rc77f3NmXdWbhWfAzzeHXfcZZPzCIwS5aCQ9F5D2vCGHghPpCcn%2FqnNdyHaiwqD%2FaqEHQeQIv4kU8D8HA4wZFnTRplHDmEEvdR9RmfO84BBIITOrUM1bDfW%2FJsjmpJThv5wqxsbi%2BnABhkMLIaV3k4AKrey9R9i%2BwbajM%2FMnIJ%2BNd7JSguPMNYIelRZXg94mrz9mC2%2FwV7wBoLQq6RMFdHLNuJ4hNK6vkK5PaFM%2BdDzqtcNPEiNWsSUhuxQLP%2Bk%2FQoC5VXy5Jsv%2FRnxKSdFn8ZxGq%2Bri%2BfJeIWXQYJtGMwCLdtFoGJMBlVLhnl3WgsG4eAnzZh8QNxtF4kHfEAm97dfg8UvPhTq2c57f1gNdTVFfJHO6DSZSSULSug5uRY7%2FKhEMPeN0scGOqUB2ISPLraW00SWbYx%2BGf%2B3a%2BszyhLaU492BA0aQpz%2Fq3eHgfjnxBWmIwlY2SpbGz6BBV2LuWsCP8ihAH73ypW%2FrbuKFMoxzPlEHP%2F0GLVhQ1lSxh03mSErV%2FrC1UgtHKwT29ZcKggh53w0N%2BuRqdwxn92hY1rJbdexHvxNO3UDrcv%2BPHGQsAFcn1ebc5kANABh9SHwGMPwQXAy0jAI3pCVr707UZgQ&X-Amz-Signature=e6d30794b0e5a0f43b60e4b49069fae63dae8e3c9d503632c463fd1e274c9176&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


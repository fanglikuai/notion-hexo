---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FDVXZBJ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJHMEUCIQCv0qdP7TkLTbrEoQb8mTvJrLzwIT%2BzAeUvqryKdKx0SgIgEQcYJuxoHO%2B2eKGpeqP6c%2BjWGX75r6uUQmt6pH%2Bi38sq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDA5ceMwyhfSfiKpWhyrcA0en6Qw%2B9nHK83wxf9dJoJmdKcx3EtcoZINnMMRyqhcvdfT70vt3yA5UPtkkst%2BITxfE6F%2BF900UDnnjKTkvxX4ZwT8Vhi4uwa5pM7HYgNiJmXVrxfHkNMRMFghsPcmTc0tD7PBamqUN8AZoXHB2lrg01dkmZdtChR5tHoyMLt0h74PFpkp11KFWGQxp%2Fi2TE14V0ZXt7a7BFiU7Zae4xgwuBItvMR7tBTiEmimg0WLR5b4fW7WHxNF4B5XbA5kT64ntG844hPAwV7zsh4%2BHn00rm8sfm1Jinl4aoHNxOAMLSo5wCiHPE8svP0JacHQl9RUbkgcbogzjejUIX7sf%2BQIzHI5Bgfshvpkc9nUq9LH%2FHFQEO%2BAhJE3gJOUp%2FTD1Vq6rqWehSb6uEYexatboOzOpYILYMyqq59k2w%2FLbqSoQq%2BpniStMnMMs4BZEpS8EpbspEhs7X8I%2BeDMzDVzWgRgGf75ItVsSeDMgSkm2sYqvlksnTvdNiCtySC4KQ4%2B%2FO61SZn4TuLad6tdriekJpI%2FgtnljuHMTmxnpo7kurmTz08Yna5c%2Bq%2Fi8eAzMmByY1W4JUXZyaNf2UgcbF03LFflZfUyXAxdG3MaDtn62YLW7Y%2FjQ4hYe7Z0a3NE%2BML7t88YGOqUBP3uDSVFsdz%2FcrQ6etC5hfIpP%2Fz8e3CCTRce7gOkt3AdZFDWRUxHalGfmFK7T49vKwEgRzSAc9Rw4O0CzFLvWnwnPJHlL6gcER3lro6w5Y%2FuQfttcnTxDl3nKSGXXCpUcb1E7dOFG8uUA%2BrNPrM6rzLUyahnnykz7cugmMkg8xjYSJw7%2BNjla%2FuNqizfa8NpUfBZnpyPUu%2FaLnGTIGzIz0gdLRqG7&X-Amz-Signature=9e9caee29393e4a3a1dd5a625fd4065e41b6d3204561fef4318d6905ec5498fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667QPNXBEI%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJHMEUCICHC%2FYuToBfynsPsgaKtoCo39%2FVf1erW2nPdrqO1Phg3AiEA59Y4EeOIRe4t5ZL8X0J5Jw6YHRV%2BoBTvVsh5eeA8lasq%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDBrVjxcvsstuaRZJTSrcAwBh3U0WDXX0LMinLy8XLh2s88vaChChYc%2Fefs6fqsPPQWT9gVFhodkXJ78UpVYYjEUJaUzqwhRqfYP6Jv3Q%2BqyzoLHwXt4%2BOOl8ytvdOX1%2Fpzo0OkOttrwVBb93petC3nogKEmb4BX5%2B16GJqHYdqKBfqlXRap56LUT7bHizku5lXnkm65FpNns7nEp0QPnLLtMH5eokhi0W81NYyL62A9Seg5iksHp0oFtqhBRxoa8%2FrEuRZKk%2BT61nqhWyHQZbaSLC1gWViUGTXh%2BTNyoQD2FEF6%2BpV9MNQoZCuD40jm5Vvx3YObBnHSesqMXp7GRg%2FBgADcv%2Bzb9N65SbCLYbDl%2BBicdkuU7BLiapjR3qjNAUq46a%2Bd4LUdmpcUe2RUNv%2BtA1hqk%2BCXS9ufBlTGsNA2ptOZ5dOfTsrxx2unk6Gc29ZjYr5uHC8mKOwdgXGu2iGkgzfJVxg61cnwTb8CxGygGEkmkqATUSJinGkTSmX2%2BiK7nfz%2BG%2FvH55tOEZ6JG6GaoZAwlFNfYzr2Ri00Evk%2FROhE2GUhABpiGnyBBG%2F7f0AaWFjClsB%2BzbY9oWnU67EWsTWldL7EyPzha6XLD727U0XLCnTuBvuLqgZhebgE6uJ0e%2BRTwviTZf6FsMMHrjckGOqUBZ1BZOizvOcIRVfmNGswE2wgL6XbA13Wk%2B%2Bvvo4MI0tDXsBHDS%2FojrYNZwgExmOqEhQI4Ub6236TYA0M%2FiLOoiHF8mDc3A5INm5f7kD3fe5zP%2BoJ2i49qnbybi6onf1g4jbtvSUXjAtaXTAs4ZehCvgu8SWewNbM6J%2B4iFSrzwKjR44lkKJbM05BwoEL3GLb5%2BmXvHciNyNL6xZ9R%2BtQCep5pfh5h&X-Amz-Signature=d3ab26e9061881befc2c48aae12530a3c2753216cf5a5a7917e4b9c460fb5555&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

